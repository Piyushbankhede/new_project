# Microservice-project

# Streamlining Microservices with EKS, ArgoCD, and Istio

This project demonstrates how to deploy and manage microservices using Amazon Elastic Kubernetes Service (EKS), ArgoCD for GitOps, and Istio as a service mesh.

## 1. Setting Up the Environment

### AWS Configuration
Ensure your AWS CLI is properly configured:

```bash
aws configure
```

Provide your AWS Access Key, Secret Key, region, and output format.

### EKS Cluster Creation
Create an EKS cluster using `eksctl`:

```bash
eksctl create cluster --name microservices-cluster
```

## 2. Implementing GitOps with ArgoCD

### ArgoCD Installation
Deploy ArgoCD in the Kubernetes cluster:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Accessing ArgoCD
Set the context and access the ArgoCD server:

```bash
kubectl config set-context --current --namespace=argocd
argocd login --core
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

## 3. Enhancing with Istio Service Mesh

### Istio Installation
Install Istio for enhanced traffic management and security:

```bash
curl -L https://istio.io/downloadIstio | sh -
export PATH=$PWD/bin:$PATH
istioctl install --set profile=demo -y
```

### Enable Istio Injection
Enable automatic sidecar injection:

```bash
kubectl label namespace default istio-injection=enabled
```

## 4. Deploying Applications with ArgoCD

Deploy your microservices application using ArgoCD:

```bash
argocd app create microservices-app --repo <repository_url> --path <app_path> --dest-server https://kubernetes.default.svc --dest-namespace default
argocd app sync microservices-app
```

## 5. Monitoring and Observability with Prometheus and Grafana

Deploy Prometheus and Grafana for monitoring:

```bash
kubectl apply -f istio-1.24.2/samples/addons/
kubectl port-forward -n istio-system deployment/prometheus 9090:9090
kubectl port-forward -n istio-system deployment/grafana 3000:3000
```

## Conclusion
This setup provides a scalable and efficient environment for deploying and managing microservices using Kubernetes, GitOps, and a service mesh. By combining these tools, you can achieve continuous deployment, enhanced security, and comprehensive observability.

---

Feel free to explore and customize this project further!
