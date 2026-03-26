# 🚀 Run Jenkins Locally Using Docker Compose

This guide walks you through setting up and running Jenkins locally using Docker Compose instead of running the `.war` file directly.

---

## ✅ Prerequisites

Make sure you have the following installed:

* Docker
* Docker Compose

Verify installation:

```bash
docker --version
docker compose --version
```

---

## 📦 Step 1: Create a `docker-compose.yaml`

Create a new file named `docker-compose.yaml` in your project directory:

```yaml
version: '3'

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home

volumes:
  jenkins_home:
```

---

## ▶️ Step 2: Start Jenkins

Run the following command in the same directory:

```bash
docker compose up -d
```

This will:

* Pull the Jenkins LTS image (if not already downloaded)
* Start Jenkins in the background
* Expose Jenkins on port `8080`

---

## 🔑 Step 3: Get Initial Admin Password

After starting the container, Jenkins requires an initial admin password.

Run:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Copy the password.

---

## 🌐 Step 4: Access Jenkins

Open your browser and go to:

```
http://localhost:8080
```

* Paste the admin password
* Click **Install Suggested Plugins**
* Create your admin user

---

## ⚠️ Common Issues & Fixes

### 🔸 Permission issues (Linux/macOS)

If you face permission errors:

```bash
sudo chown -R 1000:1000 /path/to/jenkins_home
```

---

### 🔸 Use Docker inside Jenkins (for CI builds)

If your pipelines need Docker, update your service:

```yaml
services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
```

---

## 🛑 Stop Jenkins

```bash
docker compose down
```

---

## 💡 Pro Tips

* Jenkins data is persisted in the `jenkins_home` volume
* You can version control your `docker-compose.yaml`
* Easy to migrate this setup to cloud or Kubernetes later

---

## 🎉 You're Ready!

You now have Jenkins running locally using Docker Compose. Clean, portable, and production-friendly 🚀
