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


