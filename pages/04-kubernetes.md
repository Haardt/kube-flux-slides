# Why Kubernetes

- Service discovery and load balancing (*DNS*)
- Storage orchestration (*Nodes*)
- Automated rollouts and rollbacks (*Manifests*)
- Automatic bin packing (*CPU*, *RAM*, ...)
-  <span v-mark.underline.orange>Self-healing (*Events*)</span>
- Secret and configuration management
- Batch execution
- Horizontal scaling (*Repicas*, ...)
- IPv4/IPv6 dual-stack
- Designed for extensibility (*Custom Resources*)


---
transition: slide-left
layout: default
---
# What kubernetes is not

- No limits on workload (Hmm... RTP?)
- Does not deploy source code and does not build your application.
- Does not provide application-level services.
- Does not dictate <span v-mark.underline.orange>logging, monitoring, or alerting solutions</span>
- Does not provide nor mandate a configuration language/system.
- Does not provide nor adopt any comprehensive machine configuration, maintenance, management, or self-healing systems.
- Additionally, Kubernetes is not a mere orchestration system. In fact, it eliminates the need for orchestration.

---
transition: slide-left
layout: image
image: './images/components-of-kubernetes.svg'
backgroundSize: contain
---

---
transition: slide-left
layout: two-cols
---
# Components


## Control-Plane

- kube-api-server
- etcd
- kube-scheduler
- kube-controller-manager


## Node-Services

- kubelet
- kube-proxy
- Container runtime (containerd, CRI-O, WebAssembly,...)

::right::


# Addons

- DNS (coreDNS)
- CNI (container network interface | SDN) (calico)
 - Traffic shaping, eBPF, ... 
- Custom-Services like monitoring 

---
transition: slide-left
layout: default
---
# Kubernetes Node
- self-registration (on API-Service)
- cloud-provider support (get metadata about itself)
- IP4/IP6 IPs management
- Labels support (e.g. for GPU-Nodes)
- Health monitoring
- Cordon support ("unschedulable")
- <span v-mark.circle.orange>Drain support</span>
- Resource tracking (CPU/RAM)

---
transition: slide-left
layout: default
---

# Node Examples
- kubectl get nodes
- kubectl get nodes -o wide
- kubectl describe node apps-2024-06-pool2-5gpd5bnv6l
- kubectl cordon node-name
- kubectl drain node-name
- kubectl label nodes node-name label-key=label-value
- kubectl top node apps-2024-06-pool2-5gpd5bnv6l
- kubectl rollout restart node node-name
- kubectl get events --all-namespaces

---
transition: slide-left
layout: two-cols
---

# Pod/Container
- <span v-mark.underline.orange>Environment variables</span> and PVs access
- Status:
  - Scheduled, Pulled, Pulling, Started, Killing, Backoff, ...
- Events:
  - Logs occurred statues (e.g. started...) 
- Hooks:
  - PostStart, PreStop, Readiness, Healthy, ...
- Sidecars (Init-Container, DAPR-Sidecar, ...)
- Resources:
  - Limits and **Requests** (CPU, RAM)
- Pod Topology Spread Constraints
  - maxSkew, topologyKey, labelSelector, ...

::right::


```plantuml
@startuml
skinparam component {
  BackgroundColor<<Pod>> LightSkyBlue
  BackgroundColor<<Node>> LightGreen
  BackgroundColor<<Container>> LightYellow
  BorderColor Black
  RoundCorner 15
}

component "Node" <<Node>> {
  component "Pod-A" <<Pod>> {
    component "Container-A" <<Container>>
  }
  component "Pod-X" <<Pod>> {
    component "Container-X" <<Container>>
  }
}

@enduml

```

<arrow v-click x1="510" y1="210" x2="230" y2="450" color="red" width="2" arrowSize="1" />


---
transition: slide-left
layout: default
---

# Pod Examples
- kubectl get pods
- kubectl get pods --namespace staging
- kubectl describe pod meeting-service-578d8f55b6-552bl
- kubectl logs meeting-service-578d8f55b6-552bl
- kubectl logs -lapp=meeting-service
- kubectl delete pod meeting-service-578d8f55b6-552bl
- kubectl exec -it pod-name -n namespace -- /bin/bash
- kubectl get pods --all-namespaces --field-selector spec.nodeName=meeting-service

