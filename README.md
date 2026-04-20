# MyKubinvaders

### Steps to deploy Kubeinvaders

#### First lets get the helm charts and update them

1. helm repo add kubeinvaders https://lucky-sideburn.github.io/helm-charts/
2. helm repo update

#### Next lets create the rbac permissions and kubeinvaders namespace using kubeinvaders-rbac.yaml

3. kubectl apply -f kubeinvaders-rbak.yaml

#### Next we will re-create the secret in the kubeinvaders namespace for 8 hours, change the value to 8760h for 1 year

4. kubectl create token kinv-sa -n kubeinvaders --duration=8h

#### Next we will create namespace1 and namespace2 for the nginx pods that are the aliens to shoot down

5. kubectl create namespace namespace2  
 `namespace/namespace1 created`
5. kubectl create namespace namespace2
 `namespace/namespace2 created`

helm install kubeinvaders kubeinvaders/kubeinvaders --set-string config.target_namespace="namespace1\,namespace2" --set route_host="localhost" -n kubeinvaders
helm install kubeinvaders kubeinvaders/kubeinvaders --set-string config.target_namespace="namespace1\,namespace2" --set route_host="http://localhost:8080" -n kubeinvaders


kubectl port-forward svc/kubeinvaders 8080:80 -n kubeinvaders

