# Understanding of Ingress,helm,promethues

This repo contains all the configuration steps and commands 

## Helm 

In Kubernetes, Helm is a package manager that facilitates the deployment and management of applications on a Kubernetes cluster. It helps in defining, installing, and upgrading complex Kubernetes applications. 

Here's a brief overview of how <b> Helm </b> works:

- Charts: Helm packages for Kubernetes applications.
- Templates: Files allowing parameterization and dynamic generation of Kubernetes manifests.
- Repositories: Collections of packaged charts for distribution.
- Client-Server Architecture: Helm client interacts with Kubernetes cluster via Tiller (prior to Helm v3) or directly (Helm v3 onwards).
- Lifecycle Management: Simplifies installation, upgrade, rollback, and uninstallation of Kubernetes applications with release history management.

Overall, Helm streamlines the process of managing Kubernetes applications, making it easier to deploy and manage complex workloads. However, with the introduction of Helm v3, Tiller was removed, simplifying the architecture and improving security by directly interacting with the Kubernetes API server.


## Ingress

In Kubernetes, an Ingress is an API object that serves as a way to manage external access to services within a cluster. 

Its main components include:

- Ingress Resource: This is the primary Kubernetes resource for defining rules for routing external HTTP(S) traffic to services within the cluster. It specifies how incoming requests should be directed to different services based on criteria like hostnames, paths, or other HTTP headers.

- Ingress Controller: An Ingress Controller is a component responsible for implementing the rules specified in the Ingress resource. It typically runs as a pod within the cluster and continuously monitors the Ingress resources, configuring the underlying load balancer (or equivalent) to route traffic accordingly. Examples of Ingress controllers include NGINX Ingress Controller, Traefik, and HAProxy Ingress.

- Backend Services: These are the Kubernetes services that the Ingress routes traffic to. Each service typically represents a set of pods providing a specific functionality, such as a web server or an API.

- Rules: Ingress rules define how incoming requests should be routed to different backend services. Rules can include criteria like hostnames, paths, or other HTTP headers. Each rule specifies the backend service to send matching requests to.

- Annotations: Ingress annotations are optional metadata that can be added to Ingress resources to provide additional configuration options or hints to the Ingress controller. Annotations can be used to customize the behavior of the Ingress controller, such as configuring SSL termination, setting up rate limiting, or defining rewrite rules.

Together, these components enable Kubernetes users to define and manage complex routing configurations for handling incoming HTTP(S) traffic to services within the cluster.