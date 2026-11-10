Serverless Customer Feedback Application

Executive Summary -
This project demonstrates the design and deployment of a fully serverless, event-driven web application to collect and persist customer product feedback on AWS. By moving away from traditional infrastructure (such as provisioning EC2 web servers and relational RDS databases), this solution relies entirely on managed cloud services—Amazon S3, AWS Lambda, AWS IAM, and Amazon DynamoDB. The architecture automatically handles capacity planning, high availability, fault tolerance, and security while maintaining a zero-maintenance footprint when idle.

Key AWS Services Used -
•	Amazon S3: Hosts the static single-page application (index.html) using S3 Static Website Hosting.
•	AWS Lambda: Serves as the serverless compute layer. It processes incoming HTTP requests via a Function URL and executes backend JavaScript business logic.
•	AWS IAM: Provides fine-grained execution security by granting the Lambda function least-privilege dynamodb:PutItem access.
•	Amazon DynamoDB: Acts as the scalable, low-latency NoSQL database to store structured feedback records.

Implementation Summary -
1.	Database Layer (DynamoDB): Created the ProductFeedback table with a String partition key (FeedbackId) to store dynamic keys, submission timestamps, product names, ratings, and user comments.
2.	Backend Compute & API Layer (Lambda & Function URL):
o	Configured a Node.js runtime Lambda function (SubmitFeedbackFunction).
o	Configured a public Lambda Function URL with native Cross-Origin Resource Sharing (CORS) handling (AllowOrigins: *, AllowMethods: *, AllowHeaders: content-type) to accept submissions safely from web clients.
o	Wrote function logic to parse the payload and execute PutItemCommand against DynamoDB.
3.	Security Policy (IAM): Created an execution role for Lambda containing an inline IAM policy limited exclusively to dynamodb:PutItem operations on the ProductFeedback table resource ARN.
4.	Frontend Hosting (S3): Configured an S3 bucket with public read access (s3:GetObject) and static website hosting enabled. Developed an HTML/JS frontend that submits form payloads to the Lambda Function URL via asynchronous fetch() API calls.

Key Results & Verification -
•	Smooth User Experience: Tested the feedback form on the live S3 website to make sure everything worked seamlessly from a user's perspective. Submitting the form instantly sends the feedback to the backend without reloading the page or slowing down.
•	Reliable Data Storage: Checked the DynamoDB database console to confirm that every submission was saved accurately. Each entry automatically received a unique ID (like FB-1773849600000), a timestamp, and all the details entered by the user.
•	Fixed Web Security Barriers: Resolved initial browser security blocks (CORS errors) by configuring AWS Lambda to handle web permissions cleanly. This ensured the browser could talk to the database safely without throwing error messages.

