
# CI/CD Pipeline with Azure Deployment for Node.js App 🚀

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

## 2. CI/CD Pipeline Implementation

### CI/CD Using GitHub Actions

Create a workflow file: `.github/workflows/deploy.yml`

```yaml
name: Node.js CI/CD

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests (if any)
        run: echo "No tests defined"

      - name: Deploy to Azure Web App
        uses: azure/webapps-deploy@v2
        with:
          app-name: ${{ secrets.AZURE_WEBAPP_NAME }}
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
          package: .
```

### Pipeline Stages Explained

- **Build**: Installs dependencies.
- **Test**: Placeholder stage to run any defined tests.
- **Deploy**: Uses Azure Web Apps GitHub Action for deployment.

### Secrets Management

- Use GitHub Repository Secrets:
  - `AZURE_WEBAPP_NAME`: Your App Service name.
  - `AZURE_WEBAPP_PUBLISH_PROFILE`: Export from Azure portal (App Service > Get Publish Profile).

---

## 3. Azure Configurations Used

- **Resource Group**: `azure-node-rg`
- **App Service Plan**: `nodeAppPlan` (SKU: FREE)
- **Web App Name**: Your unique app name
- **Region**: East US (or your preferred region)

---

## 4. Deployment Steps

### Azure CLI Deployment

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
