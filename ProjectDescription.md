# Serverless Customer Feedback Application on AWS

## 1. Project Overview

This project demonstrates the design and deployment of a serverless, event-driven web application on Amazon Web Services (AWS) to collect, process, and store customer product feedback.

The solution replaces traditional server-based infrastructure, such as EC2-hosted web servers and relational databases, with fully managed AWS services.

The application uses **Amazon S3, AWS Lambda, Amazon DynamoDB, and AWS IAM** to provide a scalable, cost-efficient, and low-maintenance architecture.

Customers can submit product feedback through a web-based form, which is processed by an AWS Lambda function and stored in an Amazon DynamoDB table without requiring dedicated server infrastructure.

## 2. Project Objectives

- Design and deploy a serverless customer feedback application.
- Host the frontend using Amazon S3 Static Website Hosting.
- Develop a serverless backend using AWS Lambda and Node.js.
- Configure a Lambda Function URL to receive HTTP POST requests.
- Store customer feedback records in Amazon DynamoDB.
- Implement secure service-to-service communication using AWS IAM.
- Apply least-privilege permissions to the Lambda execution role.
- Configure Cross-Origin Resource Sharing (CORS) to enable frontend-backend communication.
- Test and verify successful feedback submission and database storage.

## 3. AWS Services and Technologies Used

| AWS Service / Technology | Purpose |
|---|---|
| Amazon S3 | Hosts the static frontend application (HTML, CSS and JavaScript). |
| AWS Lambda | Executes backend business logic without managing servers. |
| Lambda Function URL | Provides an HTTPS endpoint for receiving feedback submissions. |
| Amazon DynamoDB | Stores customer feedback records in a NoSQL database. |
| AWS IAM | Manages permissions and secures access between AWS services. |
| Node.js | Provides the runtime environment for the Lambda function. |
| JavaScript | Handles frontend interactions and backend processing. |
| HTML and CSS | Build and style the customer feedback interface. |
| Fetch API | Sends asynchronous HTTP requests from the frontend to Lambda. |

## 4. Solution Architecture

The application follows a serverless architecture in which the frontend, backend processing, and database storage are handled by managed AWS services.

### Architecture Workflow

1. **Client Browser:** Customers access the feedback application through a web browser.
2. **Amazon S3:** Serves the static frontend application containing HTML, CSS, and JavaScript files.
3. **AWS Lambda Function URL:** Receives HTTP POST requests containing customer feedback in JSON format.
4. **AWS Lambda:** Processes the incoming request, validates and extracts the submitted information, and prepares the database record.
5. **AWS IAM Execution Role:** Provides the Lambda function with the necessary permissions to write records to DynamoDB.
6. **Amazon DynamoDB:** Stores the submitted feedback, including unique identifiers, timestamps, product names, ratings, and comments.

### Application Data Flow

```text
              Customer
                 |
                 v
          Client Browser
                 |
          +------+------+
          |             |
          v             v
     Amazon S3     HTTP POST Request
     (Frontend)          |
                         v
                Lambda Function URL
                         |
                         v
                  AWS Lambda
                 (Node.js)
                         |
                         v
                 AWS IAM Role
                         |
                         v
                    DynamoDB
                (ProductFeedback)
                         |
                         v
                 Feedback Stored
```

## 5. Implementation and Deployment

### Phase 1: Database Layer – Amazon DynamoDB

Created an Amazon DynamoDB table to store customer feedback records.

**Table Configuration:**

| Parameter | Configuration |
|---|---|
| Table Name | `ProductFeedback` |
| Partition Key | `FeedbackId` |
| Partition Key Type | String |
| Database Type | NoSQL |
| Storage | Customer feedback records |

**Data Stored:**

Each feedback record contains:

- `FeedbackId` – Unique identifier for each submission.
- `Timestamp` – Date and time of submission.
- `ProductName` – Name of the product being reviewed.
- `Rating` – Customer-provided product rating.
- `Comment` – Customer feedback or review.

**Outcome:** Established a scalable NoSQL database to persist customer feedback records.

### Phase 2: Backend Development – AWS Lambda

Created a Lambda function to process incoming customer feedback submissions.

**Lambda Configuration:**

| Parameter | Configuration |
|---|---|
| Function Name | `SubmitFeedbackFunction` |
| Runtime | Node.js |
| Compute Service | AWS Lambda |
| Database Integration | Amazon DynamoDB |
| Database Operation | `PutItemCommand` |
| Trigger / API | Lambda Function URL |

**Implementation Steps:**

1. Created the Lambda function named `SubmitFeedbackFunction`.
2. Selected the Node.js runtime.
3. Developed JavaScript backend logic to process incoming HTTP requests.
4. Parsed the JSON payload received from the frontend.
5. Generated a unique feedback identifier and submission timestamp.
6. Used the AWS SDK for JavaScript and `PutItemCommand` to insert feedback records into DynamoDB.
7. Configured the Lambda function to return an appropriate HTTP response to the frontend.

**Lambda Function URL Configuration:**

A public HTTPS Function URL was configured to allow the frontend application to submit feedback directly to the backend.

CORS settings:

| Parameter | Configuration |
|---|---|
| Allowed Origins | `*` |
| Allowed Methods | `*` |
| Allowed Headers | `content-type` |

**Outcome:** Established a serverless backend capable of receiving, processing, and storing customer feedback without provisioning or managing EC2 instances.

### Phase 3: Security Configuration – AWS IAM

Implemented role-based permissions to allow secure communication between AWS Lambda and DynamoDB.

**Implementation Steps:**

1. Created an IAM execution role for the Lambda function.
2. Configured the role with the appropriate Lambda execution permissions.
3. Created an inline IAM policy granting access to the DynamoDB table.
4. Restricted database permissions to the `dynamodb:PutItem` action.
5. Scoped the permission to the `ProductFeedback` table ARN.
6. Attached the execution role to the Lambda function.

