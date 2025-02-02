# **Containerization and Load Balancing with AWS**  
This project demonstrates how to build a containerized system for a simple Flask application that fetches NFL sports data.

---

## **Tools Used**  
- **Docker**  
- **AWS ECS (Elastic Container Service)**  
- **AWS ECR (Elastic Container Registry)**  
- **Elastic Load Balancer (ALB)**  
- **SerpAPI**  
- **Python (Flask)**  

---

## **Prerequisites**  
Before starting, ensure you have the following:  

1. **AWS User with Required Permissions**  
   - Allocate permissions for **ECS**, **API Gateway**, and **ECR**.  
2. **AWS CLI**  
   - Install and configure the AWS CLI with your user details.  
3. **SerpAPI Account**  
   - Register on SerpAPI and acquire an API Key.  
4. **Docker**  
   - Install and configure Docker on your local environment.  

---

## **Setup Instructions**  

### **Step 1: Clone the Repository**  
Clone the project repository to your local environment:  
```bash
git clone https://github.com/ViolaSangut/Containerization-and-Load_balancing-with-AWS.git
```

---

### **Step 2: Create an ECR Repository**  
Create an Elastic Container Registry (ECR) repository in AWS to store your Docker images:  
```bash
aws ecr create-repository --repository-name sports-api --region us-east-1
```

---

### **Step 3: Authenticate with ECR**  
Authenticate Docker with your ECR repository:  
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

### **Step 4: Build and Push the Image**  
Build the Docker image, tag it, and push it to the ECR repository:  
```bash
docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```

---

### **Step 5: Create an ECS Cluster**  
Navigate to the **AWS ECS Console** and create a new cluster. Select **Fargate** as the launch type.

---

### **Step 6: Create a Task Definition**  
Define a task in ECS with the following:  
- Container image URI from ECR.  
- Port configuration.  
- Environment variables (e.g., API keys).

---

### **Step 7: Deploy the Service**  
Deploy your task as a service within the ECS cluster:  
- Configure the desired number of tasks.  
- Set up networking (security groups, subnets).  
- Attach an **Application Load Balancer (ALB)** for traffic routing.

---

### **Step 8: Test the ALB**  
Retrieve the DNS name of the ALB from the AWS Console and test it by accessing the application in a browser.  
Example:  
```bash
http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com
```

---

### **Step 9: Configure API Gateway**  
Expose your ECS service as a REST API using **API Gateway**:  
- Create a new API.  
- Configure resources and methods (e.g., **GET** for fetching data).  
- Integrate the API Gateway with your ALB DNS.

---

### **Step 10: Test the API**  
Test the API Gateway endpoint by accessing it in a browser or using **curl**:  
```bash
curl https://<api-gateway-id>.execute-api.us-east-1.amazonaws.com/prod/sports
```

---

## **What We Learned**  
- Setting up a scalable, containerized application with ECS.  
- Creating public APIs using API Gateway.  

---

## **Future Enhancements**  
- Add caching for frequent API requests using **Amazon ElastiCache**.  
- Integrate **DynamoDB** to store user-specific queries and preferences.  
- Secure the API Gateway using API Keys or IAM-based authentication.  
- Implement **CI/CD pipelines** for automating container deployments.
