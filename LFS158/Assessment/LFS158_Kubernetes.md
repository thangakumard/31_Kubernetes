# Kubernetes Q&A Study Guide

A consolidated reference of Kubernetes questions, answer options, correct answers, and detailed explanations for every option.

---

## 1. While exposing a Service using LoadBalancer, ________________.

**Options:**
- a. A ClusterIP is assigned to the service
- b. The service is exposed at a static port on all the Worker Nodes
- c. A load balancer is configured automatically using the underlying cloud infrastructure
- d. The Port is opened only on the nodes where Pods are running for the respective service

**Correct answers: a, b, c**

- **a. A ClusterIP is assigned to the service — True.** `LoadBalancer` is built on top of `ClusterIP` and `NodePort`. Kubernetes still assigns an internal ClusterIP so the service is reachable within the cluster in addition to being exposed externally.
- **b. The service is exposed at a static port on all the Worker Nodes — True.** `LoadBalancer` builds on the `NodePort` mechanism internally, so a static port is opened on every Worker Node.
- **c. A load balancer is configured automatically using the underlying cloud infrastructure — True.** This is the defining feature of `LoadBalancer` — Kubernetes calls the cloud provider's API to provision an actual external load balancer.
- **d. The Port is opened only on the nodes where Pods are running for the respective service — False.** The NodePort is opened on **all** Worker Nodes, regardless of whether a Pod is scheduled there. `kube-proxy` routes traffic to nodes with matching Pods.

---

## 2. Once an object is created, which field displays the object's current state?

**Options:**
- a. config
- b. current
- c. spec
- d. status

**Correct answer: d. status**

- **a. config — False.** Not a standard Kubernetes object field.
- **b. current — False.** Not a standard Kubernetes object field.
- **c. spec — False.** `spec` represents the **desired state** provided by the user, not the current state.
- **d. status — True.** `status` is supplied and updated by Kubernetes controllers to reflect the **actual/current state** of the object, which is continuously reconciled against `spec`.

---

## 3. Which of the following projects are managed under the CNCF umbrella?

**Options:**
- a. Prometheus
- b. Linkerd
- c. OpenTelemetry
- d. linux-container

**Correct answers: a, b, c**

- **a. Prometheus — True.** A CNCF graduated project (monitoring/alerting toolkit); the second project to join CNCF after Kubernetes.
- **b. Linkerd — True.** A CNCF graduated project; the first service mesh donated to CNCF.
- **c. OpenTelemetry — True.** A CNCF graduated project providing unified observability APIs/SDKs/tools.
- **d. linux-container — False.** Not a recognized CNCF project; likely confused with LXC (not CNCF) or containerd (CNCF graduated, but named differently).

---

## 4. Which of the following are components of the Worker node?

**Options:**
- a. container runtime
- b. kube-proxy
- c. kubelet
- d. serverlet

**Correct answers: a, b, c**

- **a. container runtime — True.** Needed to pull images and run containers (e.g., containerd, CRI-O).
- **b. kube-proxy — True.** Maintains network rules on every Worker Node for Service networking.
- **c. kubelet — True.** The primary node agent; ensures containers described in PodSpecs are running and reports status.
- **d. serverlet — False.** Not a real Kubernetes component; a made-up distractor.

---

## 5. Which networking model is used by Kubernetes?

**Options:**
- a. Container Network Interface (CNI)
- b. Container Network Model (CNM)
- c. Both CNI and CNM
- d. None of the above

**Correct answer: a. Container Network Interface (CNI)**

- **a. CNI — True.** Kubernetes uses CNI as its networking specification, allowing plugins like Calico, Flannel, Cilium, and Weave Net to provision networking.
- **b. Container Network Model (CNM) — False.** CNM is Docker's networking model, not used by Kubernetes.
- **c. Both CNI and CNM — False.** Kubernetes exclusively uses CNI.
- **d. None of the above — False.** CNI is indeed used.

---

## 6. On what kind of environment can Kubernetes be installed?

**Options:**
- a. VMs
- b. Bare Metal
- c. Cloud infrastructure
- d. All of the above

**Correct answer: d. All of the above**

