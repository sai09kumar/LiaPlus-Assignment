# 🧠 AI Model Deployment & MLOps Assignment

## 🚀 Task Overview
**Goal:** Deploy a sample AI model using Docker & Kubernetes and expose it via an API endpoint.

---

## 📦 Dockerfile

> 🔧 Paste your Dockerfile here. Example for a simple FastAPI ML model is included:

```Dockerfile
# Use Python base image
FROM python:3.9-slim

# Set work directory
WORKDIR /app

# Copy requirements and install
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app files
COPY . .

# Expose port
EXPOSE 8000

# Start the API
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## ☸️ Kubernetes YAML Files

> 🧩 Insert your Kubernetes manifests here. Example Deployment and Service included:

### `deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ai-model-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: ai-model
  template:
    metadata:
      labels:
        app: ai-model
    spec:
      containers:
      - name: ai-model-container
        image: your-dockerhub-username/ai-model:latest
        ports:
        - containerPort: 8000
```

### `service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: ai-model-service
spec:
  selector:
    app: ai-model
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
  type: LoadBalancer
```

---

## ⚙️ Deployment Steps

1. **Build the Docker image**
   ```bash
   docker build -t your-dockerhub-username/ai-model:latest .
   ```

2. **Push to Docker Hub**
   ```bash
   docker push your-dockerhub-username/ai-model:latest
   ```

3. **Apply Kubernetes configs**
   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```

4. **Verify Deployment**
   ```bash
   kubectl get pods
   kubectl get svc
   ```

---

## 📸 Screenshot

> 🖼 Paste a screenshot of the model running on Kubernetes here.

![Screenshot Placeholder](./path-to-your-screenshot.png)

---

## ✅ Deliverables Recap

- [x] Dockerfile ✅
- [x] Kubernetes YAML files ✅
- [x] Deployment steps ✅
- [ ] Screenshot of model running on Kubernetes ⬜
