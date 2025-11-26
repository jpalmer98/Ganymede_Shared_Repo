Background 
Problem Statement 

When late flowing STRs are uploaded to a veteran's folder the VA is legally required to verify whether or not the document presents new information that could alter the consensus of a previous claim. These new documents uploaded come in large batches and a veteran’s folder can contain thousands of pages. Verifying this extensive documentation doesn’t contain any new evidence takes away valuable time from a claim’s processor. 

Sprint Objective 

Develop an agentic framework that utilizes AI to automate the inefficient process of a claims processor manually checking these documents. The agent should verify whether a newly uploaded batch of documents presents information that could alter decisions made on a veteran’s previous claims. 

Impact 

This system automates the verification of new evidence presented in late flowing STRs without requiring a claims processor to manually sift through thousands of pages. 

Key Components: 

Document Classification: In order to determine new information this system uses the document’s OCR in order to determine if a document is an STR or a DBQ. This allows the agent to decide which tool should be utilized to determine new information. 
Document Summarization: During research we discovered that documents can be over a thousand pages in length. In order to preserve the context of these documents over multiple iterations prior to directly comparing each document the agent summarizes relevant information that is crucial to whether or not a document could influence a claims decision. 
Document Comparison: The agent takes in the extensive summarization of the previous documents. With this information the agent carefully compares all relevant criteria before deciding how the EP699 claim should be handled. 
Cost Conscious: This agent was developed with efficiency at its forefront. Built into this workflow there are multiple locations of which the agent is able to identify a document does not contain new information without having to run through the entire workflow. This allows the agent to halt the workflow saving time and money through not making unnecessary calls to AWS. 
Flowchart 
 

Business Information 
 
An EP699 claim “applies to supplemental service records processing where no other applicable EP is pending". An EP699 claim is created after a Veteran’s claims are closed and a Service Treatment Record is received by the VA from the DoD in accordance with 38 C.F.R. § 3.156(c). It is mandated that the VA examine late-flowing evidence to determine whether or not that evidence, had it been available at the time of the initial rating, resulted in a higher rating. 
 
The VA currently has an automated system which, when a late-flowing document is received, creates a new EP699 claim. Upon the claim’s creation, the new document is assigned for review to a VSR who is charged with determining whether or not the new documentation provide new information not already present in any previous documentation. If there is new information, they re-open the pertinent claim for re-evaluation. If there is no new information. The EP699 claim is simply closed. 
Future Considerations 
Future Opportunities 
 
API Connections 
As this was a proof that it is possible to automate this workflow using agents, we created this workflow through connecting to dev claims API. In production it will be essential to spend time researching how to connect to the correct prod API. Some of the information necessary to pull from prod will be a veteran’s: 

Claim data:(Do we need any other information from a veteran’s previous claims?) 

It will be important to pull the previous claims a veteran has applied for in order to determine if any of the new documents are relevant. 
Newly Uploaded Documents: 

When the EP699 has been created it is important that we get each newly uploaded document as it is legally mandated that every late flowing STR is reviewed for new information. 
Previously Uploaded Documents: 

DBQs: 
The metadata pertaining to a veteran’s previously uploaded document is necessary for comparisons to any newly uploaded DBQs. 
STRs 
Texttract: The OCR automatically created when a document is uploaded to a veteran’s folder. 
Page number: This is relevant as to save tokens if a document has the same page count, we will check the first 1000 characters of their OCR’d text to determine if they are duplicates. 
ID: The document ID will be needed in order to update the claim note after the workflow has been completed. 
 
Timeline of Workflow 
At the beginning of this sprint this workflow was to run after an EP699 claim has been  created. After discussion whether or not this should be the case should be carefully discussed with VA stakeholders. Each has their own benefits and are outlined briefly below: 

After an EP699 is Created: 

Currently if new information is not identified we close an EP699 claim as NaN. This is manually done by claims processors and through the use of our agent automatically closing claims would help give back the time claims processors spent on EP699 claims as well as show the tangible benefits the VA has provided since implementing agentic solutions. 
Before an EP699 is Created: 

Nearly all EP699 claims end in no action necessary by implementing this workflow prior to any claim being created we would be able to catch that the newly uploaded document is not relevant. This would greatly reduce the overall number of created claims. 
Detailed Code Explanation: 
The following section outlines the core logic and data flow behind the agentic system. This system is designed to intelligently verify whether or not new documents present evidence that would warrant reopening a veterans claim. 

At a high level the system workflow is as follows: 

New Document Relevance Check 
First, we load in the previous claims a veteran has applied for. 
Using the contentions of the claims we compare the information present in the new documents to determine if it is relevant to the any of the previous claims. If it is relevant we continue with the workflow. If it is not relevant, we return early. 
Summarize Veteran’s folder 
Once here we now develop a summary of all of the information currently in the veteran’s folder. This summary explicitly contains information that could be relevant to the veteran’s previous claims. 
Compare New Document to summarization for new information 
Using the summary made in the previous step we now compare each of the new documents and the claim to consider through an LLM.  
The LLM evaluates all of the information provided and returns a diagnostic of whether or any new information has been returned and the workflow ends here. 
Picture 1378713485, Picture 
Pull documents from Veteran’s eFolder: 
generate_jwt_token, get_ocr_data, get_metadata, search_folder, get_annotations, upload_summary_doc 
 
