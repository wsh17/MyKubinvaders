## MyKubinvaders - Steps to Deploy kubeinvaders on kubernetes, in our case were using docker-desktop locally and the kubernetes option within docker-desktop

### Steps to deploy Kubeinvaders from `https://github.com/lucky-sideburn/kubeinvaders/` repo with a few tweaks by me to get it working properly.

#### First lets get the helm charts and set the repo and also update them to the most current version

1. ```bash
   helm repo add kubeinvaders https://lucky-sideburn.github.io/helm-charts/
  
3. ```bash
   helm repo update

#### Next lets create the rbac permissions and kubeinvaders namespace using kubeinvaders-rbac.yaml, You will need to grap the file from this repo or from the origional repo, this file was not altered from the main repo. 

3. ```bash
   kubectl apply -f kubeinvaders-rbac.yaml

#### Next we will re-create the secret in the kubeinvaders namespace for for 1 year, using 8760h which is 1 year thanks to Dr Drew Osborn for this. 

4. ```bash
   kubectl create token kinv-sa -n kubeinvaders --duration=8760h

#### Next we will create namespace1 and namespace2 and launch 20 pods in each namespace. The nginx pods that are the aliens we will shoot down once the application is running.  You will need to grab the files from my repo to create the namespaces (namespace1 and namespace2) and nginx pods, Note you might have to pull nginx manually with docker if you get an image pull error (with kubectl) which happens on a Mac due to security, the work around is to just pull the image to docker first using this command `docker pull nginx:latest` then kubectl will get it locally from docker images.  The 2 yaml file below will both create the namespaces and pods directly. This is an update from my origional instructions, trying to shorten the steps. 

5. ```bash
   kubectl apply -f kubeinvaders-ns1.yaml
   
7. ```bash
   kubectl apply -f kubeinvaders-ns2.yaml

#### Next we will use helm to deploy the kubeinvaders application into the kubinvaders namespace.  Notice were hardcoding http in the command otherwise most browsers will attempt to use https on port 443.

7. ```bash
   helm install kubeinvaders kubeinvaders/kubeinvaders --set-string config.target_namespace="namespace1\,namespace2" --set route_host="http://localhost:8080" -n kubeinvaders

#### Finially we will portforward the application to our our laptop on localhost on port 8080

8. ```bash
   kubectl port-forward svc/kubeinvaders 8080:80 -n kubeinvaders

#### Last step is to hit http://localhost:8080 with your browser to see and play with kubinvaders, enjoy ... 