- **a. VMs — True.** Kubernetes runs well on virtual machines.
- **b. Bare Metal — True.** Kubernetes can run directly on physical servers.
- **c. Cloud infrastructure — True.** Kubernetes can run as a managed service (EKS, GKE, AKS) or self-managed on cloud VMs.
- **d. All of the above — True.** Since all three are valid, this is correct.

---

## 7. A Service can be associated with multiple Endpoints.

**Options:**
- a. True
- b. False

**Correct answer: a. True**

A Service uses a label selector to identify matching Pods; each matching Pod becomes an endpoint. Since Services are typically backed by multiple Pods for scalability/HA, they're associated with multiple endpoints tracked in an `Endpoints` object (or `EndpointSlice`).

---

## 8. Containers of a Pod can be scheduled on different nodes.

**Options:**
- a. True
- b. False

**Correct answer: b. False**

All containers in a single Pod are always scheduled together on the same Node because they share a network namespace (same IP, communicate via `localhost`) and can share storage volumes. A Pod is scheduled as one atomic unit.

---

## 9. A DaemonSet controller schedules multiple instances of its Pod on each cluster node.

**Options:**
- a. True
- b. False

**Correct answer: b. False**

A DaemonSet ensures **exactly one** copy of its Pod runs on each eligible Node — not multiple. Common use cases: log collectors (Fluentd), node monitoring agents (node-exporter), storage daemons.

---

## 10. Creating a PersistentVolumeClaim means that the Volume is mounted automatically inside a Pod.

**Options:**
- a. True
- b. False

**Correct answer: b. False**

Creating a PVC only requests storage and binds it to a PersistentVolume. To actually mount it, you must explicitly reference the PVC under a Pod's `spec.volumes` and reference that volume in the container's `volumeMounts`.

---

## 11. A pod can self-heal.

**Options:**
- a. True
- b. False

**Correct answer: b. False**

A bare Pod does not self-heal — if it crashes or its Node fails, nothing recreates it automatically. `kubelet` can restart containers per `restartPolicy`, but true self-healing (recreating whole Pods) comes from controllers like ReplicaSet, Deployment, StatefulSet, and DaemonSet.

---

## 12. What resources can containers share inside a Pod?

**Options:**
- a. Same IP address
- b. Same name
- c. Volumes
- d. None of the above

**Correct answers: a, c**

- **a. Same IP address — True.** All containers in a Pod share the same network namespace and IP address.
- **b. Same name — False.** Each container in a Pod must have a unique name.
- **c. Volumes — True.** Containers can share Pod-level volumes, mounted at different paths if needed.
- **d. None of the above — False.** Both a and c are valid.

---

## 13. While creating a PersistentVolumeClaim, a user can specify the size and access mode.

**Options:**
- a. True
- b. False

**Correct answer: a. True**

Users specify `resources.requests.storage` (size) and `accessModes` (RWO, ROX, RWX, RWOP) in the PVC spec. Optionally, a StorageClass or selector can also be specified.

---

## 14. We can implement our own Ingress Controller.

**Options:**
- a. True
- b. False

**Correct answer: a. True**

Kubernetes does not ship with a built-in Ingress Controller — it must be deployed separately. Since it just needs to watch Ingress resources and configure routing accordingly, anyone can build their own (examples: NGINX, Traefik, HAProxy, Contour, Istio Gateway).

---

## 15. Which Control Plane Node component is watched by the 'kube-proxy' of each Worker Node for services and endpoints?

**Options:**
- a. API Server
- b. Controller Manager
- c. Scheduler
- d. All of the above

**Correct answer: a. API Server**

- **a. API Server — True.** `kube-proxy` watches the API Server for changes to Services/Endpoints and updates networking rules accordingly.
- **b. Controller Manager — False.** Not watched directly by kube-proxy.
- **c. Scheduler — False.** Handles Pod placement, unrelated to kube-proxy's networking role.
- **d. All of the above — False.** Only the API Server is watched, since it's the central communication hub.

---

## 16. What is the smallest Kubernetes workload object?

**Options:**
- a. Container
- b. ReplicaSet
- c. Namespace
- d. Pod

**Correct answer: d. Pod**

