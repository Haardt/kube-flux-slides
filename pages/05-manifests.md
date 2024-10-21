---
transition: slide-left
layout: center
background: './images/manifests.webp'
---

# Kubernetes Manifest-Files

---
transition: slide-left
layout: two-cols
---
# Pods

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]

```

::right::

<div v-click>
    <arrow x1="480" y1="55" x2="130" y2="145" color="red" width="2" arrowSize="1" />

- api-Version: The api-version for this specification and kind!
</div>
<div v-click>
    <arrow x1="480" y1="120" x2="90" y2="170" color="red" width="2" arrowSize="1" />

- kind: pod, deployment, config-map, secret or custom resource!
</div>
<div v-click>
    <arrow x1="480" y1="180" x2="130" y2="280" color="red" width="2" arrowSize="1" />

- image: Docker container. This need a Docker-Registry. The image is cached on evey node.
</div>

---
transition: slide-left
layout: default
background:
---

## Pods

````md magic-move

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]
# Added pod resources      
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]
# The mount path in the container      
      volumeMounts:
        - name: my-storage
          mountPath: /data
# Added pod resources      
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
# Add persitend volume to the pod      
  volumes:
    - name: my-storage
      persistentVolumeClaim:
        claimName: my-pvc


```

````
---
transition: slide-left
layout: default
---
## Pod and environments

````md magic-move

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
# Meta infos  
  labels:
    app: hello-world
    environment: production
  annotations:
    description: "Description"
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
# Meta infos  
  labels:
    app: hello-world
    environment: production
  annotations:
    description: "Description"
spec:
  containers:
    - name: hello-world-container
      image: busybox
      command: ["echo", "Hello, World!"]
# Set environment vars in the container
    env:
     - name: GREETING_MESSAGE
       value: "Hello from Kubernetes"
     - name: ENVIRONMENT
       valueFrom:
         fieldRef:
           fieldPath: metadata.labels['environment']      

```

````
---
transition: slide-left
layout: two-cols
---
## Deployments

````md magic-move

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-deployment
  labels:
    app: hello-world
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-world-container
          image: nginx
          ports:
            - containerPort: 80


```
````

::right::

<div v-click>
    <arrow x1="480" y1="55" x2="130" y2="235" color="red" width="2" arrowSize="1" />

- *selector*: used to define which Pods the Deployment is responsible for managing

</div>

<div v-click>
    <arrow x1="480" y1="120" x2="120" y2="285" color="red" width="2" arrowSize="1" />

- *template*: The "configuration" for new created pods

</div>

<div v-click>
    <arrow x1="480" y1="150" x2="130" y2="360" color="red" width="2" arrowSize="1" />

- *spec*: The spec of the pod.

</div>

---
transition: slide-left
layout: default
---
## Deployment

````md magic-move

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-deployment
  labels:
    app: hello-world
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-world-container
          image: nginx
          ports:
            - containerPort: 80
```

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 3
# Update strategy: RollingUpdate, Recreate, ...?
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1    
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-world-container
          image: nginx
          ports:
            - containerPort: 80
```

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello-world
  template:

    spec:
      containers:
        - name: hello-world-container
          image: ....
# Probes and hooks
      livenessProbe:
        httpGet:
          path: /healthz
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 10

```
````
---
transition: slide-left
layout: two-cols
---
## Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
spec:
  selector:
    app: hello-world
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

```

::right::

<div v-click>
    <arrow x1="480" y1="55" x2="130" y2="175" color="red" width="2" arrowSize="1" />

- *selector*: matches labels on pods

</div>

<div v-click>
    <arrow x1="480" y1="90" x2="160" y2="285" color="red" width="2" arrowSize="1" />

- *ClusterIP*: The ClusterIP type means it’s only accessible from within the cluster, so other Pods and services in the same cluster can access it.

</div>

---
transition: slide-left
layout: default
---
## Service features
````md magic-move

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
spec:
  selector:
    app: hello-world
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
# Metadata and labels  
  labels:
    app: hello-world
    environment: production
spec:
  selector:
    app: hello-world
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
# Labels
  labels:
    app: hello-world
    environment: production
spec:
  selector:
    app: hello-world
 ports:
    - protocol: TCP
      port: 80
      targetPort: 8080  # The port that the container listens on
      nodePort: 30001   # Optional: only needed for NodePort or LoadBalancer types
  type: NodePort  # Exposes the service on a port that can be accessed externally
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
# Labels
  labels:
    app: hello-world
    environment: production
