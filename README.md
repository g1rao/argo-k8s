# argo-k8s: Argo CD on Rancher Desktop as a local GitOps K8s lab

## Service:
 A Kubernetes Service that identifies a set of Pods using label selectors.
## Edge router: 
A router that enforces the firewall policy for your cluster. 

## Ingress:
 exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.


## Install ArgoCD
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## Deploy the additional configmap and redeploy the app for insecure

    kubectl -n argocd apply -f argocd-cmd-params-cm.yaml
    kubectl -n argocd rollout restart deployment argocd-server
    kubectl -n argocd rollout status deployment argocd-server


## Deploy the ingress
    kubectl get all,ing
    kubectl -n argocd apply -f argocd-ingress-server.yaml

##  argocd Admin Password
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

    username: admin

    Open http://argocd.rancher.localhost/
    
## Usage
The declarative yaml manifests in this repo are:
jade-shooter-deployment.yaml: deploys a scalable deployment of a simple app which creates a scalable number of K8s pods which respond to port 80 and encapsulate a container
jade-shooter-service.yaml: creates a service that allows this webapp's port 80 to communicate outside of its K8s namespace (AKA dedicated secure cluster) via port 8080
jade-shooter-ingress.yaml: creates an ingress that exposes the service to requests outside of the K8s cluster at https://jade-shooter.rancher.localhost




Manual Apply:
  Apply the manifests in this repo

  cd k8s-app/
  kubectl apply -f .
  Browse to https://jade-shooter.rancher.localhost

