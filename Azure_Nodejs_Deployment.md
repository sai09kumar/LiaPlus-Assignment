#  Deploying a Node.js Web App to Azure App Service

---

## 1.  Architecture Diagram

![WhatsApp Image 2025-04-15 at 16 50 37_9ffe0f98](https://github.com/user-attachments/assets/fb9e6ef7-c83c-42b0-8a59-87a2a6b75e39)


---

## 2.  Steps to Deploy the Application

###  Prerequisites

- Azure account
- Node.js and npm installed
- Azure CLI installed (`az login` to log in)

---

###  Step-by-Step Instructions

#### 1. Create a Simple Node.js App

```bash
mkdir azure-node-app
cd azure-node-app
npm init -y
npm install express
```

#### 2. Create `server.js`

```javascript
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Hello from Azure!'));

app.listen(port, () => console.log(`App listening on port ${port}`));
```

#### 3. Add a Start Script in `package.json`

```json
"scripts": {
  "start": "node server.js"
}
```

#### 4. Initialize a Git Repository

```bash
git init
git add .
git commit -m "Initial commit"
```

#### 5. Create and Deploy on Azure

```bash
az group create --name azure-node-rg --location eastus

az appservice plan create --name nodeAppPlan --resource-group azure-node-rg --sku FREE

az webapp create --resource-group azure-node-rg \
  --plan nodeAppPlan --name <your-app-name> \
  --runtime "NODE|18-lts" --deployment-local-git
```

#### 6. Push Code to Azure

Azure will give you a Git URL. Example:

```bash
git remote add azure https://<your-app-name>.scm.azurewebsites.net:443/<your-app-name>.git
git push azure master
```

#### 7. Access via Public URL

Open: `https://<your-app-name>.azurewebsites.net`

---

## 3. ⚙️ Azure Configurations Used

| Configuration     | Value               |
|-------------------|---------------------|
| Resource Group     | `azure-node-rg`     |
| App Service Plan   | `nodeAppPlan` (SKU: FREE) |
| Web App Name       | `<your-app-name>`   |
| Region             | `East US` (or preferred) |

---

## 4. 📸 Screenshots to Include

- ![WhatsApp Image 2025-04-13 at 20 27 54_b830c999](https://github.com/user-attachments/assets/c711bb3d-f4d7-4a55-8777-50db583027fa)

- ![WhatsApp Image 2025-04-13 at 20 28 18_32bb77b1](https://github.com/user-attachments/assets/e71cb7ce-66f1-43cc-9eb8-c4e764a7ad1a)

- ![WhatsApp Image 2025-04-13 at 20 48 59_7128f91b](https://github.com/user-attachments/assets/8a4af911-d67a-4f7f-9df5-f7496e02141e)

- ![WhatsApp Image 2025-04-13 at 20 49 44_293aac54](https://github.com/user-attachments/assets/5b9e416d-ffbe-4030-8dde-d16b3e6e7426)
  
- ![WhatsApp Image 2025-04-13 at 21 01 01_257a407d](https://github.com/user-attachments/assets/5a5d2296-0aae-4760-b7dc-93467ee6e422)

- ![WhatsApp Image 2025-04-13 at 21 12 07_3034f3f6](https://github.com/user-attachments/assets/ed2349c1-a2ce-487f-8cf3-76a5907fe799)





```