---
transition: slide-left
layout: two-cols
---

# Kubernetes Objects
- Node, Pod, Container
- More:
  - Namespace
  - Provision-Objects: *Stateful-Sets*, *Deployment*
  - Routing-Objects: *Service*, *Ingress*, *Gateway?*
  - Config-Objects: *Config Map*, *Secret*
  
::right::


```plantuml
@startuml
skinparam component {
  BackgroundColor<<Pod>> LightSkyBlue
  BackgroundColor<<Node-X>> LightGreen
  BackgroundColor<<Node-Y>> LightGreen
  BackgroundColor<<Container>> LightYellow
  BackgroundColor<<Service>> LightCoral
  BackgroundColor<<Ingress>> LightGoldenRodYellow
  BackgroundColor<<Deployment>> LightSlateGray
  BorderColor Black
  RoundCorner 15
}
component "Namespace" <<Namesspace>> {
  component "Node-X" <<Node-X>> {
    component "Pod-A" <<Pod>> {
      component "Container-A" <<Container>>
    }
    component "Pod-X" <<Pod>> {
      component "Container-X" <<Container>>
    }
  }

  component "Node-Y" <<Node-Y>> {
    component "Pod-A-R2" <<Pod>> {
      component "Container-A-S" <<Container>>
    }
    component "Pod-X-R2" <<Pod>> {
      component "Container-X-S" <<Container>>
    }
  }
  
  component "Ingress" <<Ingress>> {
  }
  
  component "Service" <<Service>> {
  }
  
  component "Deployment" <<Deployment>> {
  }
  
  Ingress --> Service
  Service --> "Node-X"
  Service --> "Node-Y"
  Deployment --> "Node-X"
  Deployment --> "Node-Y"
}
@enduml

```

<arrow v-click x1="610" y1="80" x2="190" y2="200" color="red" width="2" arrowSize="1" />


---
transition: slide-left
layout: default
---
# Namespaces
**Namespaces** are used to logically divide and <span v-mark.underline.orange>isolate</span> resources within a cluster. 
They allow multiple teams or applications to coexist within the same cluster by providing separate environments.
This helps in organizing resources, avoiding naming conflicts, and enforcing security and access controls.
Namespaces enable efficient resource management and allow for setting limits,
such as CPU or memory quotas, within each isolated environment.
Thus, they enhance scalability and manageability in large clusters by partitioning resources logically
without requiring separate physical clusters.
- RBAC security and policies
- Support by the DNS system (isolation - e.g. ping hostname, simple Service-Discovery)

kubectl get namespaces


---
transition: slide-left
layout: two-cols
---
# Deployments
**Deployments** are used to manage and maintain stateless applications by defining how many replicas (instances) of a Pod should be running at any given time.
A Deployment ensures that a specified number of identical Pods are running,
automatically restarting failed Pods and managing updates or rollbacks without downtime.

It provides features such as <span v-mark.underline.orange>scaling, rolling updates</span>, and <span v-mark.underline.orange>rollback capabilities</span>, allowing for controlled and easy management of application lifecycle and
<span v-mark.underline.orange>versioning</span>. Deployments are ideal for workloads where Pods are interchangeable and do not require persistent state.

::right::

```plantuml
@startuml
skinparam component {
  BackgroundColor<<Deployment>> LightSkyBlue
  BackgroundColor<<Node>> LightCoral
  RoundCorner 15
}
skinparam node {
  BackgroundColor<<Node>> LightGreen
  RoundCorner 15
}
node "Kubernetes Cluster" {
  component Namespace {  
    component Deployment <<Deployment>> {
      node "Node 1" <<Node>> {
              node "Pod 0" {
                  [Container 0] --> PersistentVolume0
              }
     }
      
      node "Node 2" <<Node>> {
              node "Pod 1" {
                  [Container 1] --> PersistentVolume1
              }
          }
      
      node "Node 3" <<Node>> {
              node "Pod 2" {
                  [Container 2] --> PersistentVolume2
              }
          }
      }
  }
}
@enduml
```

