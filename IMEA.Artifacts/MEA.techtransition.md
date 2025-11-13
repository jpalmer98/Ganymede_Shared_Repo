Background 

Problem Statement 

Claim processors currently lack an efficient method to link open tracked items with uploaded documents. As a result, tracked items are often left in an open state even after the necessary documents have been uploaded. This leads to manual review, comparing the contents of uploaded files to match with open tracked items. 

 

Sprint Objective 

This sprint aims to develop a solution which automatically updates the received date of a tracked item when a document, containing sufficient evidence to match with an open tracked item, is uploaded. This evidence will be based upon the metadata of the document and, if needed, the content of the document in order to determine if it matches an open tracked item. 

Impact 

This system automates a claim’s received date for a tracked item, eliminating the need for manual intervention. 

Key Components: 

Metadata semantic filtering: Feeds an uploaded document’s metadata through a large language model in order to determine if it relates to an open tracked item or if further analysis is required. 

Content based filtering: If metadata semantic filtering proves to not yield sufficient evidence to match a document to an open tracked item, Amazon Bedrock Data Automation’s (BDA) multi-modal large language models OCRs the uploaded document. The output of that OCR is then compared to the open tracked items to determine if we are able to confidently match the two. 

 

Flowchart 

Picture 

 

Business Information 

Missing Evidence will help prevent claims from sitting idle when in reality they possess enough information to be worked on. Through the use of AI-based OCR tools coupled with intelligent comparison analysis Missing Evidence enables uploaded documents to automatically update received dates for open tracked items, saving claims processors and Veterans valuable time.s 

Note: this is only a small portion of the entire Missing Evidence problem. Originally, we took the approach of identifying forms and documentation needed for each claim type and then assess whether these forms were completed in the Veteran’s claim evidence folder. This code is in the missing-evidence branch as well, if needed  down the road. 

Limitations 

API Connection  

In order to make progress on this project it will be essential to use the correct API connections to automatically know when a new document is uploaded as well as connect to VBMS to add a received date when a tracked item is matched. 

Tracked Items API 

https://trackeditem-api-test.dev.bip.va.gov/index.html#/ 

There are two existing PUT endpoints, but they are not exactly what we want. The first is to update the follow up date, and the second is to close a tracked item by updating the “Closed Date” field. We will technically need to update the “Received Date” field and not the “Closed Date,” so a new PUT endpoint will need to be added. John Lloyd is the POC for this effort and what it entails to create this. 

curl -X 'PUT' \ 
  'https://trackeditem-api-test.dev.bip.va.gov/api/v1/trackeditemapi/updateTrackedItemFollowupDate' \ 
  -H 'accept: application/json' \ 
  -H 'Content-Type: application/json' \ 
  -d '{ 
  "claimId": "600393620", 
  "trackedItemMap": { 
    "371202": [ 
      "2023-12-18", 
      "2023-12-25" 
    ] 
  } 
}' 

curl -X 'PUT' \ 
  'https://trackeditem-api-test.dev.bip.va.gov/api/v1/trackeditemapi/close' \ 
  -H 'accept: application/json' \ 
  -H 'Content-Type: application/json' \ 
  -d '{ 
  "claimId": "600405279", 
  "trackedItemId": "373088", 
  "suspenseDate": "2025-01-01" 
}' 

 

Claim Notes API:  

https://cfapi-dev.dev.bip.va.gov/swagger-ui.html#/Notes/getNotesByClaimId 

This endpoint will be used to POST new claim note using curl request similar to below: 

curl -X 'POST' \ 'https://cfapi-dev.dev.bip.va.gov/api/v1/notes' \ -H 'accept: application/json' \ -H 'Content-Type: application/json' \ -d '{ "notes": [ { "subject": "Test Temporary Claim Note 1", "actionTaken": "testAction", "narrative": "testSubject", "extItemId": 600177121, "extItemType": "CLAIM", "realm": "VBA", "noteType": "TEMPORARY" }, { "subject": "Test Temporary Claim Note 2", "narrative": "testSubject2", "extItemId": 600177121, "extItemType": "CLAIM", "realm": "VBA", "jpaVersion": 1, "noteType": "TEMPORARY" } ] }' 

