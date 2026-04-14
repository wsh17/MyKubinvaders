# MyKubinvaders

Steps to deploy Kubeinvaders

helm repo add kubeinvaders https://lucky-sideburn.github.io/helm-charts/
helm repo update


 kubectl create namespace kubeinvaders
 namespace/kubeinvaders created
 kubectl create namespace namespace1  
 namespace/namespace1 created
 kubectl create namespace namespace2
 namespace/namespace2 created

 helm install kubeinvaders kubeinvaders/kubeinvaders --set-string config.target_namespace="namespace1\,namespace2" --set route_host="localhost" -n kubeinvaders

kubectl port-forward svc/kubeinvaders 8080:80 -n kubeinvaders

