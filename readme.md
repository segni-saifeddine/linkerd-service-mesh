# Introduction to Linkerd Service Meshes:
[![Linkerd-logo](doc-images/linkerd.jpg)](doc-images/linkerd.jpg)

## What is Sevice Mesh ?

A service mesh is a tool for adding observability, security, and reliability features to “cloud native” applications by transparently inserting this functionality at the platform layer rather than the application layer.
 The service mesh is rapidly becoming a standard part of the cloud native stack, especially for Kubernetes adopters
Service mesh allows you to separate the business logic of the application from observability, and network and security policies. It allows you to connect, secure, and monitor your microservices.

Linkerd is a service mesh for Kubernetes.Linkerd is fully open source, licensed under Apache v2, and is a Cloud Native Computing Foundation graduated project.

In this guide, we’ll walk you through how to install Linkerd into your Kubernetes cluster. Then we’ll deploy a sample application to show off what Linkerd can do.

## Why Use a Service Mesh?

## Linkerd components
At a high level, Linkerd consists of a control plane and a data plane.

[![Linkerd-architecture](doc-images/Linkerd-Archi.PNG)](doc-images/Linkerd-Archi.PNG)

1- the Control plane: providing the services for automatically injecting data plane components into pods(poxy-injector), generate certificates used in mTLS communications in the data plane and authorized data plane componentes (identity), and traffic flow control services (destination)
- The destination service is used by the data plane proxies to determine various aspects of their behavior
- The identity service acts as a TLS Certificate Authority that accepts CSRs from proxies and returns signed certificates. These certificates are issued at proxy initialization time and are used for proxy-to-proxy connections to implement mTLS.
- The proxy injector is a Kubernetes admission controller that receives a webhook request every time a pod is created

2- The Data Plane, transparent proxy running as sidecar container within the pods. Proxies automatically intercept Pod’s inbound/outbound TCP traffic and add transparantly encryption (mTLS), Later-7 load balancing, routing, retries, telemetry, etc.
- The Linkerd2-proxy is an ultralight, transparent micro-proxy written in Rust. Linkerd2-proxy is designed specifically for the service mesh use case and is not designed as a general-purpose proxy.
- The linkerd-init container is added to each meshed pod as a Kubernetes init container that runs before any other containers are started. It uses iptables to route all TCP traffic to and from the pod through the proxy.

> Linkerd also provides a CLI that can be used to interact with the control and data planes.
## Assumptions and prerequisites
- Basic hands-on experience with Kubernetes.
- Access to modern Kubernetes cluster
- A functioning kubectl command on your local machine

## Step by step :Install Linkerd

During this section, we’ll borrow instructions from the linkerd official documentation page.
we’ll install the Linkerd CLI and use it validate that your cluster is capable of hosting Linkerd.

1- Install the CLI
We will need to download the linkerd CLI onto your local machine. The CLI will allow you to interact with your Linkerd deployment.
```
$ curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/install | sh
```
Once installed, verify the CLI is running correctly with:
```
$ linkerd version
```
2- Validate your Kubernetes cluster
Before we can install the Linkerd control plane, we need to check and validate that everything is configured correctly
```
$ linkerd check --pre
```
3- Install Linkerd onto your cluster
 it’s time to install Linkerd on your Kubernetes cluster , here are two methods:

- The CLI
- Helm

We’ll demonstrate using Cli ,To do this, run:

```$ linkerd install --crds | kubectl apply -f -
$ linkerd install | kubectl apply -f -
```
> The install --crds command installs Linkerd’s Custom Resource Definitions (CRDs), which must be installed first, while the install command installs the Linkerd control plane.
- It may take a minute or two for the control plane to finish installing. Wait for the control plane to be ready (and verify your installation) by running:

```
$ linkerd check
```
## Demo App
- Congratulations, Linkerd is installed! However, it’s not doing anything just yet. To see Linkerd in action, we’re going to need an application.
We will use an application called " Emojivoto" , it uses a mix of gRPC and HTTP calls to allow the user to vote on their favorite emojis.
* First let's cone the application repos

* Create an namespace for the demo application :
```
$ Kubectl create ns emojivoto
```
* Then we will deploy four applications: emoji , vote-bot ,  voting and web , then create the services to expose this applications, finally create three ServiceAccount : emoji , voting and web

[![emoji-app](doc-images/app-archi.PNG)](doc-images/app-archi.PNG)

* Forward web-svc locally to port 8080 to take a look at Emojivoto in its natural state
```
$ kubectl -n emojivoto port-forward svc/web-svc 8080:80
```

* Once deployed, you’ll need a way to get the Linkerd sidecar into the application Namespace. The easiest way in this instance is to inject the sidecar into every containerized deployment via the inject command.
```
$ kubectl get -n emojivoto deploy -o yaml \
  | linkerd inject - \
  | kubectl apply -f -
```
* Visit again the application at http://localhost:8080 ,We’ve added Linkerd to Emojivoto, but there are no visible changes to the application! That is part of Linkerd’s design—it does its best not to interfere with a functioning application.

* To take a closer look at what Linkerd is actually doing we need to add the linkerd viz extension :
```
$ linkerd viz install | kubectl apply -f -
```

The final step is to view the microservice that you deployed in the application installation section in the Linkerd UI , run Viz locally.
```
$ linkerd viz dashboard &
```
> The Viz dashboard will automatically open up for you. Change the Namespace to the emojivoto Namespace and you’ll now see that the containerized applications running in the emojivotoNamespace are secured with Linkerd.

## Debug the emojivoto app by using Linkerd

## That’s it! 👏

- Congratulations, you have joined the exalted ranks of Linkerd users! Give yourself a pat on the back.thanks for reading 🙏
What’s next? Here are some usufuel references :
- https://linkerd.io/
- https://www.redhat.com/en/topics/microservices/what-is-a-service-mesh
- https://training.linuxfoundation.org/training/introduction-to-service-mesh-with-linkerd-lfs143/
- https://istio.io/latest/about/service-mesh/s
