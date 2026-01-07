# Commands Executed

## System Prep
apt update && apt upgrade -y

## Docker Installation
apt install -y ca-certificates curl gnupg
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
apt install -y docker-ce docker-ce-cli containerd.io
systemctl enable docker
systemctl start docker

## k3s Installation
curl -sfL https://get.k3s.io | sh -
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

## Validation
docker version
docker ps
kubectl get nodes
kubectl get pods -A

## Helm Installation
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

## Open WebUI Deployment
helm repo add open-webui https://helm.openwebui.com/
helm repo update
kubectl create namespace openwebui
helm install webui open-webui/open-webui --namespace openwebui --set service.type=ClusterIP --dry-run
helm install webui open-webui/open-webui --namespace openwebui --set service.type=ClusterIP

## OIDC Configuration
helm upgrade webui open-webui/open-webui --namespace openwebui --values values-oidc.yaml
