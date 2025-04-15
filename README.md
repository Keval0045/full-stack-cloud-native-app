
# BestBuy Cloud-Native Application - Full Stack Deployment (Kubernetes + Azure AI)

Welcome to the **BestBuy App** — a full-stack, cloud-native microservices application deployed using **Azure Kubernetes Service (AKS)** and integrated with **Azure OpenAI** for AI-generated product descriptions and images.

---

## Project Overview

This project demonstrates a scalable e-commerce system powered by:
- **Kubernetes** for container orchestration  
- **Microservices architecture** (order, product, makeline, admin, and AI services)  
- **Azure OpenAI Services** (GPT-4 & DALL·E 3)  
- **MongoDB & RabbitMQ** as core backend services  

---

## Features

- AI-powered product generation (descriptions + images)  
- Admin dashboard for product management  
- Customer storefront with real-time product data  
- Simulated users and workers  
- Scalable and observable deployments

---

## Architecture

```
+-----------------------------+
|      Azure Kubernetes       |
|           (AKS)             |
+-------------+---------------+
              |
     -------------------
    |    Microservices    |
     -------------------
    | - store-front       | --> Customer UI
    | - store-admin       | --> Admin UI
    | - product-service   |
    | - order-service     |
    | - makeline-service  |
    | - ai-service        |
    | - virtual-customer  |
    | - virtual-worker    |
     -------------------
              |
     -------------------
    |   Azure OpenAI API  |
    | (GPT-4 + DALL·E 3)  |
     -------------------
              |
     -------------------
    |   MongoDB + RabbitMQ|
     -------------------
```
![Algonquin Pet Store On Steroids](https://github.com/user-attachments/assets/b967111f-4d45-4017-8b14-a85c4d102154)

---

## Docker Images

| Service             | Docker Image |
|---------------------|--------------|
| Store Front         | [keval0045/store-front1](https://hub.docker.com/r/keval0045/store-front) |
| Store Admin         | [keval0045/store-admin](https://hub.docker.com/r/keval0045/store-admin) |
| Order Service       | [keval0045/order-service](https://hub.docker.com/r/keval0045/order-service) |
| Product Service     | [keval0045/product-service](https://hub.docker.com/r/keval0045/product-service) |
| Makeline Service    | [keval0045/makeline-service](https://hub.docker.com/r/keval0045/makeline-service) |
| AI Service          | [keval0045/ai-service](https://hub.docker.com/r/keval0045/ai-service) |

---

## Prerequisites

- Azure Subscription with credits  
- Azure CLI + AKS setup  
- `kubectl`  
- Access to Azure OpenAI (GPT-4 + DALL·E 3 enabled)

---

## Installation Guide

### Step 1: Set Up AKS Cluster

Create a Kubernetes cluster with:
- 1 Node Pool
- 2+ Linux worker nodes  
Refer to your Lab 6 guide for AKS setup.

---

### Step 2: Clone Deployment Files

Clone the Kubernetes manifests repository containing:
- `aps-all-in-one.yaml`  
- `secrets.yaml`  
- `config-maps.yaml`  
- `admin-tasks.yaml`

---

### Step 3: Set Up Azure OpenAI

1. **Create Azure OpenAI Resource**  
   - Region: **East US**  
   - Deploy GPT-4 and DALL·E 3

2. **Get Keys & Endpoints**  
   - Go to Keys and Endpoints  
   - Base64-encode API Key:

```bash
echo -n "your-api-key" | base64
```

3. **Update Secrets**

Edit `secrets.yaml`:
```yaml
OPENAI_API_KEY: <base64-encoded-key>
```

Edit `aps-all-in-one.yaml` environment section:
```yaml
- name: AZURE_OPENAI_API_VERSION
  value: "2024-07-01-preview"
- name: AZURE_OPENAI_DEPLOYMENT_NAME
  value: "gpt-4-deployment"
- name: AZURE_OPENAI_ENDPOINT
  value: "https://<resource>.openai.azure.com/"
- name: AZURE_OPENAI_DALLE_ENDPOINT
  value: "https://<resource>.openai.azure.com/"
- name: AZURE_OPENAI_DALLE_DEPLOYMENT_NAME
  value: "dalle-3-deployment"
```

---

### Step 4: Deploy Application

```bash
kubectl apply -f config-maps.yaml
kubectl apply -f secrets.yaml
kubectl apply -f aps-all-in-one.yaml
```

Verify pods and services:

```bash
kubectl get pods
kubectl get services
```

---

### Step 5: Test UI Access

Get external IP for services:

```bash
kubectl get svc
```

Visit:
- **Customer Storefront** → `http://<external-ip>:80`
- **Admin Dashboard** → `http://<external-ip>:80`

---

### Step 6: Simulate Traffic

```bash
kubectl apply -f admin-tasks.yaml
kubectl logs -f deployment/virtual-customer
kubectl logs -f deployment/virtual-worker
```

---

### Step 7: Scale & Monitor

```bash
kubectl scale deployment order-service --replicas=3
kubectl top pods
kubectl top nodes
```

---

### Step 8: MongoDB & RabbitMQ Access

- **MongoDB CLI**:
```bash
kubectl exec -it <mongodb-pod-name> -- mongo
```

- **RabbitMQ UI**:
```bash
kubectl port-forward service/rabbitmq 15672:15672
```

---
https://youtu.be/nUjwltxGk4U