**IAM Permission Configuration:**

| Parameter | Configuration |
|---|---|
| Principal | Lambda Execution Role |
| AWS Service | Amazon DynamoDB |
| Allowed Action | `dynamodb:PutItem` |
| Resource | `ProductFeedback` Table ARN |
| Access Model | Least-Privilege Permissions |

**Outcome:** Established controlled service-to-service access, allowing Lambda to insert feedback records without granting unnecessary database permissions.

### Phase 4: Frontend Development and Hosting – Amazon S3

Created a static web application to provide customers with an interface for submitting product feedback.

**Frontend Components:**

- HTML – Defines the feedback form structure.
- CSS – Provides styling and layout.
- JavaScript – Handles form submissions and backend communication.
- Fetch API – Sends asynchronous HTTP POST requests to the Lambda Function URL.

**Implementation Steps:**

1. Created an Amazon S3 bucket for frontend hosting.
2. Enabled S3 Static Website Hosting.
3. Configured the required public read permissions using `s3:GetObject`.
4. Developed the frontend application using HTML, CSS, and JavaScript.
5. Added a customer feedback form to collect product names, ratings, and comments.
6. Configured the JavaScript `fetch()` API to send JSON payloads to the Lambda Function URL.
7. Uploaded the frontend files to the S3 bucket.
8. Accessed the application through the S3 static website endpoint.

**Outcome:** Successfully deployed a publicly accessible frontend application that communicates with the serverless backend.

### Phase 5: CORS Configuration and Integration Testing

During initial testing, browser security restrictions caused Cross-Origin Resource Sharing (CORS) errors when the frontend attempted to communicate with the Lambda Function URL.

**Resolution:**

1. Identified the CORS issue affecting frontend-backend communication.
2. Configured CORS settings on the Lambda Function URL.
3. Allowed the required origin, HTTP methods, and content-type header.
4. Retested the frontend application.
5. Verified successful HTTP requests and database integration.

**Outcome:** Resolved browser cross-origin communication issues and established successful communication between the S3-hosted frontend and Lambda backend.

## 6. Testing and Verification

The application was tested end-to-end to verify successful customer feedback submission and database persistence.

### Test 1: Frontend Application

- Opened the live S3-hosted website.
- Entered product details, ratings, and comments.
- Submitted the feedback form.
- Verified that the form processed submissions without requiring a page reload.

### Test 2: Backend Processing

- Verified that the Lambda Function URL received HTTP POST requests.
- Confirmed that the Lambda function processed the JSON payload.
- Checked that the function executed the DynamoDB `PutItemCommand` operation.

### Test 3: DynamoDB Data Persistence

- Opened the Amazon DynamoDB console.
- Navigated to the `ProductFeedback` table.
- Verified that submitted feedback records were successfully stored.
- Checked the generated feedback IDs, timestamps, product names, ratings, and comments.

**Example Feedback Record:**

```json
{
  "FeedbackId": "FB-1773849600000",
  "Timestamp": "2026-03-18T00:00:00.000Z",
  "ProductName": "Example Product",
  "Rating": 5,
  "Comment": "Excellent product and service."
}
```

*Example record structure for illustration.*

### Test 4: CORS and Application Integration

- Verified successful communication between the S3 frontend and Lambda Function URL.
- Confirmed that browser CORS restrictions were resolved.
- Tested multiple feedback submissions.
- Verified that records were persisted in DynamoDB.

## 7. Project Outcomes and Key Achievements

The project successfully demonstrated the implementation of a fully serverless customer feedback application using AWS managed services.

Key achievements include:

- **Serverless Architecture:** Developed an application without provisioning or maintaining EC2 web servers or relational database infrastructure.
- **Static Website Hosting:** Deployed a customer-facing frontend using Amazon S3.
- **Serverless Backend:** Implemented backend processing using AWS Lambda and Node.js.
- **API Integration:** Configured a Lambda Function URL to receive HTTP POST requests.
- **NoSQL Database Integration:** Successfully stored customer feedback in Amazon DynamoDB.
- **Security:** Implemented IAM execution roles with least-privilege DynamoDB permissions.
- **CORS Configuration:** Resolved cross-origin communication issues between the frontend and backend.
- **End-to-End Testing:** Verified successful form submissions, backend processing, and database persistence.
- **Cost Efficiency:** Used managed, on-demand services to avoid maintaining continuously running application servers.

## 8. Key Skills Demonstrated

- AWS Serverless Architecture
- Amazon S3 Static Website Hosting
- AWS Lambda and Node.js
- Lambda Function URLs
- Amazon DynamoDB
- AWS IAM Roles and Policies
- Least-Privilege Access Management
- AWS SDK for JavaScript
- DynamoDB `PutItemCommand`
- REST-style HTTP Integration
- JavaScript Fetch API
- JSON Data Processing
- CORS Configuration
- Frontend and Backend Integration
- Cloud Application Deployment
- Application Testing and Troubleshooting

## 9. Conclusion

This project provided practical experience in designing and deploying a serverless, event-driven application using AWS cloud services.

By integrating Amazon S3, AWS Lambda, Amazon DynamoDB, and AWS IAM, the solution demonstrated how managed cloud services can eliminate traditional server-management requirements while providing scalable application processing and persistent data storage.

The project also strengthened practical skills in serverless application development, IAM security, API integration, CORS configuration, and end-to-end cloud application troubleshooting.

---

**Technology Stack:** AWS | Amazon S3 | AWS Lambda | Lambda Function URL | Amazon DynamoDB | AWS IAM | Node.js | JavaScript | HTML | CSS
