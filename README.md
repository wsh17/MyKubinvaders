## MyKubinvaders - Steps to Deploy on kubernetes, in our case were using docker-desktop locally and the kubernetes option

### Steps to deploy Kubeinvaders from ```https://github.com/lucky-sideburn/kubeinvaders/```. 

#### First lets get the helm charts and set the repo and also update them to the most current

1. `helm repo add kubeinvaders https://lucky-sideburn.github.io/helm-charts/`
  
2. `helm repo update`

#### Next lets create the rbac permissions and kubeinvaders namespace using kubeinvaders-rbac.yaml, You will need to grap the file from this repo or from the origional repo, this file was not altered from the main repo. 

3. `kubectl apply -f kubeinvaders-rbac.yaml`

#### Next we will re-create the secret in the kubeinvaders namespace for 8 hours, change the value to 8760h for 1 year

4. `kubectl create token kinv-sa -n kubeinvaders --duration=8h`

#### Next we will create namespace1 and namespace2 for the nginx pods that are the aliens to shoot down.  You will need to grap the files from this repo to create the nginx pods, also you can increase the number of pods by changing the replicas to a larger number, today they are set at 10 replicas. 

5. `kubectl create namespace namespace1` <br>

 `namespace/namespace1 created`

7. `kubectl create namespace namespace2` <br>

 `namespace/namespace2 created`
 
8. `kubectl apply -f kubeinvaders-ns1.yaml`
   
9. `kubectl apply -f kubeinvaders-ns2.yaml`

#### Next we will use helm to deploy the kubeinvaders application into the kubinvaders namespace.  Notice were hardcoding http in the command otherwise most browsers will attempt to use https on port 443.

10. `helm install kubeinvaders kubeinvaders/kubeinvaders --set-string config.target_namespace="namespace1\,namespace2" --set route_host="http://localhost:8080" -n kubeinvaders`

#### Finially we will portforward the application to our our laptop on localhost on port 8080

11. `kubectl port-forward svc/kubeinvaders 8080:80 -n kubeinvaders`

#### Last step is to hit http://localhost:8080 with your browser to see and play with kubinvaders, enjoy ... 
