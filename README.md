# Sock Shop : A Microservice Demo Application HA

Fork from https://github.com/microservices-demo/microservices-demo
HA release

https://github.com/argoproj/argo-cd/blob/master/manifests/ha/namespace-install.yaml

helm repo add argo https://argoproj.github.io/argo-helm

Pre-requisites
Please complete the steps before proceeding with installation argocd  in High Availability mode.



Installation Steps
Step 1: Create namespace
All argocd components are deployed in a separate namespace inside Kubernetes called 'argocd'. Create the namespace using command.

kubectl create namespace argocd
Step 2: To Install the argocd


kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.6.7/manifests/ha/install.yaml
Change the service type cluster IP to Node Port.

kubectl -n argocd patch svc argocd-server -p '{"spec": {"type": "NodePort"}}'
To get the port of argocd-server service.

kubectl -n argocd  get svc argocd-server  -o go-template='{{(index .spec.ports 1).nodePort}}';echo;
To get the default password use the following command.

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
To get all the pod's names use the following command

kubectl get po -n argocd
first, we need to execute the pod in order to change the password.

kubectl exec -it argocd-server{pod-name}  -n argocd -- bash
login to argocd cli use the following command

argocd login {IP-OF-THE-Machine}:{Argocd-CD-Service-Port}
use the following command to change the password

argocd account update-password
ArgoCD Web UI
To access the argocd from the browser

https://{IP-OF-THE-Machine}:{Argocd-CD-Service-Port}
username : admin
password: yournewpassword

argocd
deploy/kubernetes/manifests/