Metrics 

In order to determine whether an uploaded document was received we use the metadata included in the uploaded document. If that proves to be insufficient, we then OCR the document through Amazon’s Bedrock Data Automation (BDA) and use the text from the OCR to run an additional check to determine if the uploaded document matches an open tracked item. 

 

Metadata Analysis 

When a document is uploaded, its metadata is stored as a JSON file which we will use for the first similarity check. The purpose of this check is to potentially save computation time and resources by determining if the LLM is confident that there is or is not a match using just the metadata from the uploaded document. The following fields are gathered from the uploaded document: ‘documentTypeId’, ‘contentSource’, ‘subject’, ‘name’, ‘description’, and ‘contentName’. With the given metadata, we feed it to an LLM to see if it is similar to the title of an open tracked item. If the LLM outputs either ‘No Relationship’ or ‘Guaranteed Relationship’, we can avoid passing the document into the OCR step and thus saving resources. However, if the model outputs ‘Some Relationship’, we need to gather more evidence before making a determination. The workflow would then move on to the next the step, OCR Analysis. 

OCR Analysis 

If the uploaded document passed through the metadata-based check and the LLM determined there was ‘Some Relationship’, meaning there was not a definitive yes/no match between the uploaded document and an open tracked item, we OCR the uploaded document to retrieve its content as a text file. Using a different LLM check, we compare the content of the document from the OCR with the Veteran’s open tracked items to see if we can make a confident determination if there is a match or not. If the LLM outputs ‘No Relationship’ or ‘Some Relationship’, the workflow ends. If the LLM outputs ‘Guaranteed Relationship’, the workflow produces a final output. 

Detailed Code Explanation: 

This section outlines each step in the workflow to ultimately determine if an uploaded document corresponds to any of the Veteran’s open tracked items. 

Load data into the workflow 

For demo purposes, we used dummy data which mimics pulling data using the Claims API. In production, this workflow would be directly connected to the Claims API. Whenever a Veteran uploads a document, the workflow would be triggered to compare the document to the open tracked items 

Apply Content Source and Document Type filters 

Before using any resources by calling LLMs, we want to run some preliminary checks on the uploaded document. These checks can potentially save computation time and resources by only running LLMs when necessary. Thw workflow should only run if the uploaded document’s metadata contains Content Source = VHA_CAPRI and Document Type = C&P Exam. Note that another way of filtering on Document Type = C&P Exam is by using DocumentTypeId = 356.  

Does the Veteran have any open claims? 

Again, before running any LLMs, we use the VetaranID to check if the Veteran has at least one open claim. If they don’t, the workflow ends and no output is produced. 

Do those open claims have any open tracked items? 

The last preliminary check is ensuring that there are open tracked items associated with the open claims. If there are open tracked items, return a list of dictionaries where each dictionary contains the relevant open tracked items data needed in future steps. If there are no open tracked items, the process ends and no output is produced. 

Compare the document’s metadata with the open tracked items 

If all the checks before this step are satisfied, we use an LLM to compare the document’s metadata with all the open tracked items to determine which open tracked item(s), if any, correspond to the uploaded document. If the LLM determines there is an open tracked item that matches the open tracked item, it outputs a label representing the strength of match between the uploaded document and the open tracked item. The three possible labels are ‘No Relationship’, ‘Some Relationship’, and ‘Guaranteed Relationship’. If the model outputs ‘No Relationship’, the workflow ends and no output is produced. If the model outputs, ‘Some Relationship’, we move on to the next step in the workflow to gather more details about the document before making a determination. If the model outputs ‘Guaranteed Relationship’, we don’t want to waste resources by running additional checks, so we skip to the final output step in the workflow using the identified open tracked item. 

If ‘Some Relationship’ determination is made, OCR the document and compare the content of the document with the open tracked items 

