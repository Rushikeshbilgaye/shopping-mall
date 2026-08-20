# 🛍️ UrbanMall – Shopping Mall Website

A modern and responsive **Shopping Mall Website** built using HTML, CSS, and JavaScript and deployed using a complete **DevOps CI/CD pipeline**.

This project demonstrates how a web application can be containerized using **Docker** and automatically built, pushed, and deployed using **Jenkins**.

---

## 🚀 Project Overview

**UrbanMall** is a simple e-commerce-style shopping mall website containing:

* 🏠 Home page
* 🔎 Product search
* 🛍️ Product categories
* 🛒 Shopping cart counter
* ⭐ Product ratings
* 💰 Product prices
* 🔥 Special offers
* 📱 Responsive design
* 📞 Contact information

The application runs inside an **Nginx Docker container**.

---

# 🏗️ Project Architecture

```text
                     👨‍💻 Developer
                          |
                          ▼
                       GitHub
                          |
                          ▼
                       Jenkins
                          |
                 ┌────────┴────────┐
                 │                 │
                 ▼                 ▼
            Git Checkout      Docker Build
                                   |
                                   ▼
                              Docker Image
                                   |
                                   ▼
                              Docker Hub
                                   |
                                   ▼
                         Docker Container
                                   |
                                   ▼
                                Nginx
                                   |
                                   ▼
                         🛍️ UrbanMall
```

---

# 🛠️ Technologies Used

| Technology | Purpose                       |
| ---------- | ----------------------------- |
| HTML5      | Website structure             |
| CSS3       | Website styling               |
| JavaScript | Search and cart functionality |
| Nginx      | Web server                    |
| Docker     | Application containerization  |
| Docker Hub | Docker image repository       |
| Jenkins    | CI/CD automation              |
| Git        | Version control               |
| GitHub     | Source code management        |
| Linux      | Server environment            |
| AWS EC2    | Application hosting           |

---

# 📁 Project Structure

```text
shopping-mall/
│
├── index.html
│
├── Dockerfile
│
├── Jenkinsfile
│
└── README.md
```

---

# 🌐 Application

The main application is contained in:

```text
index.html
```

The website is served using Nginx.

---

# 🐳 Dockerfile

The application uses the official lightweight Nginx Alpine image.

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Dockerfile Explanation

```text
FROM nginx:alpine
```

Uses Nginx Alpine as the base image.

```text
COPY index.html /usr/share/nginx/html/index.html
```

Copies the website into the Nginx web root.

```text
EXPOSE 80
```

Exposes HTTP port 80.

```text
CMD ["nginx", "-g", "daemon off;"]
```

Starts Nginx in the foreground so the Docker container continues running.

---

# 💻 Run Application Locally

## 1. Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/shopping-mall.git
```

## 2. Enter Project

```bash
cd shopping-mall
```

## 3. Build Docker Image

```bash
docker build -t shopping-mall .
```

## 4. Run Container

```bash
docker run -d \
  --name shopping-mall-container \
  -p 80:80 \
  shopping-mall
```

## 5. Check Container

```bash
docker ps
```

## 6. Open Website

Open:

```text
http://localhost
```

---

# 🐳 Docker Hub

The Docker image can be pushed to Docker Hub.

## Login

```bash
docker login
```

## Tag Image

```bash
docker tag shopping-mall \
YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

## Push Image

```bash
docker push \
YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

---

# 🔄 Jenkins CI/CD Pipeline

The project uses a **Jenkinsfile** to automate the Docker build and deployment process.

## Pipeline Flow

```text
Developer
    |
    ▼
GitHub
    |
    ▼
Jenkins
    |
    ▼
Checkout
    |
    ▼
Docker Build
    |
    ▼
Docker Login
    |
    ▼
Docker Push
    |
    ▼
Stop Existing Container
    |
    ▼
Remove Existing Container
    |
    ▼
Deploy New Container
    |
    ▼
🌐 UrbanMall Live
```

---

# 📜 Jenkinsfile

The Jenkins pipeline contains the following stages:

```text
1. Checkout
2. Build Docker Image
3. Docker Login
4. Push Image to Docker Hub
5. Stop Existing Container
6. Remove Existing Container
7. Deploy Container
```

Example pipeline:

```groovy
pipeline {

    agent any

    environment {
        IMAGE_NAME = "YOUR_DOCKERHUB_USERNAME/shopping-mall"
        CONTAINER_NAME = "shopping-mall-container"
        PORT = "80"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USERNAME/shopping-mall.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Docker Login') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                    docker push ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${PORT}:80 \
                    ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {

        success {
            echo 'UrbanMall deployed successfully!'
        }

        failure {
            echo 'Deployment failed. Check Jenkins console output.'
        }

        always {
            sh 'docker ps'
        }
    }
}
```

---

# 🔐 Jenkins Docker Hub Credentials

Create Docker Hub credentials in Jenkins.

Go to:

```text
Jenkins
   ↓
