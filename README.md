#  AI Chatbot – 3 Tier DevOps Project

This repository contains a **3-tier AI Chatbot application** built to understand **real-world DevOps workflows** using **Docker, Kubernetes, and AI integration (Gemini)**.

The focus of this project is on:

* Proper **architecture**
* Clear **deployment flow**
* Real **DevOps practices**, not just coding

---

## 📌 Project Objective

To build and deploy a **scalable chatbot application** with:

* A user interface (Frontend)
* A backend API integrated with **Gemini AI**
* A database (MongoDB)
* Deployed on **Kubernetes**

---

## 🧱 Architecture (3-Tier)

```
Frontend (React UI)
        |
        v
Backend (Node.js + Gemini API)
        |
        v
Database (MongoDB)
```

All components are:

* Containerized using **Docker**
* Orchestrated using **Kubernetes**

---

## 🛠️ Tech Stack

| Layer         | Technology             |
| ------------- | ---------------------- |
| Frontend      | React                  |
| Backend       | Node.js (Express)      |
| AI            | Google Gemini API      |
| Database      | MongoDB                |
| Containers    | Docker                 |
| Orchestration | Kubernetes             |
| Secrets       | Kubernetes Secrets     |
| OS            | macOS (Docker Desktop) |

---

## 📂 Folder Structure

```
chatbot-project/
│
├── chatbot-ui/                 # Frontend (React)
│   ├── src/App.js              # UI logic & API call
│   └── Dockerfile              # Frontend Docker build
│
├── chatbot-backend/            # Backend (Node.js)
│   ├── app.js                  # API + Gemini logic
│   ├── package.json
│   └── Dockerfile              # Backend Docker build
│
├── chatbot-manifests/          # Kubernetes YAML files
│   ├── frontend.yaml
│   ├── backend.yaml
│   ├── backend-service.yaml
│   ├── mongodb.yaml
│   └── mongodb-service.yaml
│
└── README.md                   # Project documentation
```

---

## 🔁 Application Flow

1. User enters a message in the UI
2. Frontend sends the request to Backend Service
3. Backend sends the request to **Gemini AI**
4. Gemini generates a response
5. Backend returns the response
6. Frontend displays it to the user

---

## 🔐 Security Flow (Important)

* Gemini API key is **never hardcoded**
* Stored securely using **Kubernetes Secrets**
* Injected into the backend as an environment variable

```
Gemini API Key
 → Kubernetes Secret
 → Backend Pod (env variable)
 → Backend Code
```

---

## ⚙️ Prerequisites

Make sure you have the following installed:

```bash
Docker Desktop
kubectl
git
```

Enable Kubernetes in Docker Desktop:

```
Docker Desktop → Settings → Kubernetes → Enable
```

Verify:

```bash
kubectl get nodes
```

---

## 🚀 Deployment Steps (High Level)

### 1️⃣ Frontend

* Add React UI code in `chatbot-ui/src/App.js`
* Build Docker image
* Push image to Docker Hub

```bash
docker build -t <dockerhub-username>/chatbot-ui .
docker push <dockerhub-username>/chatbot-ui
```

---

### 2️⃣ Backend

* Add Node.js + Gemini logic in `chatbot-backend/app.js`
* Read Gemini API key using environment variable
* Build and push Docker image

```bash
docker build -t <dockerhub-username>/chatbot-backend .
docker push <dockerhub-username>/chatbot-backend
```

---

### 3️⃣ MongoDB

* MongoDB runs inside Kubernetes
* Backend connects using service DNS name

Apply:

```bash
kubectl apply -f mongodb.yaml
kubectl apply -f mongodb-service.yaml
```

---

### 4️⃣ Create Gemini API Secret

```bash
kubectl create secret generic gemini-secret \
  --from-literal=GEMINI_API_KEY=<YOUR_REAL_API_KEY>
```

---

### 5️⃣ Deploy Backend & Frontend

```bash
kubectl apply -f backend.yaml
kubectl apply -f backend-service.yaml
kubectl apply -f frontend.yaml
```

Restart backend:

```bash
kubectl delete pod -l app=backend
```

---

## 🧪 Testing

### Backend Test

```bash
kubectl port-forward deployment/chatbot-backend 4000:3000
```

```bash
curl -X POST http://localhost:4000/chat \
-H "Content-Type: application/json" \
-d '{"message":"Hello"}'
```

---

### Frontend Test

```bash
kubectl port-forward deployment/chatbot-frontend 3000:3000
```

Open browser:

```
http://localhost:3000
```

---

## 🧠 Key DevOps Learnings

* Kubernetes runs **Docker images**, not local code
* Services are required for pod-to-pod communication
* Secrets must be injected securely
* Any code change requires:

```
Build → Push → Pod Restart
```

---

## 🚀 Future Improvements

* Store chat history in MongoDB
* Add health checks (liveness & readiness probes)
* Add Jenkins CI pipeline
* Add GitOps with Argo CD
* Deploy to cloud using Terraform
