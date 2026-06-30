# ShopNow Infra - GitOps Infrastructure Repository

This repository contains all Helm charts and ArgoCD configurations for the ShopNow e-commerce platform. ArgoCD watches this repo and automatically syncs changes to the Kubernetes cluster.

## GitOps Flow

```
shopnow-app CI Pipeline → Updates image tag in this repo →
ArgoCD detects change → Syncs Helm chart → New pod deployed
```

## Tech Stack

- **GitOps Tool:** ArgoCD
- **Package Manager:** Helm
- **Orchestration:** Kubernetes (Docker Desktop)
- **Database:** PostgreSQL (StatefulSet)
- **Cache:** Redis (StatefulSet)
- **Ingress:** Nginx Ingress Controller
- **Monitoring:** Prometheus + Grafana

## Repository Structure

```
shopnow-infra/
├── helm/
│   ├── user-service/          # Helm chart - User Service
│   ├── product-service/       # Helm chart - Product Service
│   ├── order-service/         # Helm chart - Order Service
│   ├── payment-service/       # Helm chart - Payment Service
│   ├── notification-service/  # Helm chart - Notification Service
│   ├── frontend/              # Helm chart - React Frontend
│   └── databases.yaml         # PostgreSQL + Redis StatefulSets
└── argocd/
    ├── namespace.yaml          # shopnow namespace
    ├── databases-app.yaml      # ArgoCD app - Databases
    ├── services-app.yaml       # ArgoCD apps - All microservices
    └── ingress.yaml            # Nginx Ingress rules
```

## ArgoCD Applications

| App | Namespace | Sync Policy |
|-----|-----------|-------------|
| shopnow-databases | shopnow | Auto |
| shopnow-user-service | shopnow | Auto |
| shopnow-product-service | shopnow | Auto |
| shopnow-order-service | shopnow | Auto |
| shopnow-payment-service | shopnow | Auto |
| shopnow-notification-service | shopnow | Auto |
| shopnow-frontend | shopnow | Auto |

## Access

| Service | URL |
|---------|-----|
| ShopNow App | http://localhost |
| ArgoCD UI | https://argocd.local |
| Grafana | http://grafana.local |

## Deployment

ArgoCD automatically deploys when this repo is updated. To manually sync:

```bash
kubectl apply -f argocd/namespace.yaml
kubectl apply -f argocd/databases-app.yaml
kubectl apply -f argocd/services-app.yaml
kubectl apply -f argocd/ingress.yaml
```

## Related Repository

Application Code & CI Pipeline → [shopnow-app](https://github.com/pixelopscloud/shopnow-app)