- **a. Container — False.** Not a Kubernetes object itself; exists only inside Pods.
- **b. ReplicaSet — False.** Manages multiple Pods, so it's larger in scope.
- **c. Namespace — False.** A logical partition, not a workload object.
- **d. Pod — True.** The smallest deployable/schedulable unit that Kubernetes manages directly.

---

## 17. Which file format do we generally use to send an object's creation details to the API Server?

**Options:**
- a. XML
- b. UML
- c. YAML
- d. Plain text

**Correct answer: c. YAML**

- **a. XML — False.** Not natively supported.
- **b. UML — False.** A modeling language, not a data format.
- **c. YAML — True.** The conventional, human-readable format used to author manifests (converted to JSON internally by `kubectl` before submission, since the API Server speaks JSON natively).
- **d. Plain text — False.** Too unstructured for object definitions.

---

## 18. What is etcd?

**Options:**
- a. NoSQL database
- b. Relational database
- c. Distributed key-value store
- d. Graph database

**Correct answer: c. Distributed key-value store**

- **a. NoSQL database — False (imprecise).** Loosely related but not the precise classification.
- **b. Relational database — False.** No tables, rows, or SQL.
- **c. Distributed key-value store — True.** etcd is Kubernetes' consistent, highly-available backing store, using the Raft consensus algorithm, storing all cluster object data as key-value pairs.
- **d. Graph database — False.** Doesn't store nodes/edges relationships.

---

## 19. What does a Deployment automatically create?

**Options:**
- a. Replica Controller
- b. ReplicaSet and Pod
- c. Volumes and Claims
- d. None of the above

**Correct answer: b. ReplicaSet and Pod**

- **a. Replica Controller — False.** Naming distractor; the correct modern term is ReplicaSet (ReplicationController is an older, deprecated concept).
- **b. ReplicaSet and Pod — True.** A Deployment creates a ReplicaSet, which in turn creates and manages Pods (Deployment → ReplicaSet → Pods).
- **c. Volumes and Claims — False.** Not automatically created by Deployments.
- **d. None of the above — False.** Since b is correct.

---

## 20. How can we achieve Service Discovery in Kubernetes?

**Options:**
- a. Environment variable
- b. DNS add-ons
- c. Environment variables and DNS add-ons
- d. None of the above

**Correct answer: c. Environment variables and DNS add-ons**

- **a. Environment variable — Partially true but incomplete.** `kubelet` injects env vars like `{SVCNAME}_SERVICE_HOST` for Services that existed before the Pod was created — but this has limitations.
- **b. DNS add-ons — Partially true but incomplete.** CoreDNS creates DNS records for Services dynamically, working regardless of creation order — the more common/recommended method.
- **c. Environment variables and DNS add-ons — True.** Kubernetes supports both mechanisms.
- **d. None of the above — False.** Both mechanisms are valid.

---

## 21. There is a Kubernetes service type which does not receive a ClusterIP.

**Options:**
- a. True
- b. False

**Correct answer: a. True**

This refers to a **Headless Service** (`clusterIP: None`). It's used for direct Pod-to-Pod communication, DNS resolution returning individual Pod IPs, and is commonly used with StatefulSets.

---

## 22. We can assign more than one label to an object.

**Options:**
- a. True
- b. False

**Correct answer: a. True**

Labels are key-value pairs with no restriction to just one per object. Multiple labels allow objects to be organized and selected along multiple dimensions (app, environment, tier, version, team, etc.), which is how Services/ReplicaSets/Deployments use label selectors to identify Pods.

---

## 23. While storing values, Secrets encrypt data.

**Options:**
- a. True
- b. False

**Correct answer: b. False**

Secrets only **Base64-encode** data by default — not encrypt it. Base64 is trivially reversible and provides no real security. To truly protect Secret data, you must enable encryption at rest for etcd, use RBAC restrictions, or use external tools like HashiCorp Vault or cloud KMS solutions.

---

## 24. Which of the following can be an argument to the kubectl create -f command?

**Options:**
- a. YAML manifest with object definition
- b. Pod name
- c. XML file with object definition
- d. UML file with object definition

**Correct answer: a. YAML manifest with object definition**

