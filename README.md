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

## Promethues


Prometheus is an open-source monitoring and alerting toolkit originally built by SoundCloud in 2012. It is now a standalone open-source project maintained by the Cloud Native Computing Foundation (CNCF). Prometheus is designed for reliability, scalability, and ease of use. 

Its main components include:

- Prometheus Server: The core component responsible for collecting and storing time-series data metrics from various targets. It scrapes and stores metrics data in its time-series database, allowing for querying and analysis.

- Data Model: Prometheus uses a multidimensional data model with time series identified by metric name and key/value pairs. This model allows flexible querying and aggregation over time.

- PromQL: Prometheus Query Language is a powerful expression language for querying and manipulating metrics data. It enables users to perform ad-hoc analysis, create dashboards, and set up alerting rules.

- Prometheus Alertmanager: This component handles alerts sent by client applications such as Prometheus server. It manages groupings, deduplications, and routing of alerts to various notification channels (e.g., email, PagerDuty, Slack).

- Service Discovery: Prometheus supports multiple service discovery mechanisms to dynamically discover and monitor targets. This includes DNS-based discovery, Kubernetes service discovery, and integrations with cloud service providers.

- Exporters: Exporters are small programs that expose metrics from third-party systems in a format Prometheus can understand. There are exporters available for various technologies like databases, web servers, and cloud platforms.

- Push Gateway: In some cases, where the architecture doesn't allow Prometheus to scrape metrics directly from a target, the Push Gateway can be used. It allows short-lived jobs to push their metrics to Prometheus.

- Grafana: While not a part of Prometheus itself, Grafana is often used alongside Prometheus for visualization and monitoring. It provides rich visualization capabilities, dashboards, and alerting features that complement Prometheus's monitoring capabilities.

These components work together to provide robust monitoring, alerting, and visualization solutions for modern cloud-native architectures.