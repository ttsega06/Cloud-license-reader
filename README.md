# Automated "Photo Enforced Ticket" Generator
This project consists of a Windows utility for uploading captured images of license plates that were involved in fictional "traffic violations" to an Amazon Web Services S3 cloud storage bucket.
This event triggers a serverless AWS Lambda function, which utilizes AWS Textract machine learning tool for text recognition to extract the state and plate number from the image. 
If the plate is not recognized as a California plate, it is rejected to an S3 bucket for manual processing.


For California plates, this lambda function then generates a JSON based message containing infraction details that were extracted from S3 metadata.
This message is forwarded to an AWS Simple Queueing Service (SQS) queue which is continuously polled for messages by a Windows Service. 
This service queries an XML based locally stored "database" to locate the plate owner's information, which is then used to generate a JSON message that is again sent to an SQS queue.
This triggers another Lambda function to generate an infraction notice (translated into the owner's preferred language using AWS Translate) that is sent to the owner via 
AWS Simple Notification Service using their contact information that was stored in the database

# Cloud-Based/Hybrid Architecture
![SystemWorkflowDiagram](https://github.com/ttsega06/Cloud-license-reader/assets/104414973/1549126e-a19c-465b-8223-2d3ea71632cf)
