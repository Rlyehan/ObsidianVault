222022-10-27

Style: 

Domain:

Tags:

# Understanding Kubernetes Architecture with Diagrams

### Kubernetes Architecture and Components

Kubernetes has a decentralized architecture that does not handle tasks sequentially. It functions based on a declarative model and implements the concept of a ‘_desired state_.’ These steps illustrate the basic Kubernetes process:

1.  An administrator creates and places the desired state of an application into a manifest file.
2.  The file is provided to the Kubernetes API Server using a CLI or UI. Kubernetes’ default command-line tool is called **_kubectl_**. In case you need a comprehensive list of kubectl commands, check out our [Kubectl Cheat Sheet](https://phoenixnap.com/kb/kubectl-commands-cheat-sheet).
3.  Kubernetes stores the file (an application’s desired state) in a database called the **Key-Value Store (etcd)**.
4.  Kubernetes then implements the desired state on all the relevant applications within the cluster.
5.  [Kubernetes continuously monitors the elements of the cluster](https://phoenixnap.com/kb/prometheus-kubernetes-monitoring) to make sure the current state of the application does not vary from the desired state.

![[Pasted image 20221027142907.png]]

### What is Master Node in Kubernetes Architecture?

The Kubernetes Master (Master Node) receives input from a CLI (Command-Line Interface) or UI (User Interface) via an API. These are the commands you provide to Kubernetes.

You define pods, replica sets, and services that you want Kubernetes to maintain. For example, which container image to use, which ports to expose, and how many pod replicas to run.

You also provide the parameters of the desired state for the application(s) running in that cluster.

![[Pasted image 20221027142919.png]]
#### API Server

The **API Server** is the front-end of the control plane and the only component in the control plane that we interact with directly. Internal system components, as well as external user components, all communicate via the same API.

#### Key-Value Store (etcd)

The Key-Value Store, also called **etcd**, is a database Kubernetes uses to back-up all cluster data. It stores the entire configuration and state of the cluster. The Master node queries **etcd** to retrieve parameters for the state of the nodes, pods, and containers.

#### Controller

The role of the **Controller** is to obtain the desired state from the API Server. It checks the current state of the nodes it is tasked to control, and determines if there are any differences, and resolves them, if any.

#### Scheduler

A **Scheduler** watches for new requests coming from the API Server and assigns them to healthy nodes. It ranks the quality of the nodes and deploys pods to the best-suited node. If there are no suitable nodes, the pods are put in a pending state until such a node appears.

### What is Worker Node in Kubernetes Architecture?

Worker nodes listen to the API Server for new work assignments; they execute the work assignments and then report the results back to the Kubernetes Master node.

![[Pasted image 20221027143143.png]]

#### Kubelet

The **kubelet** runs on every node in the cluster. It is the principal Kubernetes agent. By installing kubelet, the node’s CPU, RAM, and storage become part of the broader cluster. It watches for tasks sent from the API Server, executes the task, and reports back to the Master. It also monitors pods and reports back to the control panel if a pod is not fully functional. Based on that information, the Master can then decide how to allocate tasks and resources to reach the desired state.

#### Container Runtime

The **container runtime** pulls images from a **container image registry** and starts and stops containers. A 3rd party software or plugin, such as Docker, usually performs this function.

#### Kube-proxy

The **kube-proxy** makes sure that each node gets its IP address, implements local _iptables_ and rules to handle routing and [traffic load-balancing](https://phoenixnap.com/kb/load-balancing).

#### Pod

A **pod** is the smallest element of scheduling in Kubernetes. Without it, a container cannot be part of a cluster. If you need to scale your app, you can only do so by adding or removing pods.

The pod serves as a ‘wrapper’ for a single container with the application code. Based on the availability of resources, the Master schedules the pod on a specific node and coordinates with the container runtime to launch the container.

In instances where pods unexpectedly fail to perform their tasks, Kubernetes does not attempt to fix them. Instead, it creates and starts a new pod in its place. This new pod is a replica, except for the DNS and IP address. This feature has had a profound impact on how developers design applications.

Due to the flexible nature of Kubernetes architecture, applications no longer need to be tied to a particular instance of a pod. Instead, applications need to be designed so that an entirely new pod, created anywhere within the cluster, can seamlessly take its place. To assist with this process, Kubernetes uses **services**.

#### Container Deployment

Container Deployment is the next step in the drive to create a more flexible and efficient model. Much like VMs, containers have individual memory, system files, and processing space. However, strict isolation is no longer a limiting factor.

Multiple applications can now share the same underlying operating system. This feature makes containers much more efficient than full-blown VMs. They are portable across clouds, different devices, and almost any OS distribution.

![[Pasted image 20221027143240.png]]
![Diagram of Container Deployment.](https://phoenixnap.com/kb/wp-content/uploads/2021/04/container-deploment-kubernetes.png)

_Container Deployment_

The container structure also allows for applications to run as smaller, independent parts. These parts can then be deployed and managed dynamically on multiple machines. The elaborate structure and the segmentation of tasks are too complex to manage manually.

An automation solution, such as Kubernetes, is required to effectively manage all the moving parts involved in this process.


___
# References
[Understanding Kubernetes Architecture with Diagrams](https://phoenixnap.com/kb/understanding-kubernetes-architecture-diagrams)