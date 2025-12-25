Kubernetes is container orcastration system. It takes care of

- auto deployment of containerized apps across different servers.
    
- Load distribution of load across these servers
    
- auto scaling
    
- Monitoring , health check
    
- auto healing
    

**Pods**

Pod is the smallest unit in k8s world. Basic anatomy of a pod is as follows

![[Pasted image 20251225172302.png]]

One pod can contain one or many docker containers. **Typically one pod contains one container,** but there is always possibility of adding more than one containers to a pod. All containers in a pod share storage volumes and netork IP address. One pod must have containers hosted on one server only. Y**ou cannot add containers from different servers to a single pod.**

**k8s Clusters**

![[Pasted image 20251225172316.png]]

K8s cluster contains nodes, which are basically servers. These servers can be located in different servers, data centers and geographic locations. Inside nodes , there are pods, inside pods, there are containers.

![[Pasted image 20251225172331.png]]

once a cluster is formed, each cluster has master nodes and worker nodes. Master nodes typically do not run your work load, they just manage the cluster. Work load is run by workers.

Service details of the clusters

![[Pasted image 20251225172346.png]]

All the nodes, i.e. master ans worker run ffollowing services

container runtime: docker runtime for containers

kubelet : The kubelet is the primary "node agent" that runs on each node. It can register the node with the apiserver. Kubelet is responsible for communicating with the API server.

kube-proxy : this is basically network component of the node. This reflects services as defined in the Kubernetes API on each node and can do simple TCP, UDP, and SCTP stream forwarding or round robin TCP, UDP, and SCTP forwarding across a set of backends

Following services run only on master node

API server :This is central point of communication between the nodes.he API Server services REST operations and provides the frontend to the cluster's shared state through which all other components interact.

scheduler : planning and distribution of loads across the nodes in the cluster.

kube-controller-manager : In Kubernetes, a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state.

cloud controller manager : interface with the cloud provider.

ETCD: this manintains key values logs of the cluster. This acts as a brain of the cluster storing all the imp states related to cluster.

kubectl : API iteractive tool which talks to api server. Kubectl allows users to manage the cluster using apiserver apis.

**Command to run minikube :**

minikube start -driver=docker

minikube start -driver=virtualbox

once minikube is up and running, you can check the status by

minikube status

Then print the ip of the single virtual node minikube has created using

minikube ip

Then you can ssh into this docker node using

`ssh -i ~/.minikube/machines/minikube/id_rsa docker@$(minikube ip)`

in the virtual node, there is a docker runtime running, which you can check by command

docker ps

now exit ssh, and try kubectl. here is few command to list namespaces and check k8s system namespace

![[Pasted image 20251225172407.png]]





For every pod created, k8s creates a pause container. This pause container is automatically created by k8s as a placeholder. Reference [here](https://sas-dev-notes.atlassian.net/wiki/pages/resumedraft.action?draftId=54460433&draftShareId=a80f8d90-ad86-4848-a0e8-7c11b227808a "https://sas-dev-notes.atlassian.net/wiki/pages/resumedraft.action?draftId=54460433&draftShareId=a80f8d90-ad86-4848-a0e8-7c11b227808a"). Pause container is not visible to kubectl. It can be accessed by docker ps or sudo ctr containers list. Containers in pod can close/recreate by pause container remains, as a placeholder for the pod.

![[Pasted image 20251225172433.png]]

You can connect to nginx application running on the docker container from minicube virtual host or from the ngnix docker container itself. You cannot connect to this nginxx server from your local, because your local is not part of the minikude cluster and all the ips used are internal ones.

**deployments**

These are declarative ways to describe a set of pods or replicasets. You describe a _desired state_ in a Deployment, and the Deployment [Controller](https://kubernetes.io/docs/concepts/architecture/controller/ "https://kubernetes.io/docs/concepts/architecture/controller/") changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

Commads to create and list deployements
```
kubectl create deployment ngin-deploy --image=nginx
kubectl get deployments or kubectl get deploy
kubectl describe deployment ngin-deploy
kubectl scale deployment ngin-deploy --replicas=3 //change number of pods
```

Deployemtns and pods are two separate objects. You need a way to idetify which pod belongs to which deployment. For that, selectors come in picture. You can see sepectors of a pod or deployment using describe command. e.g. a deployment with selector “app=ngin-deploy“ will map to pods having the same selector value.

**services**

once deployment is created, it auto creates the number of pods specified in replicas field of deployment. You can log into each pod/container use docker exec(`docker exec -it <mycontainer> bash`). You can access te application running in that container from within cintainer or from the kbs cluster(in this case minikube virtual machine). There are two problems here

1. You cannot access that application from outside the k8s cluster
    
2. Ips assigned to pods are ever changing. How can other deployments/pods from the same cluster access this deployment as awole without knowing the individual ip addressed of the pods.
    

For this purppose kubernetes services are used. Services are of two types.

1. cluster service
    

This is local to the cluster. Other deployments from the same cluster can communicate to all the pods of this deployment using the service name. Load will be automatically distributed across all the pods by k8s in this case.


```
kubectl expose deployment ngin-deploy --port=8080 --target-port=80
//commnd to expose internal service at port 8080. Container is accepting traffic at 80, 
//so we have to specify that as target port.

kubectl get service //list services 
kubectl get svs //list services

```

![[Pasted image 20251225172526.png]]

2. **node service**
    

When you want your service to be accessible from outside of your cluster, you use node service. You can do thi with help of type NodePort.
```
kubectl expose deployment k8s-web-hello --type=NodePort --port=3000
```

Service eposed will be of type NodePort.

![[Pasted image 20251225172609.png]]

This nodePort will be accessible from your local as well. This basically binds application running on 3000 with MiniKube virtual host’s random port, 32346 in this case.

Use following miniKube commmands to access this services from your local

![[Pasted image 20251225172624.png]]

**Load balancers:**

![[Pasted image 20251225172643.png]]

delete all resorces at once
```
kubectl delete all --all //delete everythin from default namespace
kubectl delete all --all -n {namespace}
```

**rolling updates**

k8s incrementally deploys new version or previous version, without making the app down.

commands for that
```
kubectl set image deployment <deployment name> <deployment name>=image:v2
//e.g. kubectl set image deployment kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10

to roll back simply use previous version
kubectl set image deployment kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v9

//rollout status
kubectl rollout status deployment kubernetes-bootcamp

//undo to revios version
kubectl rollout undo deployment kubernetes-bootcamp
```





