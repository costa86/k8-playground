# start cluster
    minikube start
# cluster status
    minikube status
# delete cluster
    minikube delete
# summary
    kubectl get all    
# other
    kubectl config current-context

    kubectl config get-contexts

    kubectl get namespaces

    kubectl get pods -n <ns_name>

# describe is very helpful. Always remember to mention namespace (-n), otherwise it fallsback to the default one
    kubectl describe pod <some_pod> -n <some_ns> 

# run app
    minikube id
    kubectl get service

http://<kube_id>:<service_external_port>

# get services (external url's)
    minikube service list

# get url from service
    minikube service <service_name> --url    

# volumes
    kubectl apply -f <volume_file>
    kubectl get persistentvolume

# plugins for minikube
    minikube addons list
    minikube enable <name>

# go to dashboard
    minikube dashboard

# namespace summary

    kubectl describe ns <ns_name>

# create pod from docker image
    kubectl run <pod_name> --image=<image_name>

# see pods w/ more options
     kubectl get pods -o wide

# create faster deployment
    kubectl create deployment <name> --image=<image_name>

# scale/update replicas

    kubectl scale deployment <depoyment_name> --replicas=<num_replicas>

# expose port
    kubectl expose deployment <depl_name> --port=8080 --target-port=80

# create deploy custom image published to dockerhub    
    kubectl create deploy hello-js --image=costa86/sample-js

## expose port (container)
    kubectl expose deployment hello-js --port=3000

## expose port (outside)
    kubectl expose deployment hello-js --type=NodePort --port=3000

# open project in browser
    minikube service <svc_name>

## expose port as load balancer
    kubectl expose deployment hello-js --type=LoadBalancer --port=3000

# update container version
container_name is supposed to be pod name, but it did not work in my case

    kubectl set image deployment <deploy_name> <container_name>=costa86/sample-js:2.0.0

after that, run this to monitor update status:

    kubectl rollout status deployment hello-js

# delete everything
    kubectl delete all --all

# delete pod (self healing)
    kubectl delete pod <pod_name> 

# connecting deployments
to prove that 'nginx' is proxied between deployments, this command will output a Address.
This address is the same seen when you list the services.s

    kubectl exec hello-js-nginx-7f5c6f987c-gbb6z -- nslookup nginx

or this:

    kubectl exec hello-js-nginx-7f5c6f987c-gbb6z -- wget -qO- http://nginx

# access pods/container
    kubectl exec -it hello-js-nginx-7f5c6f987c-gbb6z -- bin/bash

# get more info about deployment (include 'status', which is updated constantly)
    kubectl get deployments hello-js-nginx -o yaml

# for secrets values
secret must be created before the deployment!!
    echo -n 'username' | base64

# list all types of resources
here, the ones that accept namespace

    kubectl api-resources --namespaced=true

# kubectx - tool to manage namespaces
https://github.com/ahmetb/kubectx#apt-debian