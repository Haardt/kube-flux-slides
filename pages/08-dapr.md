---
layout: default
---
# Microsoft DAPR (Distributed Application Runtime)

- **Service-to-Service Communication**: DAPR facilitates secure, reliable communication between services using HTTP, <span v-mark.underline.orange>gRPC</span>, or other protocols.
- **State Management**: It offers APIs for managing state in a distributed application, abstracting away the complexity of dealing with distributed storage solutions.
- **Pub/Sub Messaging**: DAPR enables publish/subscribe messaging across microservices using various messaging systems (like Kafka, RabbitMQ).
- **Bindings**: DAPR supports integration with external systems (e.g., databases, cloud services) via input/output bindings, simplifying event-driven architectures.
- **Observability**: It includes features for tracing, logging, and monitoring distributed applications.

DAPR is platform-agnostic and works with <span v-mark.underline.orange>any programming language. </span>
 
---
layout: image
transition: slide-left
image: ./images/building_blocks.png
backgroundSize: contain
---
# Building blocks
---
layout: default
transition: slide-left
---
# The DAPR glue:

<div style="text-align: center">
<br>
<br>
<br>
<br>
<br>

# Sidecars

<div v-click>

two containers in one pod

with localhost binding

</div>

</div>


