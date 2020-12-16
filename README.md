docker day 6
------------

Topics

* kubernetes components
* pods
* docker run to pod
* services
* expose the pod
* controllers
* replicate the pod

kubernetes components
---------------------

A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

Control Plane Components
* kube-apiserver
* etcd
* kube-scheduler
* kube-controller-manager
* cloud-controller-manager

Node Components
* kubelet
* kube-proxy
* [container runtime via CRI](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md)


Links
* [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)
* [Component Overview](https://medium.com/@karthikeyan_krishnaswamy/overview-of-kubernetes-34d8e0e59b26)

pods
----

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage/network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.

Links
* [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
* [Managing Resources](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)


docker run to pod
-----------------

Convert a docker run command into a kubernetes pod

* Checkout this repo

* Build the docker image:

    docker build -t hello-api:v0.1.0 .

* Fill out a pod from from the `pod.yml` the template in the repo and compare it to a `docker run` command

* kubectl apply -f pod.yml

* kubectl get pods (if you see your pod running then you were successful)

* kubectl logs <pod name>

* kubectl exec -it <pod name> /bin/ash

services
--------

A service can be defined as a logical set of pods. It provides a single IP address and DNS name by which pods can be accessed. This ip and dns entry load balance accross all of the pods.

Link
* [Services](https://kubernetes.io/docs/concepts/services-networking/service/)


expose the pod
---------------

Expose the previous pod to the outside world using services

* Using knowledge of services from the docs fill in the `svc.yml` file. Use type loadbalancer to map to a local port.
Project. Make sure to pick the correct service type to expose to the outside.
* kubectl apply -f svc.yml
* kubectl get svc
* Navigate to http://localhost:8090/ to see your service running

controllers
-----------

In Kubernetes, controllers are control loops that watch the state of your cluster, then make or request changes where needed. Each controller tries to move the current cluster state closer to the desired state.

Links
* [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/)
* [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/)
* [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
* [ReplicatonController vs Deployment](https://ryaneschinger.com/blog/rolling-updates-kubernetes-replication-controllers-vs-deployments/)

replicate the pod
-----------------

Create a deployment based on `pod.yml`

* Create a deployment based on `pod.yml` and the `deployment.yml` template
* Pick a replica count > 1
* kubectl apply -f deployment.yml
* kubectl get deployment
* kubectl get pods
* Curl http://localhost:8090/ to see your service responding with different pod names
* Explain why the orignal pod and deployment pods are both returning results
