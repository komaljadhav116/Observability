# Installation & Configuration

This guide explains how to create an Amazon EKS cluster and install the **kube-prometheus-stack** (Prometheus, Grafana, and Alertmanager) for Kubernetes observability.

---

# Step 1: Create an Amazon EKS Cluster

## Prerequisites

Before creating the cluster, install and configure the following tools:

- AWS CLI
- eksctl
- kubectl
- Helm

Configure AWS CLI:

```bash
aws configure
```

---

## Create the EKS Cluster

```bash
eksctl create cluster \
  --name=observability \
  --region=us-east-1 \
  --zones=us-east-1a,us-east-1b \
  --without-nodegroup
```

---

## Associate IAM OIDC Provider

```bash
eksctl utils associate-iam-oidc-provider \
  --region us-east-1 \
  --cluster observability \
  --approve
```

---

## Create a Managed Node Group

```bash
eksctl create nodegroup \
  --cluster=observability \
  --region=us-east-1 \
  --name=observability-ng-private \
  --node-type=t3.medium \
  --nodes-min=2 \
  --nodes-max=3 \
  --node-volume-size=20 \
  --managed \
  --asg-access \
  --external-dns-access \
  --full-ecr-access \
  --appmesh-access \
  --alb-ingress-access \
  --node-private-networking
```

---

## Update kubeconfig

```bash
aws eks update-kubeconfig --name observability
```

---

# Step 2: Install kube-prometheus-stack

Add the Prometheus Helm repository.

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update the repository.

```bash
helm repo update
```

---

# Step 3: Deploy kube-prometheus-stack

Create a namespace.

```bash
kubectl create namespace monitoring
```

Move to the directory containing the Helm values file.

```bash
cd day-2
```

Install the Helm chart.

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  -n monitoring \
  -f ./custom_kube_prometheus_stack.yml
```

---

# Step 4: Verify the Installation

Check all resources.

```bash
kubectl get all -n monitoring
```

---

# Access Prometheus

Forward the Prometheus service.

```bash
kubectl port-forward service/prometheus-operated \
  -n monitoring \
  9090:9090
```

Open:

```
http://localhost:9090
```

> **Note:** If you are using an EC2 instance or cloud VM, use:

```bash
kubectl port-forward service/prometheus-operated \
  -n monitoring \
  9090:9090 \
  --address 0.0.0.0
```

Then access:

```
http://<EC2-Public-IP>:9090
```

---

# Access Grafana

Port-forward the Grafana service.

```bash
kubectl port-forward service/monitoring-grafana \
  -n monitoring \
  8080:80
```

Open:

```
http://localhost:8080
```

## Default Credentials

**Username**

```
admin
```

**Password**

```
prom-operator
```

---

## Retrieve Grafana Credentials

Username

```bash
kubectl get secret monitoring-grafana \
  -n monitoring \
  -o jsonpath='{.data.admin-user}' | base64 -d
```

Password

```bash
kubectl get secret monitoring-grafana \
  -n monitoring \
  -o jsonpath='{.data.admin-password}' | base64 -d
```

---

# Access Alertmanager

```bash
kubectl port-forward service/alertmanager-operated \
  -n monitoring \
  9093:9093
```

Open:

```
http://localhost:9093
```

---

# Step 5: Cleanup

## Uninstall Helm Chart

```bash
helm uninstall monitoring --namespace monitoring
```

---

## Delete Namespace

```bash
kubectl delete namespace monitoring
```

---

## Delete the EKS Cluster

```bash
eksctl delete cluster --name observability
```

---

# Architecture

```
AWS
│
├── Amazon EKS
│   ├── Worker Nodes
│   ├── Prometheus
│   ├── Grafana
│   ├── Alertmanager
│   └── Kubernetes Metrics
│
└── Users
     ├── Prometheus UI (9090)
     ├── Grafana UI (8080)
     └── Alertmanager UI (9093)
```