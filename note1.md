# Note 1 Basics of kubectl

## 1. Basic Queries

- Get pods information
	- name
	- ready
	- status
	- restarts
	- age

```
$ kubectl get pods
```

- Get detailed pods information
	- name 
	- reday
	- status
	- restarts
	- ages 
	- IP address
	- node
	- nominated node
	- readness gates 

```
$ kubectl get pods -o wide
```

- Get nodes information
	- name
	- status 
	- roles 
	- age
	- version

```
$ kubectl get nodes
```

- Get nodes detailed information
	- name
	- status 
	- roles 
	- age
	- version
	- internal IP
	- external IP
	- OS image
	- kernel version
	- container runtime

```
$ kubectl get nodes -o wide
```

- Create pod with `nginx` named `nginx`

```
$ kubectl run nginx --image=nginx
```

- Delete pod called `nginx`

```
$ kubectl delete pods nginx
```

- Count pods 

```
$ kubectl get pods | tail -n +2 | wc -l
```

- Check the image being used to create the pod `nginx`

```
$ kubectl describe pods nginx | grep -i image
```

- Check the node where pod `nginx` is placed

```
$ kubectl describe pods nginx | grep -i node
```

- Check number of containers in the pod `webapp`
	- See the READY column
		- the first digit means the number of ready containers
		- the second digit means the total number of containers

```
$ kubectl get pods webapp 
```

- Check the status of `agentx` container in the pod `webapp`
	- Find `State` under `Containers` -> `agentx`

```
$ kubectl describe pods webapp 
``` 

- Change the image `redis123` to `redis` on the pod `redis`
	- use vim to change the image to `redis`

```
$ kubectl edit pods redis
```


## 2. ReplicaSets

- Get the information of Replica Sets
	- name
	- desired pods
	- current pods
	- ready
	- age

```
$ kubectl get replicasets
```

- Get detailed information of Replica Sets
	- name
	- desired pods
	- current pods
	- ready
	- age
	- containers
	- image
	- selector

```
$ kubectl get replicasets -o wide
```

- Delete a pod in the replicaset `new-replica-set`
	- when a pod is deleted in a replicaset, another pod will be created immediately to ensure the desired number of pods

```
$ kubectl get pods
...
new-replica-set-sd9jt
...
$ kubectl delete pods new-replica-set-sd9jt
```

- Check the YAML explaination of a replicaset

```
$ kubectl explain replicaset
```

- Create a replicaset from the conf file `replicaset-definition-1.yaml`

```
$ kubectl create -f replicaset-definition-1.yaml 
```

- Delete replicaset `replicaset-1`

```
$ kubectl delete replicasets replicaset-1
```

- Rescale the replicaset `new-replica-set` with 4 pods

```
$ kubectl scale replicasets new-replica-set --replicas=4
```

- Change the image to `busybox` for replicaset `new-replica-set`
	 - Edit the YAML file for `new-replica-set`
	 - Delete the current pods under `new-replica-set` to apply this change

```
$ kubectl edit replicasets new-replica-set
$ kubectl scale replicasets new-replica-set --replicas=0
$ kubectl scale replicasets new-replica-set --replicas=4
```

## 3. Deployments

- Get the information of deployments
	- name
	- ready
	- up-to-date
	- available
	- age

```
$ kubectl get deployments
```

- Get detailed information of deployments
	- name
	- ready
	- up-to-date
	- available
	- age
	- containers
	- images
	- selector

```
$ kubectl get deployments -o wide
```

- Get the explaination of deployments in kubernetes

```
$ kubectl explain deployment
```

- Create deployment from the config file `deployment-definition-1.yaml`

```
$ kubectl create -f deployment-definition-1.yaml 
```

## 4. Namespaces

- Get the number of namespaces

```
$ kubectl get ns | tail -n +2 | wc -l
```

- Get the pods information in the `research` namespace

```
$ kubectl get pods -n research 
```

- Create a pod named `redis` using the image `redis` under the `finance` namespace

```
$ kubectl run redis -n finance --image=redis
```

- Find which namespace has a pod named `blue`

```
$ kubectl get pods --all-namespaces | grep blue
```

## 5. Wrap up

- Create kubenetes service named `redis-service` on port `6379` with type of ClusterIP

```
$ kubectl create svc clusterip redis-service --tcp=6379
```

- Create deployment named `webapp` using image `kodekloud/webapp-color` with `3` replicas

```
$ kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3
```

- Create new pod named `custom-nginx` using image `nginx` on port `8080`

```
$ kubectl run custom-nginx --image=nginx --port=8080
```

- Create new namespace called `dev-ns`

```
$ kubectl create ns dev-ns
```

- Create new deployemnt called `redis-deploy` in the `dev-ns` namespace with the `redis` image with `2` replicas

```
$ kubectl create deployment redis-deploy -n dev-ns --image=redis --replicas=2
```

- Create a pod named `httpd` using image `httpd:alpine` and expose it to a service named `httpd` on port `80`

```
$ kubectl run httpd --image=httpd:alpine --port=80 --expose
```

