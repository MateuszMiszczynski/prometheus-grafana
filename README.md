# Web Application Monitoring with Prometheus & Grafana

This project demonstrates a monitoring stack for a containerized FastAPI application using Prometheus and Grafana.  
The goal was to learn how modern systems collect, store, and visualize application metrics using industry-standard observability tools.

The environment is orchestrated with Docker Compose, where Prometheus collects metrics from the application and Grafana visualizes them in a monitoring dashboard.

---

# Project Architecture

The stack consists of three main components.

## FastAPI Application

A Python web service exposing runtime and HTTP metrics through the `/metrics` endpoint in Prometheus format.

## Prometheus

A time-series database responsible for:

- scraping metrics from the FastAPI application
- storing them as time-series data
- providing a query interface using PromQL

## Grafana

A visualization platform used to build dashboards and analyze system behavior.

---

# Monitoring Methodologies

The dashboard is based on two commonly used monitoring frameworks.

## RED Method

Focuses on user-facing metrics:

- **Rate** – number of requests per second
- **Errors** – failed requests
- **Duration** – request latency

## USE Method

Focuses on system resources:

- **Utilization**
- **Saturation**
- **Errors**

Using both approaches provides visibility into both system health and user experience.

---

# PromQL Queries

The dashboard uses Prometheus queries to analyze application performance.

## Request Rate

```promql
rate(http_request_total[1m])
```

## Average Response Time

```promql
rate(http_request_duration_seconds_sum[1m]) /
rate(http_request_duration_seconds_count[1m])
```

## Latency Percentiles

```promql
histogram_quantile(0.99,
sum(rate(http_request_duration_seconds_bucket[5m])) by (le))
```

These queries help detect performance issues such as slow endpoints or increased error rates.

---

# Traffic Simulation

To validate the monitoring setup, application traffic was generated using Postman.

The repository includes a Postman collection:

```
fastapi-collection.json
```

It was used to simulate:

- successful API requests
- invalid requests producing **404 errors**
- different CRUD operations

This allowed Prometheus to collect realistic metrics and populate the Grafana dashboard.

---

# Dashboard Preview

Below are screenshots of the monitoring dashboard showing key metrics:

- Requests
- Errors
- Duration (latency)
- System metrics

<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/4e5ed595-21de-4dfd-a4d4-fbfa045ae9c9" />
<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/62498d45-4d5e-4a15-8fb5-6ca94e63fc9d" />
<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/4b90427f-6d2b-4721-a29c-212bfd2e18d6" />
<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/73e1c90b-d9c8-4529-bc71-346b04ce6a9f" />


And here are additional screenshots from other components of the monitoring stack.

# Prometheus

<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/fcb8bb18-475f-4fb7-b137-f6f41860cf7f" />

# FastAPI Metrics Endpoint

<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/19e10af9-d581-4246-bf3b-156c23d6621f" />

# Postman Traffic Simulation

<img width="945" height="532" alt="image" src="https://github.com/user-attachments/assets/53bcbbee-5b76-4b8b-bef2-d5ecc1f65e86" />




---

# Running the Project

## 1. Start the containers

```bash
docker compose up
```

## 2. Open Grafana

```
http://localhost:3000
```

Login:

```
admin / admin
```

## 3. Add Prometheus Data Source

Go to:

```
Connections -> Data Sources -> Add Data Source
```

Select **Prometheus** and set the URL to:

```
http://prometheus:9090
```

Click **Save & Test**.

## 4. Import the Dashboard

Navigate to:

```
Dashboards -> Import
```

Upload:

```
web-application-monitoring.json
```

Select the Prometheus data source.

The dashboard will start displaying metrics from the FastAPI application.

---

# Technologies

- Docker
- Docker Compose
- FastAPI
- Prometheus
- Grafana
- PromQL
- Postman

---

# Credits

The base environment was adapted from a tutorial by **Rayan Labs**.
Source: **Rayan Labs YouTube Channel**: https://www.youtube.com/@RayanLabs and his **GitHub**: https://github.com/rslim087a/prometheus-docker-compose
