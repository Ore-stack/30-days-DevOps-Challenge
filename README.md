# **Sports API Management System**

## **Overview**
This project demonstrates the creation of a containerized API management system designed for querying real-time sports data. It leverages **Amazon ECS (Fargate)** to manage containers, **Amazon API Gateway** to expose RESTful endpoints, and integrates with an external **Sports API** for live data. The implementation highlights advanced cloud practices, including container orchestration, API management, and secure AWS integrations.

---

## **Key Features**
- Provides a RESTful API for accessing real-time sports data.
- Runs a scalable and serverless backend using Amazon ECS with Fargate.
- Simplifies API management and routing via Amazon API Gateway.

---

## **Prerequisites**
- **Sports API Key**: Create an account at [serpapi.com](https://serpapi.com), subscribe, and obtain your API key.
- **AWS Account**: Set up an AWS account with a basic understanding of ECS, API Gateway, Docker, and Python.
- **AWS CLI**: Install and configure the AWS CLI for interacting with AWS services programmatically.
- **Serpapi Library**: Install the Serpapi library using `pip install google-search-results`.
- **Docker**: Ensure Docker CLI and Docker Desktop are installed for container image creation and management.

---

## **Technical Architecture**
![Technical Architecture](https://github.com/user-attachments/assets/32e49fe6-df16-40cb-b262-af1478cf01d5)

---

## **Technology Stack**
- **Cloud Provider**: AWS
- **Core Services**: Amazon ECS (Fargate), API Gateway, CloudWatch
- **Language**: Python 3.x
- **Containerization**: Docker
- **Security**: IAM roles with least privilege policies for ECS and API Gateway

---

## **Project Structure**

```bash
sports-api-management/
├── app.py           # Flask application for querying sports data
├── Dockerfile       # Dockerfile for containerizing the Flask app
├── requirements.txt # Dependencies for the application
├── .gitignore       # Ignored files and directories
└── README.md        # Documentation
```

---

## **Setup Instructions**

### **1. Clone the Repository**
```bash
git clone https://github.com/Ore-stack/30-days-DevOps-Challenge.git
cd Day 04
```

### **2. Create an Amazon ECR Repository**
```bash
aws ecr create-repository --repository-name sports-api --region us-east-1
```

### **3. Build and Push the Docker Image**
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```

### **4. Configure ECS and Fargate**
1. Create an ECS Cluster:
- Go to the ECS Console → Clusters → Create Cluster
- Name your Cluster (sports-api-cluster)
- For Infrastructure, select Fargate, then create Cluster

2. Create a Task Definition:
- Go to Task Definitions → Create New Task Definition
- Name your task definition (sports-api-task)
- For Infrastructure, select Fargate
- Add the container:
    Name your container (sports-api-container)
    Image URI: <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
    Container Port: 8080
    Protocol: TCP
    Port Name: Leave Blank
    App Protocol: HTTP
- Define Environment Eariables:
    Key: SPORTS_API_KEY
    Value: <YOUR_SPORTSDATA.IO_API_KEY>
    Create task definition

3. Run the Service with an ALB
- Go to Clusters → Select Cluster → Service → Create.
- Capacity provider: Fargate
- Select Deployment configuration family (sports-api-task)
- Name your service (sports-api-service)
- Desired tasks: 2
- Networking: Create new security group
- Networking Configuration:
    Type: All TCP
    Source: Anywhere
- Load Balancing: Select Application Load Balancer (ALB).
- ALB Configuration:
- Create a new ALB:
- Name: sports-api-alb
- Target Group health check path: "/sports"
- Create service

4. Test the ALB:
- After deploying the ECS service, note the DNS name of the ALB (e.g., sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com)
- Confirm the API is accessible by visiting the ALB DNS name in your browser and adding /sports at end (e.g, http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports)

### **5. Configure API Gateway**
1. Create a New REST API:
- Go to API Gateway Console → Create API → REST API
- Name the API (e.g., Sports API Gateway)

2. Set Up Integration:
- Create a resource /sports
- Create a GET method
- Choose HTTP Proxy as the integration type
- Enter the DNS name of the ALB that includes "/sports" (e.g. http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports

3. Deploy the API:
- Deploy the API to a stage (e.g., prod)
- Note the endpoint URL
---

## **Testing**
Use a browser or command-line tool like `curl` to test the API:
```bash
curl https://<api-gateway-id>.execute-api.us-east-1.amazonaws.com/prod/sports
```

---

## **Key Takeaways**
- Set up a scalable, containerized application using ECS and Fargate.
- Manage public-facing APIs effectively with API Gateway.

---

## **Future Improvements**
- **Caching**: Implement Amazon ElastiCache for frequently accessed data.
- **Data Storage**: Integrate Amazon DynamoDB for user-specific queries and preferences.
- **Security**: Enhance security with API keys or IAM-based authentication.
- **CI/CD**: Automate container builds and deployments with a CI/CD pipeline.