Manage Jenkins
   ↓
Credentials
   ↓
Global
   ↓
Add Credentials
```

Use:

```text
Kind:
Username with password

Username:
YOUR_DOCKERHUB_USERNAME

Password:
YOUR_DOCKERHUB_ACCESS_TOKEN

ID:
dockerhub-credentials
```

The Jenkinsfile uses:

```groovy
credentialsId: 'dockerhub-credentials'
```

to access the credentials securely.

---

# 🐧 Jenkins Docker Permission

If Jenkins cannot execute Docker commands, add Jenkins to the Docker group:

```bash
sudo usermod -aG docker jenkins
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

Test:

```bash
sudo -u jenkins docker ps
```

---

# ☁️ AWS EC2 Deployment

This project can be hosted on an Ubuntu EC2 instance.

## Install Docker

```bash
sudo apt update
sudo apt install docker.io -y
```

## Start Docker

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## Verify Docker

```bash
docker --version
```

---

# 🔥 AWS Security Group

Allow HTTP traffic:

| Type | Protocol | Port | Source    |
| ---- | -------- | ---: | --------- |
| HTTP | TCP      |   80 | 0.0.0.0/0 |
| SSH  | TCP      |   22 | Your IP   |

For better security, SSH should be restricted to your own IP address.

---

# 🌍 Access Application on AWS

After the container is running:

```bash
docker ps
```

You should see:

```text
0.0.0.0:80->80/tcp
```

Open your browser:

```text
http://YOUR_EC2_PUBLIC_IP
```

Your UrbanMall website should now be accessible.

---

# 🧪 Useful Docker Commands

### View running containers

```bash
docker ps
```

### View all containers

```bash
docker ps -a
```

### View images

```bash
docker images
```

### View container logs

```bash
docker logs shopping-mall-container
```

### Stop container

```bash
docker stop shopping-mall-container
```

### Remove container

```bash
docker rm shopping-mall-container
```

### Remove container forcefully

```bash
docker rm -f shopping-mall-container
```

---

# ⚠️ Common Docker Error

If you receive:

```text
Conflict. The container name "/shopping-mall-container"
is already in use
```

Run:

```bash
docker rm -f shopping-mall-container
```

Then run:

```bash
docker run -d \
  --name shopping-mall-container \
  -p 80:80 \
  YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

---

# 🎯 DevOps Skills Demonstrated

This project demonstrates practical experience with:

* ✅ Git
* ✅ GitHub
* ✅ Linux
* ✅ Docker
* ✅ Dockerfile
* ✅ Docker Hub
* ✅ Jenkins
* ✅ Jenkins Pipeline
* ✅ CI/CD
* ✅ Nginx
* ✅ AWS EC2
* ✅ AWS Security Groups
* ✅ Container Deployment
* ✅ Troubleshooting

---

# 📈 Future Improvements

The project can be extended with:

* [ ] Backend API
* [ ] User authentication
* [ ] Product database
* [ ] Real shopping cart
* [ ] Payment gateway
* [ ] Kubernetes deployment
* [ ] Terraform infrastructure
* [ ] AWS Load Balancer
* [ ] Auto Scaling
* [ ] Route 53
* [ ] HTTPS / SSL
* [ ] CloudWatch monitoring
* [ ] Blue-Green Deployment
* [ ] Automated testing

---

# 👨‍💻 Author

## Rushikesh Bilgaye

**DevOps Engineer | AWS | Docker | Kubernetes | Terraform | Jenkins | CI/CD**

### Technologies

```text
AWS
Linux
Git & GitHub
Docker
Kubernetes
Terraform
Jenkins
CI/CD
Nginx
```

---

# ⭐ Project Objective

The objective of this project is to demonstrate a real-world DevOps workflow where application code is stored in GitHub, automatically built using Jenkins, containerized with Docker, pushed to Docker Hub, and deployed as a containerized application.

```text
Code
  ↓
GitHub
  ↓
Jenkins
  ↓
Docker Build
  ↓
Docker Hub
  ↓
Docker Container
  ↓
Nginx
  ↓
AWS EC2
  ↓
🌐 UrbanMall
```

---

## 📄 License

This project is created for **learning, portfolio, and DevOps practice purposes**.
