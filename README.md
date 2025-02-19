# ML Model Deployment Using Docker on AWS EC2

## 📖 Overview
This project demonstrates the deployment of an ML model using Docker on an AWS EC2 instance. The model is served via Streamlit, providing an interactive UI for data visualization and analysis.

## 🌟 Features
- **Containerized ML Model**: Deploy an ML model within a Docker container for easy portability and scalability.
- **AWS EC2 Integration**: Set up and run the model on an EC2 instance with Amazon Linux.
- **Interactive Web Interface**: Users can explore and interact with the ML model through Streamlit.
- **Docker Hub Deployment**: Easily share and distribute the model using Docker Hub.

---
## 🛠️ Prerequisites
Before getting started, ensure you have the following:
- An AWS account with access to EC2.
- A key pair (`Uttkarsh.pem`) for secure SSH access.
- Docker installed on the EC2 instance.
- Basic knowledge of the command line.

---
## 📋 Setup Instructions
### 1️⃣ **Launch EC2 Instance**
- Go to the AWS Management Console and navigate to EC2.
- Create a new instance with **Amazon Linux 2**.
- Assign a name to the instance (e.g., `Website`).
- Choose an instance type (e.g., `t2.micro` for free-tier eligibility).
- Create a **key pair** (e.g., `Uttkarsh.pem`) for secure SSH access.

![EC2 Setup](image1.jpeg)

---
### 2️⃣ **Configure Security Groups**
- Edit the **inbound rules** to allow traffic on port **8501**.
- Set the source to `0.0.0.0/0` (public access for testing purposes).

![Security Group](image2.jpeg)

---
### 3️⃣ **Connect to EC2 via SSH**
```bash
chmod 600 Uttkarsh.pem
ssh -i Uttkarsh.pem ec2-user@<EC2-Public-IP>
```

---
### 4️⃣ **Update System and Install Docker**
```bash
sudo yum update -y
sudo dnf install docker -y
sudo service docker start
sudo docker --version
```

![Docker Installation](image3.jpeg)

---
### Documentation
- Docker
- ML model using Docker
- Machine Learning

---
### 5️⃣ **Import Dockerfile**
Now we will import the Docker file also.

```dockerfile
# For more information, please refer to https://aka.ms/vscode-docker-python
FROM python:3.9-slim

WORKDIR /app
COPY app.py /app
COPY requirements.txt /app
COPY mushrooms.csv /app
RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt
EXPOSE 8501

# Creates a non-root user with an explicit UID and adds permission to access the /app folder

# During debugging, this entry point will be overridden. For more information, please refer to https://aka.ms/vscode-docker-python-debug
ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---
### 6️⃣ **Set Up Working Directory**
```bash
mkdir downloads
cd downloads
```

---
### 7️⃣ **Transfer Files to EC2**
On your local machine, run:
```bash
scp -i Uttkarsh.pem Dockerfile app.py requirements.txt mushrooms.csv ec2-user@<EC2-Public-IP>:/home/ec2-user/downloads
```

![File Transfer](image4.jpeg)

---
### 8️⃣ **Build and Run Docker Container**
```bash
cd /home/ec2-user/downloads
sudo docker build -t my_app:v1.0 -f Dockerfile .
sudo docker run -d -p 8501:8501 my_app:v1.0
```

Once the container is running, find the **container ID**:
```bash
sudo docker ps
```

![Container Running](image5.jpeg)

---
### 9️⃣ **Access the Application**
Open your browser and go to:
```
http://<EC2-Public-IP>:8501
```

You should see the ML model’s web interface.

![ML App](image6.jpeg)

---
## 🎉 Congratulations!
Your ML model is now successfully deployed using Docker on AWS EC2. 🚀

