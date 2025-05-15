# 🚀 MERN Blog App Deployment

This project demonstrates the **automated deployment** of a full-stack MERN (MongoDB, Express, React, Node.js) blog application on AWS using **Terraform** and **Ansible**.

---

## 🧠 Architecture Overview

A high-level architecture showing integration between:

- EC2 instance hosting the backend
- MongoDB Atlas as a managed database
- S3 for media upload & static frontend hosting

![Architecture](screenshot/architecture-diagram.png)

---

## 🗃️ MongoDB Atlas Setup

We used **MongoDB Atlas**, a cloud-hosted NoSQL database, as the primary data store for the blog application.

### 📌 Steps taken:
1. Created a **free-tier MongoDB Atlas cluster**
2. Created a new **database user** with read/write access
3. Whitelisted the **EC2 instance IP** to allow secure connections
4. Retrieved the **connection URI**, and added it to the backend `.env` file:

MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/<db-name>?retryWrites=true&w=majority
Backend successfully connected and interacted with MongoDB Atlas for:

User authentication

Blog post creation and fetching

Comments and media references

✅ Verified the connection by running the backend and querying data.

![MongoDB Setup](screenshot/MongoDB-cluster.png)

---

## ⚙️ Backend Provisioning with Ansible

Backend is deployed to an Ubuntu EC2 instance using **Ansible Playbook** with PM2.

![PM2 Running](screenshot/PM2showingbackendrunning.png)


### Run the backend playbook:

ansible-playbook -i inventory backend-playbook.yml
PM2 Process Status:
bash


pm2 list

🖼️ Media Uploads to S3
A separate bucket (media-bucket-alsufyani) is used

IAM access keys are used for programmatic uploads from the backend


🌐 Frontend Deployment to S3
The frontend was built using Vite + React and deployed to S3 static website hosting.

.env config:
env


VITE_BASE_URL=http://<EC2-PUBLIC-DNS>:5000/api
VITE_MEDIA_BASE_URL=https://media-bucket-alsufyani.s3.eu-north-1.amazonaws.com
Build & Deploy:
bash


pnpm run build
aws s3 sync dist/ s3://frontend-bucket-alsufyani/ --delete

🔐 Security Configuration
IAM user created with restricted access to the media S3 bucket

Security group allows ports: 22, 80, 5000

Sensitive data managed via environment variables

.gitignore includes:

bash


.env
.pem
.terraform/
*.key
🛠️ Technologies Used
Tool	Purpose
AWS EC2	Backend hosting
MongoDB Atlas	Managed NoSQL DB
AWS S3	Media + Frontend storage
Terraform	Infrastructure provisioning
Ansible	Configuration management
PM2	Process manager for Node.js
Node.js	Backend server
React.js	Frontend application

🧼 Cleanup Steps
To destroy the created infrastructure:

bash


terraform destroy
Also:

Remove .env and credential files from EC2

Revoke any IAM access keys if used manually

Delete MongoDB users and IP access rules

✅ Submission Checklist
✔️ Terraform configuration files
✔️ Ansible Playbook with roles
✔️ Deployed & running MERN app
✔️ Functional media uploads
✔️ Frontend live on S3
✔️ All screenshots attached
✔️ Secure and cleaned-up repo

Made with ❤️ by NawalSuf
