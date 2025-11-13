Demo Transcript 

 

In the course of claim development, a tracked item which you can think of as a task on the claims to do list is created when an exam is requested for contentions on the claim, like a project with unfinished steps. 

The claim remains in open status until the tracked items are complete. 

When the exam is conducted and uploaded by the exam management system, the tracked item is automatically closed, allowing the claim to progress to the next step. 

However, when an exam is completed at a VHA facility and uploaded to claim evidence, there is no mechanism to automatically close the corresponding tract item, and as a result, a claim can sit in open status with development paused indefinitely, even though all the exam results have been received. 

Now it may seem like a simple challenge, but in reality it cannot be solved by traditional automation approaches. 

In order to match an exam result with a corresponding tracked item, the system has to, in effect, think like a VSR to understand the tracked items and DBQ documents and determine the most likely match. 

As you'll see now, that's exactly what the missing evidence smart agent system does. 

Our example has 4 open tracked items one for an ankle DBQ, a back DVQ, and exam requests for both the right and left knees. 

Taking a look at the veterans claim evidence folder, you can see the Thora columbar spine DBQ has been uploaded, but the tracked item remained open. 

Now I'll upload a couple of documents so you can see the smart system in action. 

I'll start with the ankle DBQ with content source VHA Capri and return to the tracked items to see that the receipt date has automatically been updated, which will trigger the tracked item to be closed. 

Now how about we take it up a notch and see if we can stump the smart agent with a more complicated example. 

ou see here, I have a knee DBQ with the results for both the right and left knee. 

I'll just take a quick moment to upload the DBQ. 

And remember there's no way to know from the metadata which knee it applies to. 

So let's look at the tracked items to see how the smart agent handled it. 

Wow, it updated the receipt date for both the right and left knee tracked items. 

This is because when needed, the agents can read the OCR document just like a VSR would when a match would otherwise be inconclusive. 

And finally, it is vital in an agentic system to earn a VSR's trust by providing visibility into the decision making process. 

Along with updating the tracked item, the system automatically creates a claim note logging exactly which document was determined to match which tracked item for total transparency. 

Thank you for watching this demo. 