- **a. YAML manifest — True.** `-f` expects a file (or directory/URL) containing a manifest defining object(s), typically YAML.
- **b. Pod name — False.** Not applicable to `create -f`; that's used for commands like `get`, `describe`, or `delete`.
- **c. XML file — False.** Not supported by Kubernetes.
- **d. UML file — False.** UML is a diagramming language, not usable for object definitions.

---

## 25. What type of selectors can we use with ReplicaSets?

**Options:**
- a. Equality-based selectors
- b. Set-based selectors
- c. Both equality-based selectors and set-based selectors
- d. None of the above

**Correct answer: c. Both equality-based selectors and set-based selectors**

- **a. Equality-based selectors — True but incomplete.** Simple `key=value` matching (`matchLabels`).
- **b. Set-based selectors — True but incomplete.** More expressive matching using `In`, `NotIn`, `Exists`, `DoesNotExist` operators (`matchExpressions`).
- **c. Both — True.** ReplicaSets support both, unlike the older ReplicationController which only supports equality-based selectors.
- **d. None of the above — False.** Both types are supported.

---

## 26. Which of the following projects has inspired Kubernetes?

**Options:**
- a. Docker Swarm
- b. Borg
- c. ZFS
- d. Raft

**Correct answer: b. Borg**

- **a. Docker Swarm — False.** Actually a competitor/alternative to Kubernetes, not an inspiration.
- **b. Borg — True.** Google's internal cluster management system; many Kubernetes concepts (Pods, labels, services, declarative configuration) trace back to lessons learned from Borg.
- **c. ZFS — False.** A filesystem/volume manager, unrelated to orchestration.
- **d. Raft — False.** A consensus algorithm used by etcd (a supporting technology within the ecosystem), not an inspiration for Kubernetes' overall design.

---

## 27. While exposing a Service using NodePort, ___________.

**Options:**
- a. A ClusterIP is assigned to the service
- b. A Load Balancer is configured automatically, using the underlying cloud infrastructure
- c. The service is exposed at a static port on all the Worker Nodes
- d. The port is opened only on the nodes where Pods are running for the respective service

**Correct answers: a, c**

- **a. A ClusterIP is assigned to the service — True.** `NodePort` is built on top of `ClusterIP`.
- **b. A Load Balancer is configured automatically, using the underlying cloud infrastructure — False.** This is the defining behavior of `LoadBalancer`, not `NodePort`.
- **c. The service is exposed at a static port on all the Worker Nodes — True.** A static port (default range 30000–32767) is opened on every Worker Node.
- **d. The port is opened only on the nodes where Pods are running for the respective service — False.** The port opens on all nodes regardless of Pod placement; `kube-proxy` forwards traffic accordingly.

---

## 28. The record is a valuable option to users when performing Deployment rolling updates and rollbacks.

**Options:**
- a. True
- b. False

**Correct answer: a. True**

The `--record` flag saves the command used to make a change as an annotation (`kubernetes.io/change-cause`) on the Deployment's revision. This makes `kubectl rollout history` show what change was made at each revision, aiding auditing, debugging, and informed rollback decisions.

---

## 29. Kubernetes assigns the IP address defined with the ExternalIP service to a node.

**Options:**
- a. True
- b. False

**Correct answer: b. False**

Kubernetes does **not** assign or configure the ExternalIP on a Node — that's the cluster administrator's responsibility to route externally beforehand. Once traffic arrives at a Node via that IP, `kube-proxy` routes it to the appropriate Pod. This differs from `LoadBalancer`, where Kubernetes *does* automatically provision the external IP via the cloud provider.

---

## 30. What do we define with the apiVersion field in the object's configuration file?

**Options:**
- a. API endpoint to connect to the API server
- b. Object name
- c. Object type
- d. None of the above

**Correct answer: d. None of the above**

- **a. API endpoint to connect to the API server — False.** The client already knows how to reach the API Server via kubeconfig; `apiVersion` isn't a connection endpoint.
- **b. Object name — False.** Defined under `metadata.name`.
- **c. Object type — False.** Defined by the `kind` field, not `apiVersion`.
- **d. None of the above — True.** `apiVersion` specifies the **version of the Kubernetes API** (e.g., `v1`, `apps/v1`, `batch/v1`) used to interpret and validate the object.

---
