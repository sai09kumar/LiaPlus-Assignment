#  AI Model Deployment & MLOps Assignment

##  Task Overview
**Goal:** Deploy a sample AI model using Docker & Kubernetes and expose it via an API endpoint.

---

##  Dockerfile

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

# Start the FastAPI app
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

##  Python Files

### `app.py` — API Code

```python
from fastapi import FastAPI
from pydantic import BaseModel
import joblib

app = FastAPI()
model = joblib.load("model.joblib")

class InputData(BaseModel):
    feature1: float
    feature2: float

@app.post("/predict")
def predict(data: InputData):
    prediction = model.predict([[data.feature1, data.feature2]])
    return {"prediction": int(prediction[0])}
```

**Explanation:**
- This is a simple FastAPI app that loads a trained ML model.
- It defines a POST endpoint `/predict` where users send two features.
- The model makes a prediction and returns it as JSON.

---

### `train_model.py` — Model Training Script

```python
from sklearn.linear_model import LogisticRegression
import joblib

X = [[0, 0], [1, 1]]
y = [0, 1]

model = LogisticRegression()
model.fit(X, y)

joblib.dump(model, "model.joblib")
```

**Explanation:**
- This script trains a basic logistic regression classifier on dummy data.
- It saves the trained model using `joblib` to a file named `model.joblib`.

---

##  requirements.txt

```txt
fastapi
uvicorn
scikit-learn
joblib
```

---

## ☸ Kubernetes YAML Files

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

## ⚙ Deployment Steps

1. **Train the model**
   ```bash
   python train_model.py
   ```

2. **Build Docker image**
   ```bash
   docker build -t your-dockerhub-username/ai-model:latest .
   ```

3. **Push image to Docker Hub**
   ```bash
   docker push your-dockerhub-username/ai-model:latest
   ```

4. **Apply Kubernetes YAML files**
   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```

5. **Check status**
   ```bash
   kubectl get pods
   kubectl get svc
   ```

6. **Test Via Powershell**
   ```bash
   $body = @{
    feature1 = 1.0
    feature2 = 1.0
   } | ConvertTo-Json

   Invoke-RestMethod -Uri http://127.0.0.1:53081/predict -Method Post -Body $body -ContentType "application/json"
    ```

---

##  Screenshot

> ![WhatsApp Image 2025-04-15 at 15 35 02_c71e0962](https://github.com/user-attachments/assets/7ace71a1-b737-4965-aecd-25a399e3d369)


![WhatsApp Image 2025-04-15 at 15 35 35_2d42a7bd](https://github.com/user-attachments/assets/bdc10b47-7afe-41ba-abd3-87b0be553831)


![WhatsApp Image 2025-04-15 at 15 36 06_218eb129](https://github.com/user-attachments/assets/5a51626a-0ce3-4f20-8add-11a8a77af163)

![WhatsApp Image 2025-04-15 at 15 42 32_c04d29a8](https://github.com/user-attachments/assets/a9328621-2c92-48e1-8ccc-74d4aa6eecc2)

![WhatsApp Image 2025-04-15 at 15 43 14_451dadcd](https://github.com/user-attachments/assets/3d98eeed-b892-41ca-8279-170e6d7842a9)






---

##  Deliverables

- [x] Dockerfile 
- [x] Kubernetes YAML files 
- [x] Deployment steps 
- [x] FastAPI + Model code 
- [ ] Screenshot of model running on Kubernetes 
