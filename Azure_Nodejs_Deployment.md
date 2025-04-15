# 🚀 Deploying a Node.js Web App to Azure App Service

---

## 1. 🗺 Architecture Diagram

```plaintext
[User Browser]
      |
      v
[Public URL - Azure App Service]
      |
      v
[Node.js Web App]
      |
      v
[Azure Resource Group & App Service Plan]
```

---

## 2. 🧰 Steps to Deploy the Application

### 🔧 Prerequisites

- Azure account
- Node.js and npm installed
- Azure CLI installed (`az login` to log in)

---

### 💻 Step-by-Step Instructions

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

- ✅ Web page showing “Hello from Azure!”
- ✅ Azure Portal > Resource Group view
- ✅ Azure App Service configuration page
- ✅ Git push success terminal output

> 💡 You can insert screenshots in this markdown like below:

```markdown
![Hello Page](./screenshots/hello-from-azure.png)
![Resource Group](./screenshots/resource-group.png)
![App Config](./screenshots/app-config.png)
![Git Push](./screenshots/git-push-success.png)
```