Purpose: 
Form the API interaction layer for retrieving and uploading claim evidence documents. This handles token generation, authenticated data pulls (OCR text, metadata, annotations), folder-level searches, and document uploads to VBMS. 

Flow: 
generate_jwt_token() builds a JWT and identifies fields like userID and stationID, valid for one hour. It’s called internally by all other functions. 
get_ocr_data(file_id) checks if OCR text for a given file is cached; if not, it calls the claim evidence OCR endpoint (/files/{file_id}/data/ocr) and stores the JSON response in ocr_cache. 
get_metadata(file_id) retrieves general file metadata from the /data endpoint using a VBMS system token. 
search_folder(filenumber, query_body) queries a veteran’s eFolder by filenumber, returning a JSON object of matching files (just STRs or DBQs). 
get_annotations(uuid) fetches any CE annotations attached to a specific file UUID. 
upload_summary_doc(participant_id, local_file) uploads an analyzed document back into VBMS, attaching metadata such as documentTypeId and dateVaReceivedDocument, authenticating with the same JWT scheme. 
Picture 1378713485, PictureLoad and OCR the newly uploaded documents from VBMS 

Purpose: 
This section retrieves the new documents that triggered the automated EP699 workflow (usually an STR or DBQ recently uploaded to VBMS). It pulls the OCR text for each file from the Claim Evidence API so the system can later classify/analyze it against the veteran’s existing documents. 

Essentially section initializes the “new document” context by pulling its OCR’d text into memory for the subsequent comparison. 

Flow: 
Sample document object uploaded_document is defined, containing each newly uploaded document's name and unique file ID. 
Defines veterans_folder as a list of prior documents for reference. 
Calls get_ocr_data(uploaded_document["ID"]) using the API helper to fetch the OCR text layer of the uploaded document. 
Response JSON is unpacked, and the OCR text is extracted from response['currentVersion']['file']['text']. 
Extracted text becomes the raw content for all downstream checks (classification, relevance). 
Picture 1378713485, Picture 

Define which documents are part of the Veteran’s eFolder 
Purpose: 
Build a fully enriched list of documents in the veteran’s folder by pulling OCR data and metadata for each file, preparing them for later comparison, classification, and relevance checks. 
Flow: 
Iterates through each document in veterans_folder. 
For each doc, calls get_ocr_data(document["ID"]) to retrieve OCR results via the claims evidence API. 
Appends two key attributes to each document: 
page_no > total number of pages (len(document_data['currentVersion']['file']['pages'])) 
text > the full OCR-extracted text of the document. 
Performs the same enrichment for uploaded_document, adding its page_no and text. 
Returns the updated veterans_folder object containing OCR-ready content. 
 
Determine the document type 
Purpose: 
Classifies a newly uploaded document as either a STR or a DBQ and, when applicable, extracts the DBQ’s type and exam date directly from the OCR text. 

Flow: 
The helper function split_and_chunk_text() divides the OCR text into overlapping chunks to be processed by the model. 
determine_doc_type()calls Claude-Haiku (temperature=0, top_p=0). 
System prompt instructs the model to identify whether the document is an STR or DBQ, and if DBQ, to extract two key fields: 
the DBQ type 
the examination date in MM/DD/YYYY format 
The prompt explicitly enforces structured Python-dict output: 
{'STR/DBQ':'STR' or 'DBQ','type_of_dbq':'','dbq_date_of_exam':''} 
 
The function takes the first 1,000 characters of the uploaded document’s OCR text, chunks, and sends it to the model. 
 

Nate v 

 

Determine if New Document is a Duplicate 
Purpose: 
Identify whether a newly uploaded document already exists in the veteran’s folder. This node uses LLM-based semantic comparison to detect near-duplicates despite OCR noise, formatting inconsistencies, or scan distortions. STRs are compared against prior STRs using direct text analysis; DBQs are checked against historical metadata for subject and date overlap. 

Flow: 
Chunking: split_and_chunk_text() segments long OCR text into overlapping windows (1,000 characters with 100-character overlap) to maintain context and limit token usage. 
Comparison Engine: compare_documents() runs two text inputs through ChatBedrockConverse (Claude 3.5 Sonnet, deterministic params). The ruleset_prompt directs the model to determine semantic duplication, not strict text equality. 
Output Parsing: The LLM response is parsed via extract_duplicate_answer() (regex-based) to isolate "yes" or "no" following Duplicate. 
Routing: If the model’s earlier classification labeled the document as STR, each veteran folder document is compared. A "yes" triggers an early return, skipping redundant downstream nodes. 
DBQ Path: If classified as DBQ, the script prints “Document is DBQ,” extracts type_of_dbq and dbq_date_of_exam, and passes those values to the DBQ match check node. 
 

