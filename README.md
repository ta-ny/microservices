Microservice Deployment Configurations

This repository provides examples of deploying a microservices application using three different approaches.

Structure
microservice/
├── git-repo/          # Plain Kubernetes manifests
├── helm-chart/        # Helm Chart
└── kustomize/         # Kustomize configuration
1. Git Repository Method (Plain Manifests)

The most basic deployment method using standard Kubernetes manifest files.

Usage
kubectl apply -f git-repo/microservice.yaml
# kubectl apply -f ../microservice/git-repo/microservice.yaml

kubectl get pods
kubectl port-forward svc/apache 8080:80
2. Helm Chart Method

Deploy using a templated Helm Chart for reusable and configurable installations.

Usage
# Install (you can choose any release name)
helm install microservice-demo helm-chart/

# Verify installation
helm list
kubectl get pods

# Upgrade (after modifying values.yaml)
helm upgrade microservice-demo helm-chart/

# Uninstall
helm uninstall microservice-demo

# Package chart (creates a .tgz file)
helm package helm-chart/
# Output: microservice-demo-1.0.0.tgz
3. Kustomize Method

Apply environment-specific configurations using Kustomize overlays.

Usage
# Apply base configuration
kubectl apply -k kustomize/base/

# Apply development environment
kubectl apply -k kustomize/overlays/dev/

# Apply production environment
kubectl apply -k kustomize/overlays/prod/
Accessing the Application
Local Port Forward
kubectl port-forward svc/apache 8080:80

Then open your browser and navigate to:

http://localhost:8080
Service Architecture

apache: Frontend service (Port 80)

catalog: Catalog service (Port 8080)

customer: Customer service (Port 8080)

order: Order service (Port 8080)
