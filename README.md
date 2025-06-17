Argocd-gitops

This repository demonstrates a GitOps workflow for deploying a static NGINX website using Argo CD and Kubernetes. By leveraging declarative configurations stored in Git, this setup ensures consistent and automated deployments.

🧩 Project Structure

The repository is organized as follows:

graphql
Copy
Edit
argocd-gitops/
├── src/                      # Static content for NGINX
├── Dockerfile                # Builds the NGINX image
├── k8s/
│   ├── deployment.yaml       # Kubernetes deployment manifest
│   └── service.yaml          # Kubernetes service manifest
└── argocd-application.yaml   # Argo CD Application manifest
🚀 Prerequisites
Before deploying, ensure you have the following:

A Kubernetes cluster (e.g., Minikube, AKS, EKS, GKE)

kubectl configured to interact with your cluster

Argo CD installed in your cluster

Docker installed for building images

🛠️ Setup Instructions
1. Clone the Repository
bash
Copy
Edit
git clone https://github.com/yourusername/argocd-gitops.git
cd argocd-gitops
2. Build and Push the NGINX Docker Image
bash
Copy
Edit
docker build -t yourusername/nginx-website:latest .
docker push yourusername/nginx-website:latest
Replace yourusername with your Docker Hub username.

3. Apply Kubernetes Manifests
bash
Copy
Edit
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
4. Configure Argo CD Application
bash
Copy
Edit
kubectl apply -f argocd-application.yaml
Argo CD will now monitor the repository and deploy the application as specified in the k8s/ directory.

🔄 Continuous Deployment
With Argo CD, any changes pushed to the k8s/ directory in the Git repository will automatically trigger a deployment in your Kubernetes cluster, ensuring a seamless continuous deployment pipeline.

🧪 Accessing the Application
Once deployed, you can access the NGINX website by exposing the service using a LoadBalancer or NodePort, depending on your Kubernetes setup.