---
transition: slide-left
layout: two-cols
---
# Statefull-Sets
A StatefulSet is used to manage <span v-mark.underline.orange>stateful applications</span> that require stable, 
unique network identities and persistent storage. Unlike regular Deployments where pods are interchangeable, 
StatefulSets ensure that each pod has a fixed identity, ordered deployment and termination, and persistent storage volumes 
that retain data even if the pod is restarted. This makes it ideal for managing databases and other stateful services.
In addition, the DNS pod names are defined and <span v-mark.underline.orange>numbered sequentially</span>.

### Useful for
- Databases
- Cluster (Service-Programms)
- Pub/Sub-Services

::right::

```plantuml
@startuml
skinparam component {
  BackgroundColor<<StatefulSet>> LightSkyBlue
  BackgroundColor<<Node>> LightCoral
  RoundCorner 15
}
skinparam node {
  BackgroundColor<<Node>> LightGreen
  RoundCorner 15
}
node "Kubernetes Cluster" {
  component Namespace {  
    component StatefulSet <<StatefulSet>> {
      node "Node 1" <<Node>> {
              node "Pod 0" {
                  [Container 0] --> PersistentVolume0
              }
     }
      
      node "Node 2" <<Node>> {
              node "Pod 1" {
                  [Container 1] --> PersistentVolume1
              }
          }
      
      node "Node 3" <<Node>> {
              node "Pod 2" {
                  [Container 2] --> PersistentVolume2
              }
          }
      }
  }
}
@enduml
```

---
transition: slide-left
layout: two-cols
---
# Ingress & Service
A **Ingress** manages <span v-mark.underline.orange>external access</span> to services within the cluster, typically via HTTP/HTTPS. It defines rules for routing traffic to specific services based on hostnames, paths, or other conditions. Ingress can also handle SSL termination and offer features like load balancing and virtual hosting, providing a more flexible and feature-rich alternative to exposing services through NodePort or LoadBalancer.

A **Service** provides a <span v-mark.underline.orange>stable endpoint</span> to access a set of Pods. It abstracts the underlying Pod IPs, which can change as Pods are created and destroyed, and ensures reliable communication within the cluster or externally. Services can load balance traffic across multiple Pods and offer different types like ClusterIP (internal access), NodePort (external access via node IP), and LoadBalancer (provider’s load balancer).

::right::

# Load-Balancing

```plantuml

@startuml
actor Client

node "Kubernetes Cluster" {

    node "Ingress Controller" {
        [Ingress] --> [Service A] : Routes to Service A based on rules
        [Ingress] --> [Service B] : Routes to Service B based on rules
    }
    
    node "Node 1" {
        [Service A] --> PodA1 : Load balances to PodA1
        [Service B] --> PodB1 : Load balances to PodB1
    }
    
    node "Node 2" {
        [Service A] --> PodA2 : Load balances to PodA2
        [Service B] --> PodB2 : Load balances to PodB2
    }
    
    node "Node 3" {
        [Service A] --> PodA3 : Load balances to PodA3
        [Service B] --> PodB3 : Load balances to PodB3
    }
}

Client --> Ingress : Sends request
@enduml
```
<br>

- Load-Balancing
- TLS
- A Ingress-Controller is implemented by Traefik, NGINX, ...,  and hyperscaler LBs
- kubectl get service, kubectl get ingress

---
transition: slide-left
layout: default
---
# Configs and Secrets

**ConfigMaps** are used to store configuration data in key-value pairs. They allow you to 
<span v-mark.underline.orange>decouple configuration</span>
artifacts from container images, 
enabling you to configure applications dynamically without rebuilding container images. 
ConfigMaps can provide
<span v-mark.underline.orange>environment</span> variables, command-line arguments, or configuration <span v-mark.underline.orange>files</span> to your pods, 
helping manage application configuration in a more flexible and centralized manner.

<br>

**Secrets** are used to store sensitive information, such as passwords, tokens, or keys, in a secure way. 
Unlike ConfigMaps, which store non-sensitive data, Secrets ensure that confidential information is encoded and
only accessible to <span v-mark.underline.orange>authorized</span> pods.
This helps protect sensitive data from being exposed in plaintext within configuration files or environment variables,
allowing secure management of critical information in your applications.

- kubectl get configs
- kubectl get secrets
