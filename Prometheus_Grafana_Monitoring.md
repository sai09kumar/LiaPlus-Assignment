#  Monitoring & Logging Setup with Prometheus & Grafana

This guide helps you set up a complete monitoring stack using **Prometheus** and **Grafana** to track application performance and errors.

---

##  Deliverables

-  Steps to configure monitoring/logging tools
-  Dashboard screenshots showing application metrics
-  Alert setup for critical issues

---

##  Step 1: Create a Sample Node.js Application with Prometheus Metrics

###  Folder Structure

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

###  `package.json`

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

###  `index.js`

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

###  `Dockerfile`

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

##  Step 2: Update Docker Compose and Prometheus Config

### `docker-compose.yml`

Add this under services:

```yaml
  app:
    build: ./app
    ports:
      - "8080:8080"
```

###  `prometheus.yml`

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

## ▶ Step 3: Run the Stack

```bash
docker-compose down
docker-compose up --build -d
```

- Your app: `http://localhost:8080`
- App metrics: `http://localhost:8080/metrics`
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`

---

##  Step 4: Grafana Dashboard & Alerts

- Add Prometheus as a data source in Grafana
- Create a dashboard panel using the metric: `http_requests_total`
- Add a graph to visualize requests per path/method
- Configure alert rules for:
  - Sudden spike in requests
  - App unavailability (missing metrics)

---

##  Screenshots to Include

Insert screenshots showing:
- Sample app running
- `/metrics` endpoint output
- Prometheus showing `sample-app`
- Grafana dashboard with charts
- Alert rule config and alerting output

![WhatsApp Image 2025-04-14 at 12 34 07_ab3582df](https://github.com/user-attachments/assets/376f07e6-ff03-4d60-9996-8a9fa6032521)

![WhatsApp Image 2025-04-14 at 12 02 57_43e0716c](https://github.com/user-attachments/assets/425772b0-a0ba-491a-a570-6b91def7475a)

![WhatsApp Image 2025-04-14 at 12 04 41_71805f54](https://github.com/user-attachments/assets/42982c9f-0dda-4b54-9435-12391027cc75)

![WhatsApp Image 2025-04-14 at 12 05 36_4a20f604](https://github.com/user-attachments/assets/613838f6-193d-40cc-9a5d-bf2daf23d7a8)

![WhatsApp Image 2025-04-14 at 12 11 00_d1e6267a](https://github.com/user-attachments/assets/f034bcf8-8eb6-4cd0-a6dc-aaa0673c72dd)

![WhatsApp Image 2025-04-14 at 12 29 32_7ab93081](https://github.com/user-attachments/assets/fa0055ef-0d75-4353-8f1e-e912d720eff3)







---

## 📦 Outcome

✅ Full monitoring setup for custom Node.js app  
✅ Application metrics visualized in Grafana  
✅ Prometheus alerting integrated  
