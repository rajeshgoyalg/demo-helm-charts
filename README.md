# Helm Chart: demo-flask-app

A multi-cloud-ready Helm chart for deploying a containerized Python Flask application to Kubernetes clusters. This chart supports AWS EKS (ALB), GCP GKE (GCE Ingress), and Azure AKS (NGINX or AGIC) out-of-the-box, and is ideal for learning, demos, and real-world cloud-native deployments.

---

## Overview

This Helm chart deploys a sample Flask application as:
- A Kubernetes **Deployment** (configurable replica count)
- A **Service** (NodePort by default, configurable)
- An optional **Ingress** resource, with cloud-provider-specific configuration

You can easily customize image, service, ingress, and cloud provider settings via `values.yaml` or Helm CLI overrides.

---

## Chart Structure

```
demo-helm-charts/
├── demo-flask-app/
│   ├── Chart.yaml           # Chart metadata
│   ├── values.yaml          # Default configuration values
│   └── templates/
│       ├── deployment.yaml  # Deployment manifest template
│       ├── service.yaml     # Service manifest template
│       └── ingress.yaml     # Ingress manifest template (multi-cloud logic)
└── README.md                # (This file)
```

---

## Prerequisites

- [Helm 3+](https://helm.sh/docs/intro/install/)
- Access to a Kubernetes cluster (EKS, GKE, AKS, or local)
- [kubectl](https://kubernetes.io/docs/tasks/tools/) configured for your cluster
- (For cloud) Appropriate ingress controllers installed (ALB, GCE, NGINX, or AGIC)

---

## Installation

1. **Clone the repo:**
   ```sh
   git clone https://github.com/your-username/demo-helm-charts.git
   cd demo-helm-charts
   ```
2. **Install the chart:**
   ```sh
   helm install demo-flask-app ./demo-flask-app \
     --set cloudProvider=aws    # or gcp, azure
   ```
   *You can override any value in `values.yaml` with `--set` or by providing a custom YAML file.*

---

## Configuration (values.yaml)

Key configurable parameters:

| Parameter                | Description                                   | Default                        |
|--------------------------|-----------------------------------------------|---------------------------------|
| `appName`                | Name for app resources                        | demo-flask-app                  |
| `image.repository`       | Docker image repository                       | rajeshgoyalg/demo-flask-app     |
| `image.tag`              | Image tag                                     | v20250326-10df4d1               |
| `replicaCount`           | Number of pod replicas                        | 2                               |
| `service.type`           | Service type (NodePort/ClusterIP/LoadBalancer)| NodePort                        |
| `service.port`           | Service port                                  | 80                              |
| `service.targetPort`     | Container port                                | 3000                            |
| `cloudProvider`          | Target cloud: aws, gcp, azure                 | aws                             |
| `ingress.enabled`        | Enable ingress                                | true                            |
| `ingress.path`           | Ingress path                                  | /                               |
| `ingress.pathType`       | Ingress pathType                              | Prefix                          |
| `ingress.<provider>.annotations` | Provider-specific ingress annotations      | See values.yaml                 |

See [`values.yaml`](./demo-flask-app/values.yaml) for all available options and annotation examples for each cloud provider.

---

## Cloud Provider Guidance

- **AWS EKS:**
  - Set `cloudProvider=aws`
  - Requires AWS Load Balancer Controller (ALB)
  - Ingress uses the `alb` class and AWS-specific annotations
- **GCP GKE:**
  - Set `cloudProvider=gcp`
  - Requires GKE Ingress Controller
  - Ingress uses the `gce` class and GCP annotations
- **Azure AKS:**
  - Set `cloudProvider=azure`
  - Supports NGINX Ingress (default) or AGIC (customize as needed)
  - Ingress uses the `nginx` class and Azure annotations

---

## Upgrade & Uninstall

- **Upgrade chart:**
  ```sh
  helm upgrade demo-flask-app ./demo-flask-app \
    --set cloudProvider=gcp
  ```
- **Uninstall chart:**
  ```sh
  helm uninstall demo-flask-app
  ```

---

## Example Usage

- **Install for AWS EKS:**
  ```sh
  helm install demo-flask-app ./demo-flask-app --set cloudProvider=aws
  ```
- **Install for GCP GKE:**
  ```sh
  helm install demo-flask-app ./demo-flask-app --set cloudProvider=gcp
  ```
- **Install for Azure AKS:**
  ```sh
  helm install demo-flask-app ./demo-flask-app --set cloudProvider=azure
  ```
- **Override image tag:**
  ```sh
  helm install demo-flask-app ./demo-flask-app --set image.tag=latest
  ```

---

## Troubleshooting

- **No external endpoint after install:**
  - Ensure the correct ingress controller is installed and running.
  - Check that your cloud provider's ingress controller is supported and configured.
- **Pods not starting:**
  - Check image repository/tag and pull secrets if using a private image.
- **Ingress 404s:**
  - Verify ingress rules and service ports match your application.
- **For more:**
  - Run `kubectl get all`, `kubectl describe <resource>`, and check pod logs.

---

## Contributing

Contributions and improvements are welcome! Please:
- Fork the repo and create a feature branch
- Open an issue or pull request with your proposed changes
- Follow best practices for Helm charts and Kubernetes YAML

---

## License

This project is licensed under the [MIT License](LICENSE).
 demo-helm-charts
demo-helm-charts
