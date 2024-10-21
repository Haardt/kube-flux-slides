---
transition: slide-left
layout: center
---
# One-Hub (NEON)

---
transition: slide-left
layout: image
image: ./images/one-hub-direct-streaming.png
backgroundSize: contain
---

---
transition: slide-left
layout: image
image: ./images/virtual-actor.png
backgroundSize: contain
---

---
transition: slide-left
layout: image
image: ./images/neon-architecture.png
backgroundSize: contain
---
# NEON (Video-Meeting)

---
transition: slide-left
layout: image-right
image: ./images/neon-bridge-sfu.png
backgroundSize: contain
---
# Bridge

- The bridge is a SFU = Selective Forwarding Unit
- Can run on any docker <span v-mark.underline.orange>host</span>.
- The bridge registers itself with the cluster.
- There is no <span v-mark.underline.orange>cluster-to-bridge connection</span>.


---
transition: slide-left
layout: image
image: ./images/bridge-interfaces.png
backgroundSize: contain
---

---
transition: slide-left
layout: image-left
image: ./images/neon-external-interfaces.png
backgroundSize: contain
---
# External Interfaces

- Tell don't ask concept


---
transition: slide-left
layout: image-left
image: ./images/sun-and-ass.webp
backgroundSize: contain
---
# Darkness

- complex helm values (e.g prometheus, mongodb)
- jsonnet
- logging (Grafana Loki fail)
- tracing (no jaeger)
- too little documentation
- current cluster security
- cluster resources by IONOS (unused storage)
- Fucking static ips (Gitlab, Container-Registry, Customer-Firewalls)
- Shared passwords (Third party services)
- TBD

---
transition: slide-left
layout: default
---
# Missing pieces

- Teams (NATS Pub/Sub)
- Monitoring and altering
- Rebuild on fresh cluster in 30 minutes
- NEON-Admin-Portal
- Certificate (Bridge, Cluster -> Cert-Manager)
- Turn-Server (coturn)
- IONOS
- Host-Europe (EMail)
- 

