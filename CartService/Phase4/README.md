📦 Phase 4 – Kubernetes & AKS Deployment (Learning Phase)
🎯 Phase Goal

# In Phase 4, we transitioned from App Service–based container deployments to Kubernetes.
The objective was not speed, but understanding:

* Why Kubernetes is used for microservices

* How Pods, Services, ConfigMaps, and Secrets work together

* How local Kubernetes (Minikube) maps to Azure Kubernetes Service (AKS)

* How CI/CD deploys manifests to AKS

* This phase intentionally stopped short of advanced topics (Terraform, Helm, rollbacks) to avoid overload.

🧠 Key Concepts Learned
# Why Kubernetes (and AKS)?

   # App Services are great for:

  * Simple container hosting

  * Low operational overhead

  * Kubernetes is better for:

  * Multiple microservices

  * Independent scaling

  * Service-to-service communication

  * Standardized deployment patterns

  * Cloud portability

# AKS = Managed Kubernetes
# You don’t manage:

* Control plane

* etcd

* API server upgrades

* You do manage:

* Nodes (to a degree)

* Deployments

* Networking

* CI/CD

🏗 Architecture (Phase 4)
[ CartService Pod ] ---> [ Redis Pod ]
        |
        v
   Kubernetes Service
        |
   NodePort / LoadBalancer

Components Used

Deployment – Manages Pods

Service – Stable networking

ConfigMap – Non-secret config

Secret – Credentials

AKS – Managed Kubernetes cluster

Azure Container Registry (ACR) – Docker images

🗂 Repository Structure
repo-root/
│
├── src/
│   └── Dockerfile.basic
│
├── k8s/
│   ├── cartservice-configmap.yaml
│   ├── cartservice-secret.yaml
│   ├── cartservice-deployment.yaml
│   ├── redis-deployment.yaml
│
└── azure-pipelines.yaml

🧪 Local Kubernetes (Minikube)
1️⃣ Start Minikube
minikube start

2️⃣ Use Minikube Docker Engine
eval $(minikube docker-env)

3️⃣ Build Image Locally

From k8s/ directory:

docker build -t cartservice:basic -f ../src/Dockerfile.basic ..

4️⃣ Apply Kubernetes Manifests
kubectl apply -f cartservice-configmap.yaml
kubectl apply -f cartservice-secret.yaml
kubectl apply -f redis-deployment.yaml
kubectl apply -f cartservice-deployment.yaml

5️⃣ Access the Service
minikube service cartservice

🔑 Configuration Patterns
ConfigMap (example)

Used to inject non-secret runtime config

apiVersion: v1
kind: ConfigMap
metadata:
  name: cartservice-config
data:
  REDIS_ADDR: "redis:6379"

Secret (example)

Used for credentials

apiVersion: v1
kind: Secret
metadata:
  name: cartservice-secret
type: Opaque
data:
  cartservice-user: Y2FydHNlcnZpY2U=
  cartservice-password: Y2FydHNlcnZpY2VwYXNzd29yZA==

☸ AKS Deployment
Key Difference from Minikube

Images must come from ACR

cartservice:basic ❌

obacrregistry123.azurecr.io/cartservice:<tag> ✅

Deployment Image Example
image: obacrregistry123.azurecr.io/cartservice:latest

🚀 CI/CD Integration (Azure DevOps)
Docker Build & Push (CI)

Builds image

Pushes to ACR

Tags image

Kubernetes Deploy (CD)

Uses KubernetesManifest@1

- task: KubernetesManifest@1
  inputs:
    action: deploy
    connectionType: azureResourceManager
    azureSubscriptionConnection: AKS-connection
    azureResourceGroup: KM1-rg
    kubernetesCluster: OB-AKS-Cluster
    manifests: 'k8s/*.yaml'


✔ kubectl apply is handled automatically
✔ Manifests stay source-controlled
✔ Environment parity between local & cloud

🔍 Verification Commands
kubectl get pods
kubectl get svc
kubectl describe pod <pod-name>
kubectl logs <pod-name>


Expected:

CartService pod: Running

Redis pod: Running

Logs show: Now listening on ...

📌 Image Tagging (Preview)

You noticed image tag confusion — that’s expected.

Next phase will introduce:

Immutable image tags (commit SHA)

:latest avoidance

Kustomize overlays

Environment-specific manifests

🧠 What We Intentionally Did NOT Cover (Yet)

Rollback strategies

Helm

Terraform

Autoscaling

Ingress controllers

Service Mesh

These come after core Kubernetes mental models are solid.

✅ Phase 4 Outcome

✔ Understood Kubernetes primitives
✔ Deployed microservices locally
✔ Deployed to AKS via CI/CD
✔ Debugged real-world image pull issues
✔ Built strong mental models

➡ What’s Next (Phase 5 Preview)

Image versioning strategy

Kustomize overlays (base/, dev/, prod/)

Environment isolation

Clean promotion model

(Later) Rollbacks & scaling