spec:
  selector:
    app: hello-world
 ports:
    - protocol: TCP
      port: 80
      targetPort: 8080  # The port that the container listens on
      nodePort: 30001   # Optional: only needed for NodePort or LoadBalancer types
  type: NodePort  # Exposes the service on a port that can be accessed externally
  sessionAffinity: ClientIP  # Ensures clients are routed to the same Pod (useful for stateful apps)
  externalTrafficPolicy: Local  # Ensures that traffic is routed to the node where the Pod is running

```


````
---
transition: slide-left
layout: two-cols
---

## Ingress
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: sub.example.com
      http:
        paths:
          - path: /hello
            pathType: Prefix
            backend:
              service:
                name: hello-world-service
                port:
                  number: 80

```

::right::

<div v-click>
    <arrow x1="480" y1="55" x2="220" y2="175" color="red" width="2" arrowSize="1" />

- *annotations*: Configuration for the ingress-**controller**.

</div>

<div v-click>
    <arrow x1="480" y1="120" x2="140" y2="230" color="red" width="2" arrowSize="1" />

- *virtual hosts*: The ClusterIP type means it’s only accessible from within the cluster, so other Pods and services in the same cluster can access it.

</div>

<div v-click>
    <arrow x1="480" y1="220" x2="200" y2="340" color="red" width="2" arrowSize="1" />

- *server*: Specifies the backend service that the Ingress routes traffic to.

</div>

---
transition: slide-left
layout: default
---

## Ingress and TLS
````md magic-move

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: sub.example.com
      http:
        paths:
          - path: /hello
            pathType: Prefix
            backend:
              service:
                name: hello-world-service
                port:
                  number: 80

```

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    cert-manager.io/cluster-issuer: letsencrypt-prod  # Refers to the ClusterIssuer
spec:
# Cert-Manager fill the secret with LetsEncrypt
  tls:
    - hosts:
        - sub.example.com
      secretName: hello-world-tls  # Secret where the TLS certificate will be stored
  rules:
    - host: sub.example.com
      http:
        paths:
          - path: /hello
            pathType: Prefix
            backend:
              service:
                name: hello-world-service
                port:
                  number: 80

```

```yaml
# Custom-Resource from Cert-Manager!
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your-email@example.com
    privateKeySecretRef:
      name: letsencrypt-prod-key
    solvers:
      - http01:
          ingress:
            class: nginx  # Depend on the Ingress Controller (Traefik, nginx, ...)?

```

```yaml
# Custom-Resource from Traefik for routing tcp (TLS-SNI)

apiVersion: traefik.containo.us/v1alpha1
kind: IngressRouteTCP
metadata:
  name: hello-world-tcp
  namespace: default
spec:
  entryPoints:
    - websecure-tcp  # Define the entry point in your Traefik configuration
  routes:
    - match: HostSNI(`*`)  # Match all hostnames (wildcard)
      services:
        - name: hello-world-tcp-service
          port: 3306  # Route traffic to this service

```
````

---
transition: slide-left
layout: two-cols
---
## Secrets

````md magic-move

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  db.username: bXlfdXNlcm5hbWU=  # base64
  db.password: bXlfcGFzc3dvcmQ=  # base64
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: busybox
      command: ["sh", "-c", "echo Hello from \
                 $APP_NAME in $APP_ENV; sleep 3600"]
      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: db.username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: db.password
```
````

::right::
## Config-Maps

````md magic-move

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  app.name: "HelloWorldApp"
  app.environment: "production"
  app.version: "1.0.0"

```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: busybox
      command: ["sh", "-c", "echo \
                    $APP_NAME in $APP_ENV"]
      env:
        - name: APP_NAME
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: app.name
        - name: APP_ENV
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: app.environment
```

````

---
transition: slide-left
layout: default
---

# Conclusion

<div v-click>

- **Auto-scaling**: Automatically adjusts the number of containers based on demand.

</div>
<div v-click>

- **Self-healing**: Restarts failed containers and ensures the desired state of applications.

</div>

<div v-click>

- **Service Discovery & Load Balancing**: Provides automatic service discovery and evenly distributes traffic across containers.

</div>

<div v-click>

- **Storage Orchestration**: Manages storage resources dynamically, supporting various storage solutions.

</div>

<div v-click>

- **Rollouts & Rollbacks**: Seamlessly updates applications and allows easy rollbacks in case of issues.

</div>

<div v-click>

- **Event-driven architecture**: It reacts to changes in the cluster state, automatically adjusting resources based on events.

</div>


<div v-click>

- **Custom Resources**: Through Custom Resource Definitions (CRDs), Kubernetes can be extended to create custom platforms tailored to specific needs.

</div>


