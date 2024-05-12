## Installation steps for Nginx Ingress Controller 

This guide explains how to install NGINX Ingress Controller in a Kubernetes cluster using manifests. In addition, it provides instructions on how to set up role-based access control, create  resources  and also uninstall NGINX Ingress Controller.

## before you start
Get the NGINX Controller Image

<b>Note:</b>
<br>
Always use the most up-to-date stable release listed on the releases page.
<br>
Choose one of the following methods to get the NGINX Ingress Controller image:

<b>NGINX Ingress Controller:</b> 
<br>
Download the image nginx/nginx-ingress from DockerHub.

If you have not downloaded the image no problem while performing other steps image will be downloaded automatically.


## Clone the repository

Clone the NGINX Ingress Controller repository using the command shown below, and replace <version_number> with the specific release you want to use.

 ```

git clone https://github.com/nginxinc/kubernetes-ingress.git --branch <version_number>

```
For example,

If you want to use version 3.5.1, the command would be
```
 git clone https://github.com/nginxinc/kubernetes-ingress.git --branch v3.5.1.

```

This guide assumes you are using the latest release.

## Set up role-based access control (RBAC)

Admin access required
<br>
To complete these steps you need admin access to your cluster. Refer to to your Kubernetes platform’s documentation to set up admin access. For Google Kubernetes Engine (GKE), you can refer to their Role-Based Access Control guide.

- Create a namespace and a service account:
```
kubectl apply -f deployments/common/ns-and-sa.yaml
```
- Create a cluster role and binding for the service account:
 ```
kubectl apply -f deployments/rbac/rbac.yaml
```

 
## Create common resources

In this section, you’ll create resources that most NGINX Ingress Controller installations require:

(Optional) Create a secret for the default NGINX server’s TLS certificate and key. Complete this step only if you’re using the default server TLS secret command-line argument. If you’re not, feel free to skip this step.By default, the server returns a 404 Not Found page for all requests when no ingress rules are set up. Although we provide a self-signed certificate and key for testing purposes, we recommend using your own certificate.

 ```
kubectl apply -f examples/shared-examples/default-server-secret/default-server-secret.yaml
```
Create a ConfigMap to customize your NGINX settings:

 ```
kubectl apply -f deployments/common/nginx-config.yaml
```
Create an IngressClass resource. NGINX Ingress Controller won’t start without an IngressClass resource.

 ```
kubectl apply -f deployments/common/ingress-class.yaml
```
If you want to make this NGINX Ingress Controller instance your cluster’s default, uncomment the ingressclass.kubernetes.io/is-default-class annotation. This action will auto-assign IngressClass to new ingresses that don’t specify an ingressClassName.

## Create custom resources defination (CRDs)

To make sure your NGINX Ingress Controller pods reach the Ready state, you’ll need to create custom resource definitions (CRDs) for various components.

Alternatively, you can disable this requirement by setting the -enable-custom-resources command-line argument to false.

There are two ways you can install the custom resource definitions:

- Using a URL to apply a single CRD yaml file, which we recommend.
- Applying your local copy of the CRD yaml files, which requires you to clone the repository.


## INSTALL CRDS FROM SINGLE YAML
This single YAML file creates CRDs for the following resources:
- VirtualServer and VirtualServerRoute
- TransportServer
- Policy
- GlobalConfiguration

```
kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds.yaml

```
or 

## INSTALL CRDS AFTER CLONING THE REPO

If you are installing the CRDs this way, ensure you have first cloned the repository.

These YAML files create CRDs for the following resources:

- VirtualServer and VirtualServerRoute
- TransportServer
- Policy
- GlobalConfiguration
```
kubectl apply -f config/crd/bases/k8s.nginx.org_virtualservers.yaml
kubectl apply -f config/crd/bases/k8s.nginx.org_virtualserverroutes.yaml
kubectl apply -f config/crd/bases/k8s.nginx.org_transportservers.yaml
kubectl apply -f config/crd/bases/k8s.nginx.org_policies.yaml
kubectl apply -f config/crd/bases/k8s.nginx.org_globalconfigurations.yaml

```
 
## Deploy NGINX Ingress Controller

You have two options for deploying NGINX Ingress Controller:

<b> Deployment:</b> Choose this method for the flexibility to dynamically change the number of NGINX Ingress Controller replicas.
<br>

<b> DaemonSet:</b> Choose this method if you want NGINX Ingress Controller to run on all nodes or a subset of nodes.
Before you start, update the command-line arguments for the NGINX Ingress Controller container in the relevant manifest file to meet your specific requirements.

## Using a Deployment

When you deploy NGINX Ingress Controller as a Deployment, Kubernetes automatically sets up a single NGINX Ingress Controller pod.


 ```
kubectl apply -f deployments/deployment/nginx-ingress.yaml
```

## Using a DaemonSet

When you deploy NGINX Ingress Controller as a DaemonSet, Kubernetes creates an Ingress Controller pod on every node in the cluster.



 ```
kubectl apply -f deployments/daemon-set/nginx-ingress.yaml

```

## Confirm NGINX Ingress Controller is running

To confirm the NGINX Ingress Controller pods are operational, run:
```
 
kubectl get pods --namespace=nginx-ingress
```
## How to access NGINX Ingress Controller

Connect to ports 80 and 443 using the IP address of any node in the cluster where NGINX Ingress Controller is running.

## Uninstall NGINX Ingress Controller

Delete the nginx-ingress namespace:

 To remove NGINX Ingress Controller and all auxiliary resources, run:

 ```
kubectl delete namespace nginx-ingress
```
 Remove the cluster role and cluster role binding:

```
kubectl delete clusterrole nginx-ingress
kubectl delete clusterrolebinding nginx-ingress
```
Delete the Custom Resource Definitions(CRDs):
```
 kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds-nap-waf.yaml

 ```
