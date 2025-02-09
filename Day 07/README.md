# **Game Day Notifications / Sports Alerts System**

## **Project Overview**  
This project is a real-time NBA game day notification system that sends live score updates to subscribed users via SMS or email. It utilizes **Amazon SNS**, **AWS Lambda with Python**, **Amazon EventBridge**, and **NBA APIs** to provide up-to-date game alerts. Additionally, it demonstrates **Infrastructure as Code (IaC)** by using **Terraform**, allowing for seamless deployment and teardown of the solution within seconds.  

---

## **Key Features**  
- Retrieves live NBA game scores from an external API.  
- Delivers formatted score updates to subscribers via **Amazon SNS** (SMS/Email).  
- Automates scheduled updates using **Amazon EventBridge**.  
- Implements **IAM least privilege** principles to enhance security.  

---

## **Prerequisites**  
To set up and run this project, you will need:  
- A free account and API key from [SportsData.io](https://sportsdata.io/)  
- An AWS account with a basic understanding of **AWS and Python**  
- AWS CLI installed and configured  
- Terraform CLI (v1.10.5) installed on your local machine  

---

## **Technical Architecture**  
![nba_API](https://github.com/user-attachments/assets/5e19635e-0685-4c07-9601-330f7d1231f9)  

This system consists of the following AWS services:  
- **Amazon EventBridge** – Triggers the notification system at scheduled intervals.  
- **AWS Lambda** – Fetches live NBA scores and processes notifications.  
- **Amazon SNS** – Sends game score alerts via SMS or email to subscribed users.  
- **AWS Systems Manager (SSM)** – Securely stores the API key for authentication.  

---

## **Technology Stack**  
- **Cloud Provider**: AWS  
- **Infrastructure as Code**: Terraform  
- **Core AWS Services**: SNS, Lambda, EventBridge  
- **External API**: NBA Game API (SportsData.io)  
- **Programming Language**: Python 3.x  
- **Security**: IAM with least privilege policies for all services  

---

## **Project Structure**  
```bash
.
├── nba_notifications.py         # Lambda function to fetch and send notifications
├── nba_notifications.zip        # Zipped Lambda function
├── game_day_notifications.tf    # Terraform configuration file
├── LICENSE                     
├── .gitignore
└── README.md                    # Documentation
```

---

## **Setup Instructions**  

### **1. Clone the Repository**  
```bash
git clone https://github.com/Ore-stack/30-days-DevOps-Challenge.git
cd Day 07
```  

### **2. Store the API Key Securely in AWS Parameter Store**  
Run the following AWS CLI command to securely store your NBA API key:  
```bash
aws ssm put-parameter --name "nba-api-key" --value "<API_KEY>" --type "SecureString"
```  

### **3. Deploy Using Terraform**  

#### **Initialize Terraform**  
```bash
terraform init
```  
This command sets up the Terraform environment and installs necessary provider plugins.  

#### **Format Configuration Files**  
```bash
terraform fmt
```  
Ensures your Terraform files are properly formatted and follow best practices.  

#### **Validate Configuration**  
```bash
terraform validate
```  
Checks for syntax errors and ensures your configuration is correct.  

#### **Preview Changes**  
```bash
terraform plan
```  
Displays a summary of the infrastructure changes Terraform will apply.  

#### **Deploy the Infrastructure**  
```bash
terraform apply
```  
Creates or updates the AWS resources as defined in the Terraform configuration.  

#### **Destroy the Infrastructure (When No Longer Needed)**  
```bash
terraform destroy
```  
Deletes all resources provisioned by Terraform.  

---

## **4. Subscribe to Notifications via SNS**  

1. Navigate to the **SNS Console** in AWS.  
2. Select the **SNS topic** created by Terraform.  
3. Click on **Create Subscription**.  
4. Choose a **protocol**:  
   - **Email**: Enter a valid email address.  
   - **SMS**: Enter a valid phone number in international format (e.g., +1234567890).  
5. Click **Create Subscription**.  
6. If using email, confirm your subscription by clicking the link sent to your inbox.  

---

## **5. Test the Notification System**  

1. Open the **AWS Lambda Console**.  
2. Select the deployed Lambda function.  
3. Create a test event and execute it.  
4. Check **CloudWatch Logs** for any errors.  
5. Confirm that SMS/email notifications are received by subscribers.  

---

## **Lessons Learned**  
Through this project, we gained experience in:  
✅ Designing a real-time notification system with **AWS SNS and Lambda**.  
✅ Implementing **IAM least privilege** security policies.  
✅ Automating workflows using **Amazon EventBridge**.  
✅ Integrating **external APIs** with cloud-based workflows.  
✅ Deploying infrastructure efficiently with **Terraform**.  

