# AI Prompt - yes this whole thing was my initial prompt

This task is part of a project to get Kubeinvaders to work.  Your still a kubernetes application expert and here is the repo were working from, its on purpose setup with not a step by step flow, its kind of a troubleshooting exercise.  Kubeinvaders is modeled after a game from the 1970's called space invaders, its was a great game.  Basically shooting thing that are marching towards with a space gun that you can control at the bottom with the arrow keys and space bar to shot down the invaders .. before they get to you and crush you.   

Here is the repo

https://github.com/lucky-sideburn/kubeinvaders/tree/master

What we have done so far is get part of the application running by using a helm chart that is here in the same repo

https://github.com/lucky-sideburn/kubeinvaders/tree/master/helm-charts/kubeinvaders

We use the following commands, which allows to see the shooter at the bottom of the game ... to get deployed into a POD in Kubernetes called kuberinvaders, the 2 other namespaces are where the invaders will run, namespace1 and namespace2. We will then port-forward to our local laptop so we can get to an endpoint, localhost:8080 to see the UI for the game. 

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

now that we have the UI running and the shooter gun at the bottom shooting, we need to figure out the other steps to get the invaders to fly around and allows us to shoot them.  They are nginx pods, but how do we deploy then and have them go back and fourth like the origional space invaders did.  Please take a look and give us some advice how how to do this, thanks. 

#### Note
As it turns out, see the README.md for step by step instructions.  Above AI was wrong, for `--set route_host="localhost"` needed to be `--set route_host="http://localhost:8080`


