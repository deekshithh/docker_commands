Kubernetes is a container orchestration platform that manages and automates the deployment, scaling, and management of containerized applications. Here are some basic terms related to Kubernetes:

1. **Pod**: One or more containers running together on one node. A pod is the smallest unit in Kubernetes that can be deployed and managed. A pod can contain one or more containers that share the same network namespace and can communicate with each other using localhost.
2. **Container**: A container is a lightweight, standalone, and executable package of software that includes everything needed to run a piece of software. Containers are isolated from each other and from the host system, and they share the kernel of the host system.
3. **Node**: A node is a worker machine in Kubernetes that runs pods. A node can be a physical or virtual machine that has the necessary software to run containerized tasks.
4. **Cluster**: A cluster is a group of nodes that run containerized applications. A cluster has a master node that controls the state of the cluster and a set of worker nodes that run the applications.
5. **Namespace**: A namespace is a logical partition of a cluster that provides a scope for names. Namespaces are used to isolate different parts of an application or different teams working on the same cluster.
6. **Service**: Network endpoint to connect to a pod. A service is a logical abstraction of a set of pods that perform a specific function. A service provides a stable IP address and DNS name that can be used to access the pods.
7. **Volume**: A volume is a directory containing data, accessible to the containers in a pod. Volumes are used to share data between containers in a pod or to persist data across container restarts.
8. **Controller**: A controller is a control loop that watches the shared state of the cluster through the API server and makes changes attempting to move the current state towards the desired state.
9. **Deployment**: A deployment is a declarative way to specify how a set of pods should be created and updated. A deployment controller manages the rollout and scaling of replica sets.
10. **Secret**: A secret is a way to store sensitive information, such as passwords, tokens, or keys, in a secure and encrypted manner. Secrets can be used to provide access to external services or to authenticate with other components in the cluster.
11. **kube**: Kubernetes is called as kube in short.
12: **kubectl**: Cube control, CLI to configure kubernetes and manage apps.
13: **kubelets** Kubernetes agents running on nodes.
14: **Control Plane** Set of containers that manage the cluster.

These are just a few of the basic terms used in Kubernetes. Understanding these terms is essential to working with Kubernetes and managing containerized applications.

Citations:
[1] https://www.appvia.io/blog/understanding-common-kubernetes-terms
[2] https://kubernetes.io/docs/reference/glossary/
[3] https://kodekloud.com/blog/kubernetes-terms/
[4] https://sematext.com/glossary/kubernetes/
[5] https://www.techtarget.com/searchdatacenter/feature/Common-Kubernetes-terminology-you-should-know
