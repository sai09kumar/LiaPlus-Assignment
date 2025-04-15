# 📊 Monitoring & Logging Setup with Prometheus & Grafana

This guide helps you set up a complete monitoring stack using **Prometheus** and **Grafana** to track application performance and errors.

---

## ✅ Deliverables

- ✅ Steps to configure monitoring/logging tools
- ✅ Dashboard screenshots showing application metrics
- ✅ Alert setup for critical issues

---

## 🧱 Step 1: Create a Sample Node.js Application with Prometheus Metrics

### 📁 Folder Structure

```
devops-monitoring/
│
├── docker-compose.yml
├── prometheus.yml
├── alert.rules.yml
└── app/
    ├── Dockerfile
    ├── index.js
    └── package.json
```

### 📄 `package.json`

```json
{
  "name": "sample-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "prom-client": "^14.1.0"
  }
}
```

### 📄 `index.js`

```javascript
const express = require('express');
const client = require('prom-client');

const app = express();
const port = 8080;

const register = client.register;
const httpRequestCounter = new client.Counter({
  name: 'http_requests_total',
  help: 'Total number of HTTP requests',
  labelNames: ['method', 'path'],
});

app.use((req, res, next) => {
  httpRequestCounter.inc({ method: req.method, path: req.path });
  next();
});

app.get('/', (req, res) => {
  res.send('Hello from the monitored app!');
});

app.get('/metrics', async (req, res) => {
  res.set('Content-Type', register.contentType);
  res.end(await register.metrics());
});

app.listen(port, () => {
  console.log(`App running on http://localhost:${port}`);
});
```

### 📄 `Dockerfile`

```Dockerfile
FROM node:18

WORKDIR /app

COPY package.json ./
RUN npm install

COPY . .

EXPOSE 8080

CMD ["npm", "start"]
```

---

## 🧰 Step 2: Update Docker Compose and Prometheus Config

### 📄 `docker-compose.yml`

Add this under services:

```yaml
  app:
    build: ./app
    ports:
      - "8080:8080"
```

### 📄 `prometheus.yml`

```yaml
global:
  scrape_interval: 15s

rule_files:
  - "alert.rules.yml"

scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']

  - job_name: 'sample-app'
    metrics_path: /metrics
    static_configs:
      - targets: ['app:8080']
```

---

## ▶️ Step 3: Run the Stack

```bash
docker-compose down
docker-compose up --build -d
```

- Your app: `http://localhost:8080`
- App metrics: `http://localhost:8080/metrics`
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`

---

## 📈 Step 4: Grafana Dashboard & Alerts

- Add Prometheus as a data source in Grafana
- Create a dashboard panel using the metric: `http_requests_total`
- Add a graph to visualize requests per path/method
- Configure alert rules for:
  - Sudden spike in requests
  - App unavailability (missing metrics)

---

## 🖼 Screenshots to Include

Insert screenshots showing:
- Sample app running
- `/metrics` endpoint output
- Prometheus showing `sample-app`
- Grafana dashboard with charts
- Alert rule config and alerting output

```markdown
![App Running](./screenshots/app-homepage.png)
![Metrics](./screenshots/app-metrics.png)
![Prometheus Job](./screenshots/prometheus-app-job.png)
![Grafana Chart](./screenshots/grafana-app-dashboard.png)
![Alert Rule](./screenshots/alert-setup.png)
```

---

## 📦 Outcome

✅ Full monitoring setup for custom Node.js app  
✅ Application metrics visualized in Grafana  
✅ Prometheus alerting integrated  