If the previous step determines there is ‘Some Relationship’ with at least one of the open tracked items, we OCR the uploaded document to more accurately make a determination if it corresponds to the open tracked item. After the OCR, we perform essentially the same LLM check as the previous step but comparing the OCR of the document, instead of the document metadata, with the list of open tracked items. Again, the LLM then outputs ‘No Relationship’, ‘Some Relationship’, or ‘Guaranteed Relationship’. However, ‘Guaranteed Relationship’ is the only classification which results in identifying an open tracked item in the final output. If the LLM outputs either ‘No Relationship’ or ‘Some Relationship’, the workflow ends with no final output produced. 

Format Final Output 

Lastly, we only produce a final output if the model determines there is match between the uploaded document and an open tracked item. Again, there is no output produced if there is not a match. Therefore, the workflow must output ‘Guaranteed Relationship’ in either the document metadata LLM check or the OCR LLM check to produce a final output. Assuming there was a match identified in one of these, we output the data associated with the matched open tracked along with a ‘received_date’ which corresponds to the date the document was uploaded. We also output a claim note in which we state what actions were taken. For example, the ‘narrative’ value in the claim note states, “The uploaded document, “[Document File Name]”, satisfied the following open tracked item: [Open Tracked Item Data]. As a result, the tracked item’s received date has been updated to [Date of Document Upload].” 

 

Amazon Bedrock Data Automation (BDA) Walkthrough 

 

Purpose 

Amazon’s new tool, BDA, is a highly capable OCR tool that leverages multi-modal models on top of its pre-existing OCR tool Textract. Through using it, our team has discovered a significant increase in accuracy of the text returned by BDA compared to alternative OCR options. When accuracy of a document’s OCR is highly important it should be the go-to tool and is great to have in your toolbox of AWS technologies. 

Technical overview 

The BDA (Bedrock Data Automation) process is implemented via the OCR_node.py module and orchestrated from the main notebook. Here's how it works: 

File Input: The main notebook passes the filename to the OCR() function in OCR_node.py. 

OCR Function: 

Loads the document from the specified path. 

Checks if the file has already been processed by BDA using: 

if os.path.isfile(fullpath) 

(To reprocess a file, this condition must be bypassed or modified.) 

Global Paths: Input and output paths are defined via global variables in OCR_node.py: 

doc_input_path = ... 

doc_output_path = ... 
 

BDA_OCR Function: 

Called internally by OCR() after the file check. 

Uploads the document to an S3 bucket (required for BDA). 

Executes Amazon’s Bedrock Data Automation. 

Returns the path to the processed JSON output. 

Output Handling: 

OCR() uses the returned path to load the JSON. 

The result is stored in a temporary variable and printed to the designated output location. 

Looking forward 

While BDA is a vital tool that has a variety of use cases, it is necessary to consider the following in production: 

Cost 

BDA gives better results but costs a penny per page passed through (as of 9/29/2025). In production it will be necessary to consider cases where Amazon’s Textract is sufficient in order to save money.  

For example, a document classification node could be implemented, prior to running the workflow. Prior to the OCR node, it would determine if document document warrants using BDA or now. Handwritten STR documents would be a good example of when to use BDA whereas standard VA forms are likely good candidates for Amazon Textract 

Currently we run a preliminart check using just the metadata to determine if an uploaded document matches a tracked item. This allows us to only use BDA only when necessary to conserve resources. 

Time 

BDA can take longer to process some documents as it must upload them to an S3 bucket among process it and finally output it to where we can use it. It is necessary to consider the time it takes BDA to process a document when implementing software that may need a very low latency. 

Credentials 

Currently the credentials in BDA_OCR use the Innovation lab’s credentials. In order to run BDA, you will need to get access to your team’s profile ARN. Once you have this you will need to create a project in BDA to get your project ARN. The best way to create a project is in the Amazon tutorial below. 

Amazon’s tutorial 

The best way to get started with Amazon Bedrock Data Automation is their online GitHub tutorial. It is essential to at least locally walk through their first tutorial in order to understand how to setup your own BDA project. 
