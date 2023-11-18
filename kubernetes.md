minikube start --driver=docker

minikube delete

kubectl get nodes

kubectl create deployment <deploymentName> --image=<imageName>

kubectl get deployment
kubectl get replicaset
kubectl get pod
kubectl logs <podName>
kubectl describe pod <podName>
kubectl exec -it <podName> -- bin/bash
kubectl delete deployment <deploymentName>

kubectl apply -f config-file.yaml
kubectl delete -f config-file.yaml

kubectl describe service <serviceName>

# To get more info about the pods
kubectl get pod -o wide

# To get the current state of your deployment
kubectl get deployment <deploymentName> -o yaml

# To get the current state of your deployment and save in a yaml file
kubectl get deployment <deploymentName> -o yaml > <fileName>.yaml

echo -n '<UsernameORPasswordhere>' | base64

# Config file has 3 parts => metadata, specification and status (desired, actual states)
# You can create config (YAML) files for deployment (that has pods), Services (to talk to the pod, Secrets)
# Order of "apply" matters. First apply secrets and configmap configuration, and then deployment, and then service (because they refer one another in a defined order)
# For internal service -> no need to define the type. For external service -> type: LoadBalancer
- This type defines it as external service (internal service also acts as load balancer, but this keyword is chosen for external service) and assigns it a external IP. Also open the node port

# First you need to access the service, and the Service access it's assigned deployment (Pod)

# NamesSpaces:c(kind of virtual clusters for grouping inside K8 cluster)
- Organizing / Grouping of resources (components) in K8 cluster
- If no namespace is created / defined, the resources are created in "default" namespace
- They are used to limit the access and cluster resource quotas such as RAM, CPU, Storage etc ...
- Few components  such as Volumne (persistent) and Node can't be in a namespace
    # cmd: 
    kubectl api-resources --namespaced=false
- You can't use most resources in a namespace from another namespace
    Example: Each namespace should have its own configMap. It can't refer to another namespace's configMap. However, both configMaps can refer to the same service (database service)
- You can access a service running in another namespace in a different namespace.
    Example: In yaml file, data - dburl: mysql_db.<nameSpace>

# Commands
kubectl create namespace <nameSpacename>
- to list all the namespaces
kubectl get namespace


Defining namespaces can be in two ways. 1. In command while performing apply for congif file. 2. Or define it under metadata in config file
- kubectl apply -f myconfigfilrname.yaml --namespace=<mynamespace>

- To avoid using --namespace everytime in the command, install a plug-in called kubens or kubectx to switch between the namespaces, as Kubernetes doesn't support it on its own.

# Ingress
- routing rules defined for how your host should direct the traffic to. Please refer to the dashboard-ingress.yaml
- Implementation is done by the Ingress Controller in another pod
- You may need proxy server as an entry point that can redirect the traffic to Ingress Controller
    Proxy Server (entry point) => Ingress Pod (Ingress COntroller) => Cluster IP Service (Internal Service) => Pod (application)

# Persistent Volume (PV)
- Can not be inside the namespace like other components such as deployments and services.
- Created by admins on cluster level
- DevOps Engineers / users can make use of persistent volume with something called Persistent Volume Claim (PVC) defined inside the deployment (file)
- To avoid manual creation of PV's, use Storage Class that can create the PV's dynamically.

# Kubernetes Services -
- Each pod has its own IP, but it is ephemeral, that means destroyed or changed frequently. With Service, a stable IP can be created that can access the pod.
1. Cluster IP Service (Internal and default) - Can be accessed through Ingress
2. Multi-Port Service - If one pod has multiple containers, define 2 ports and target ports
3. NodePort Service - NodePort is added in the range of 30000 - 32767. Entire Node can be accessed externally. Not recommended. Type: NodePort
4. Loadbalancer Service - Extension of NodePort Service. NodePort parameter is defined. However, traffic is can not directly access the node. Instead, it has to go through the external load balancer created on the cloud provider. Type: Loadbalancer
5. Headless Service - for communicating with specific pod. Commonly used for stateful applications, where specific master pod in the cluster is accessed for write purpose, where as other replicas can only read. When DNS LookUp is done, cluster IP is returned. For pod IP to be returned, define clusterIP: None in the service.

# Other topics to be referred:
- Helm and helm charts
- StatefulSet - for containering stateful applications such as mongodb, mysql, where preserving state is important. Generally, K8's is recommended for stateless applications, as it is hard to implement stateful applications.
- Pod Identity (related to statefulset), sticky identity
- Pod Endpoints (related to statefulset)

