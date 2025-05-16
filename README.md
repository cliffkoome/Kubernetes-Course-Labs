# Widgetario Kubernetes Hackathon

## Overview  
Widgetario is a fictitious gadget retailer. This repository contains a step-by-step, end-to-end Kubernetes deployment of the Widgetario application, covering:

1. **Part 1**: Basic Deployments & Services  
2. **Part 2**: Configuration (ConfigMaps & Secrets)  
3. **Part 3**: Storage (StatefulSets & Volumes)  
4. **Part 4**: Ingress (HTTP routing & DNS)  
5. **Part 5**: Production Hardening (probes, resource limits, security)  
6. **Part 6**: Observability (Prometheus/Grafana & EFK)  
7. **Part 7**: CI/CD (Helm charts & Jenkins pipeline)  

## Repository Structure  

├── part-1/ # Deployments & Services
│   ├── products-db/
│   ├── products-api/
│   ├── stock-api/
│   └── web/
├── part-2/ # ConfigMaps & Secrets
│   ├── products-db/
│   ├── products-api/
│   ├── stock-api/
│   └── web/
├── part-3/ # StatefulSets & Volumes
│   ├── products-db/
│   ├── products-api/
│   └── stock-api/
├── part-4/ # Ingress YAMLs
│   ├── ingress.yaml
│   └── products-api/service.yaml
├── part-5/ # Probes & Security contexts
│   └── ...
├── part-6/ # Monitoring & Logging
│   └── ...
├── part-7/ # Helm chart & CI/CD
│   └── helm-chart/
│       └── ...
├── hackathon/files/ # Dashboards, values files, scripts
└── README.md # This file

---

## Prerequisites & Dependencies  
- **kubectl** v1.24+  
- A local cluster (Minikube, Kind, Docker Desktop) or a cloud-provided Kubernetes cluster  
- **Helm** v3+ (for Part 7)  
- **Jenkins** server with Gogs & BuildKit configured (optional for CI/CD)  
- **Docker** CLI (to verify/pull images)  

---

## Installation & Setup  

1. **Clone the repo**  
   ```bash
   git clone https://github.com/<your-org>/widgetario-k8s-hackathon.git
   cd widgetario-k8s-hackathon

2. Start your cluster
  Minikube: minikube start --driver=docker
  Docker Desktop: ensure Kubernetes is enabled

4. Install NGINX Ingress Controller (for Part 4 onwards)
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.1/deploy/static/provider/cloud/deploy.yaml

# Usage Guidelines

## Part 1 – Deployments & Services
`
kubectl apply -f part-1/products-db/
kubectl apply -f part-1/products-api/
kubectl apply -f part-1/stock-api/
kubectl apply -f part-1/web/
kubectl port-forward svc/web 8080:80
# Browse http://localhost:8080
`

## Part 2 – Configuration
`
# Create Secrets & ConfigMaps, then:
kubectl apply -f part-2/products-db/
kubectl apply -f part-2/products-api/
kubectl apply -f part-2/stock-api/
kubectl apply -f part-2/web/
`

## Part 3 – Storage
`
kubectl delete deploy/products-db svc/products-db pvc -l app=products-db  # cleanup
kubectl apply -f part-3/products-db/
kubectl apply -f part-3/products-api/
kubectl apply -f part-3/stock-api/
`

## Part 4 – Ingress
`
kubectl apply -f part-4/ingress.yaml
# Update /etc/hosts or Windows hosts:
#   127.0.0.1 widgetario.local api.widgetario.local
curl http://widgetario.local
curl http://api.widgetario.local/products
`

## Part 5, 6, 7
See folder README files under part-5/, part-6/, and part-7/ for detailed commands.

# Code Documentation
All YAML manifests are commented to explain key sections (labels, selectors, ports).

Naming conventions: each Deployment, Service, ConfigMap, Secret, and Ingress uses `app: <component-name>` labels.

## Testing
A basic smoke-test script is provided under `widgetario-k8s/scripts/test.sh.` It:
1. Verifies Pods reach Running
2. Checks Service endpoints
3. Queries the web UI and Products API

## Deployment Instructions
To deploy a production-style release via Helm (Part 7):
`
cd part-7/helm-chart
helm install widgetario ./ \
  --set image.tag=1.0.0 \
  --set ingress.enabled=true \
  --values hackathon/files/helm/uat.yaml
`

To uninstall:
`helm uninstall widgetario`
