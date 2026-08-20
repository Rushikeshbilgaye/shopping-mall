# 🛍️ UrbanMall – Shopping Mall Website

A modern and responsive shopping mall website built with **HTML, CSS, and JavaScript** and containerized using **Docker**.

This project demonstrates a complete **DevOps CI/CD workflow** using **GitHub, Jenkins, Docker, Docker Hub, and AWS EC2**.

---

## 🚀 Project Overview

UrbanMall is a simple e-commerce-style shopping mall website that includes:

* 🏠 Home page
* 🔎 Product search
* 🛍️ Product categories
* 🛒 Shopping cart counter
* ⭐ Product ratings
* 💰 Product pricing
* 🔥 Special offers section
* 📱 Responsive design
* 📞 Contact information

The application is served using **Nginx inside a Docker container**.

---

## 🏗️ Architecture

```text
                    👨‍💻 Developer
                         |
                         ▼
                    GitHub Repo
                         |
                         ▼
                      Jenkins
                         |
              ┌──────────┴──────────┐
              │                     │
           Clone                 Build
              │                     │
              └──────────┬──────────┘
                         ▼
                    Docker Image
                         |
                         ▼
                     Docker Hub
                         |
                         ▼
                     AWS EC2
                         |
                         ▼
                  Docker Container
                         |
                         ▼
                       Nginx
                         |
                         ▼
                 🛍️ UrbanMall Website
```

---

## 🛠️ Technologies Used

| Technology | Purpose                       |
| ---------- | ----------------------------- |
| HTML5      | Website structure             |
| CSS3       | Website styling               |
| JavaScript | Search and cart functionality |
| Nginx      | Web server                    |
| Docker     | Containerization              |
| Docker Hub | Docker image repository       |
| Jenkins    | CI/CD automation              |
| GitHub     | Source code management        |
| AWS EC2    | Application hosting           |
| Linux      | Server environment            |

---

## 📁 Project Structure

```text
shopping-mall/
│
├── index.html
├── Dockerfile
└── README.md
```

---

## 💻 Run Locally

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/shopping-mall.git
```

### 2. Enter the project directory

```bash
cd shopping-mall
```

### 3. Open the website

You can simply open:

```text
index.html
```

in your browser.

---

# 🐳 Run Using Docker

### 1. Build Docker image

```bash
docker build -t shopping-mall .
```

### 2. Run container

```bash
docker run -d \
  --name shopping-mall-container \
  -p 80:80 \
  shopping-mall
```

### 3. Check running container

```bash
docker ps
```

### 4. Open website

```text
http://localhost
```

---

# 🐳 Docker Hub

Tag the Docker image:

```bash
docker tag shopping-mall YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

Login to Docker Hub:

```bash
docker login
```

Push the image:

```bash
docker push YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

---

# ☁️ AWS EC2 Deployment

The application can be deployed on an **AWS EC2 Ubuntu instance**.

### 1. Connect to EC2

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

### 2. Install Docker

```bash
sudo apt update
sudo apt install docker.io -y
```

### 3. Start Docker

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### 4. Pull image from Docker Hub

```bash
sudo docker pull YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

### 5. Run container

```bash
sudo docker run -d \
  --name shopping-mall-container \
  -p 80:80 \
  YOUR_DOCKERHUB_USERNAME/shopping-mall:latest
```

### 6. Access application

Open:

```text
http://YOUR_EC2_PUBLIC_IP
```

Make sure the EC2 Security Group allows:

```text
HTTP
Port: 80
Source: 0.0.0.0/0
```

---

# 🔄 Jenkins CI/CD Pipeline

Jenkins is used to automate the application deployment.

### Pipeline Flow

```text
GitHub
   ↓
Jenkins
   ↓
Git Clone
   ↓
Docker Build
   ↓
Docker Login
   ↓
Docker Push
   ↓
AWS EC2
   ↓
Docker Pull
   ↓
Docker Run
   ↓
Application Live
```

---

## 🔧 Jenkins Pipeline Stages

The Jenkins pipeline contains stages such as:

```text
1. Checkout
2. Build Docker Image
3. Stop Existing Container
4. Remove Existing Container
5. Run New Container
6. Docker Login
7. Push Image to Docker Hub
```

---

## 📦 Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# 🔐 AWS Security Group

Required inbound rules:

| Type | Port | Source    |
| ---- | ---: | --------- |
| SSH  |   22 | Your IP   |
| HTTP |   80 | 0.0.0.0/0 |

For production environments, avoid opening SSH to everyone.

---

# 📸 Application Preview

Add your website screenshot here:

```markdown
![UrbanMall Website](screenshots/homepage.png)
```

You can create a `screenshots` folder:

```text
shopping-mall/
│
├── index.html
├── Dockerfile
├── README.md
│
└── screenshots/
    └── homepage.png
```

---

# 🎯 DevOps Skills Demonstrated

This project demonstrates practical knowledge of:

* Git and GitHub
* Linux
* Docker
* Dockerfile
* Docker Hub
* Jenkins
* CI/CD
* AWS EC2
* Nginx
* Container deployment
* AWS Security Groups
* Application troubleshooting

---

# 🧪 Troubleshooting

### Container name already exists

If you get:

```text
Conflict. The container name is already in use
```

Run:

```bash
docker rm -f shopping-mall-container
```

Then start the container again:

```bash
docker run -d \
  --name shopping-mall-container \
  -p 80:80 \
  shopping-mall
```

### Check container logs

```bash
docker logs shopping-mall-container
```

### Check running containers

```bash
docker ps
```

### Check Docker images

```bash
docker images
```

---

# 🔮 Future Improvements

Planned improvements:

* [ ] Add backend API
* [ ] Add user authentication
* [ ] Add product database
* [ ] Add real shopping cart
* [ ] Add payment integration
* [ ] Add Kubernetes deployment
* [ ] Add Terraform infrastructure
* [ ] Add AWS Load Balancer
* [ ] Add Auto Scaling
* [ ] Add CloudWatch monitoring
* [ ] Add HTTPS using SSL/TLS
* [ ] Implement complete CI/CD deployment

---

# 👨‍💻 Author

**Rushikesh Bilgaye**

### DevOps Engineer | Cloud & Automation Enthusiast

Skills:

```text
AWS | Linux | Docker | Kubernetes | Terraform |
Jenkins | Git | GitHub | CI/CD | Nginx
```

---

## ⭐ Project Goal

The main goal of this project is to demonstrate how a simple web application can be **containerized, automated, and deployed using modern DevOps practices**.

```text
Code → GitHub → Jenkins → Docker → Docker Hub → AWS EC2 → Production
```

---

## 📄 License

This project is created for **learning, practice, and portfolio purposes**.
