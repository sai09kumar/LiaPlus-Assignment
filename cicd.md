
# CI/CD Pipeline with Jenkins & Azure Deployment for Node.js App 

## 1. Node.js Web Application

### Project Setup

```bash
mkdir azure-node-app
cd azure-node-app
npm init -y
npm install express
```

### Create `server.js`

```js
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Hello from Azure!'));

app.listen(port, () => console.log(`App listening on port ${port}`));
```

### Update `package.json` Scripts

```json
"scripts": {
  "start": "node server.js"
}
```

### Initialize Git Repository

```bash
git init
git add .
git commit -m "Initial commit"
```

---

## 2. CI/CD Pipeline Using Jenkins

### Jenkinsfile

A `Jenkinsfile` is used to define the pipeline:

```groovy
<see separate Jenkinsfile>
```

### Pipeline Stages Explained

- **Checkout**: Pulls source code from Git.
- **Install Dependencies**: Runs `npm install`.
- **Build**: No build step needed here.
- **Test**: Placeholder for future test commands.
- **Deploy**: Deploys app to Azure via ZIP Deploy.

### Secrets Management

Use Jenkins **Credentials** to store:

- `azure-app-name`: Your App Service name.
- `azure-publish-profile`: Base64 string or XML contents of the Azure publish profile.

Use `credentials()` in Jenkinsfile to load these securely.

---

![Uploading img2.png…]()


## 3. Azure Configurations Used

- **Resource Group**: `azure-node-rg`
- **App Service Plan**: `nodeAppPlan` (SKU: FREE)
- **Web App Name**: Your unique app name
- **Region**: East US (or your preferred region)

---

## 4. Azure Deployment via CLI

```bash
az group create --name azure-node-rg --location eastus

az appservice plan create --name nodeAppPlan --resource-group azure-node-rg --sku FREE

az webapp create --resource-group azure-node-rg \
  --plan nodeAppPlan --name <your-app-name> \
  --runtime "NODE|18-lts" --deployment-local-git
```

### Push Code to Azure

```bash
git remote add azure https://<your-app-name>.scm.azurewebsites.net:443/<your-app-name>.git
git push azure master
```

### Access Deployed App

Visit: `https://<your-app-name>.azurewebsites.net`

---

## 5. Screenshots to Include

- The deployed web page showing “Hello from Azure!”
- Azure Portal > Resource Group view
- App Service configuration page
- Git push success output
