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