If the New Document is a DBQ, Compare the Type of DBQ to Past DBQs 
Purpose: 
 Verify whether the new DBQ already exists in the veteran’s DBQ history by comparing its subject and examination date to previously uploaded DBQs. This ensures the system does not reprocess identical DBQs, reducing cost and runtime. 

Flow: 
Input Setup: dbq_history is a list of prior DBQs containing each document’s subject, exam date, and metadata. 
LLM Evaluation: dbq_match_check() invokes ChatBedrockConverse (Claude 3.5 Sonnet, temperature=0) with a ruleset_prompt describing how to determine duplication. 
Similarity Logic: 
If the new DBQ’s subject and exam date match any entry > "DBQ already exists". 
If the subject matches but the date differs > "New DBQ". 
Output: The function outputs exactly "DBQ already exists" or "New DBQ". The result determines whether to continue to the claim relevancy evaluation. 
 

Is the Uploaded Document Relevant to One of the Veteran’s Previous Claims? 
Purpose: 
Assess whether the uploaded STR or DBQ pertains to any of the veteran’s prior claims. This step ensures that only claim-relevant documents proceed to materiality analysis, filtering out unrelated uploads. 

Flow: 
OCR Cleaning: fix_OCR() uses an LLM to correct OCR artifacts without changing the document’s meaning. 
Data Inputs: testing_dictionary represents historical claims (claim number, contention, closure date). 
Claim Comparison: determine_if_relevant() compares the uploaded document’s text to each claim entry using a structured prompt. It labels each relationship as "Guaranteed Relationship" or "No Relationship". 
Temporal Validation: If the document includes a dbq_examination_date, the model determines "dbq_date_check" = "Pass" if the date precedes the claim’s closure, "Fail" otherwise. 
Structured Output: Returns a list of JSON objects detailing claim data, relationship, date check, and rationale. 
Result Parsing: extract_claims_list() converts the stringified JSON output into a Python list for downstream processing. 
 

Process the Output of the Relevancy Node 
Purpose: 
Transform the model’s relevancy output into a unified pass/fail determination with reasoning and claim linkage. This simplifies downstream logic and determines whether materiality analysis should proceed. 

Flow: 
Input: Receives the list of structured claim outputs from extract_claims_list(). 
Classification: determine_output() loops through each claim result, categorizing as relevant or irrelevant based on relationship and dbq_date_check. 
Fail Conditions: 
If all claims return "No Relationship" or "Fail", output "Fail" with contextual reasoning (e.g., claim closed before exam date). 
Pass Condition: 
If at least one claim yields "Guaranteed Relationship" and passes date validation, output "Pass" and return those matched claims. 
Final Object: Returns a JSON dictionary: 
"determination": "Pass/Fail" 
"reasoning": "[summary]" 
"matched_claims": [list of relevant claims or ""]  
 

If the Uploaded Document is Relevant to a Claim, Is There New Information in the Document? 
Purpose: 
 Determine whether the new STR introduces material evidence that could alter a claim decision. This involves summarizing prior STRs, evaluating the new STR in that context, and producing a model-based decision on reopening the claim. 

Flow: 
Summarization: 
summarize_all_documents() iterates through prior OCR’d STRs. 
Each STR is summarized using ChatBedrockConverse (Claude 3.5 Sonnet, deterministic) with a strict JSON schema for diagnoses, symptoms, treatments, functional impact, and timeline. 
Comparison: 
evaluate_new_vs_old_strs() constructs a structured prompt that includes claim metadata (claim number, contention, relationship, and summary). 
The model compares summarized prior STRs vs. the new STR and outputs JSON with: 
Recommendation ("Increase" / "Close") 
Change and Claim Note Update 
Overall Rationale and Original STR Synopsis. 
Result Assembly: Returns a list of claim evaluations under evaluations['STRs']. 
 

Construct Claim Notes and Final Summary Output 
Purpose: 
 Generate structured claim notes and formatted outputs summarizing the agent’s final decision. Notes are designed to be directly ingestible by VBMS and human-readable for claims processors. 

Flow: 
Claim Note Construction: Iterates through each entry in evaluations. 
If Recommendation = "Increase", constructs a VBMS permanent note recommending reopening. 
Otherwise, constructs a note recommending closure. 
Both paths attach narrative text summarizing new evidence and system recommendation. 
Note Structure: Each claim note inherits from base_claim_note template fields: 
 extItemId, noteType, systemGenerated, etc. 
Final Summary Output: 
Builds summary_output as a list of dictionaries containing: 
"Document ID" (placeholder ID) 
"Notes" (short summary) 
"Narrative" (detailed rationale, if applicable). 
Output JSON is concise and UI-ready for visualization or storage. 
