# Introduction to Linkerd Service Mesh
[![Linkerd-logo](doc-images/linkerd.jpg)](doc-images/linkerd.jpg)

## What is Sevice Mesh ?

A service mesh is a tool for adding observability, security, and reliability features to “cloud native” applications by transparently inserting this functionality at the platform layer rather than the application layer.
The service mesh is typically implemented as a scalable set of network proxies deployed alongside application code (a pattern sometimes called a sidecar). These proxies handle the communication between the microservices and also act as a point at which the service mesh features can be introduced.Service mesh allows you to separate the business logic of the application from observability, and network and security policies. It allows you to connect, secure, and monitor your microservices.
The service mesh is rapidly becoming a standard part of the cloud native stack, especially for Kubernetes adopters.
Linkerd is a service mesh for Kubernetes.Linkerd.Linkerd is a Cloud Native Computing Foundation-graduated project that is totally open source and licensed under Apache v2.

This manual will show you step-by-step how to deploy Linkerd in your Kubernetes cluster. Next, we'll launch a demonstration application.

Description available on my medium story : https://medium.com/@saifeddine.segni94/introduction-to-linkerd-service-mesh-f31ab2ff1acc

