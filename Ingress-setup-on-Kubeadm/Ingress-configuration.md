## Installation of Nginx Ingress Controller

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


Download the image using your NGINX Ingress Controller subscription certificate and key. See the Getting the F5 Registry NGINX Ingress Controller Image guide.
Use your NGINX Ingress Controller subscription JWT token to get the image: Instructions are in Getting the NGINX Ingress Controller Image with JWT.
Build your own image: To build your own image, follow the Building NGINX Ingress Controller guide.

Clone the repository
Clone the NGINX Ingress Controller repository using the command shown below, and replace <version_number> with the specific release you want to use.

 Copy
git clone https://github.com/nginxinc/kubernetes-ingress.git --branch <version_number>
For example, if you want to use version 3.5.1, the command would be git clone https://github.com/nginxinc/kubernetes-ingress.git --branch v3.5.1.

This guide assumes you are using the latest release.

Set up role-based access control (RBAC)
Admin access required
To complete these steps you need admin access to your cluster. Refer to to your Kubernetes platform’s documentation to set up admin access. For Google Kubernetes Engine (GKE), you can refer to their Role-Based Access Control guide.
Create a namespace and a service account:

 Copy
kubectl apply -f deployments/common/ns-and-sa.yaml
Create a cluster role and binding for the service account:

 Copy
kubectl apply -f deployments/rbac/rbac.yaml

If you’re planning to use NGINX App Protect or NGINX App Protect DoS, additional roles and bindings are needed.

(NGINX App Protect only) Create the App Protect role and binding:

 Copy
kubectl apply -f deployments/rbac/ap-rbac.yaml
(NGINX App Protect DoS only) Create the App Protect DoS role and binding:

 Copy
kubectl apply -f deployments/rbac/apdos-rbac.yaml
Create common resources
In this section, you’ll create resources that most NGINX Ingress Controller installations require:

(Optional) Create a secret for the default NGINX server’s TLS certificate and key. Complete this step only if you’re using the default server TLS secret command-line argument. If you’re not, feel free to skip this step.

By default, the server returns a 404 Not Found page for all requests when no ingress rules are set up. Although we provide a self-signed certificate and key for testing purposes, we recommend using your own certificate.

 Copy
kubectl apply -f examples/shared-examples/default-server-secret/default-server-secret.yaml
Create a ConfigMap to customize your NGINX settings:

 Copy
kubectl apply -f deployments/common/nginx-config.yaml
Create an IngressClass resource. NGINX Ingress Controller won’t start without an IngressClass resource.

 Copy
kubectl apply -f deployments/common/ingress-class.yaml
If you want to make this NGINX Ingress Controller instance your cluster’s default, uncomment the ingressclass.kubernetes.io/is-default-class annotation. This action will auto-assign IngressClass to new ingresses that don’t specify an ingressClassName.

Create custom resources
To make sure your NGINX Ingress Controller pods reach the Ready state, you’ll need to create custom resource definitions (CRDs) for various components.

Alternatively, you can disable this requirement by setting the -enable-custom-resources command-line argument to false.

There are two ways you can install the custom resource definitions:

Using a URL to apply a single CRD yaml file, which we recommend.
Applying your local copy of the CRD yaml files, which requires you to clone the repository.
INSTALL CRDS FROM SINGLE YAML
INSTALL CRDS AFTER CLONING THE REPO
This single YAML file creates CRDs for the following resources:

VirtualServer and VirtualServerRoute
TransportServer
Policy
GlobalConfiguration
 Copy
kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds.yaml
INSTALL CRDS FROM SINGLE YAML
INSTALL CRDS AFTER CLONING THE REPO
Core custom resource definitions
Create CRDs for VirtualServer and VirtualServerRoute, TransportServer, Policy and GlobalConfiguration:

 Copy
kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds.yaml
Optional custom resource definitions
For the NGINX App Protect WAF module, create CRDs for APPolicy, APLogConf and APUserSig:

 Copy
kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds-nap-waf.yaml
For the NGINX App Protect DoS module, create CRDs for APDosPolicy, APDosLogConf and DosProtectedResource:

 Copy
kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds-nap-dos.yaml
Deploy NGINX Ingress Controller
You have two options for deploying NGINX Ingress Controller:

Deployment. Choose this method for the flexibility to dynamically change the number of NGINX Ingress Controller replicas.
DaemonSet. Choose this method if you want NGINX Ingress Controller to run on all nodes or a subset of nodes.
Before you start, update the command-line arguments for the NGINX Ingress Controller container in the relevant manifest file to meet your specific requirements.

Using a Deployment
For additional context on managing containers using Kubernetes Deployments, refer to the official Kubernetes Deployments documentation.

When you deploy NGINX Ingress Controller as a Deployment, Kubernetes automatically sets up a single NGINX Ingress Controller pod.

For NGINX, run:

 Copy
kubectl apply -f deployments/deployment/nginx-ingress.yaml
For NGINX Plus, run:

 Copy
kubectl apply -f deployments/deployment/nginx-plus-ingress.yaml
Update the nginx-plus-ingress.yaml file to include your chosen image from the F5 Container registry or your custom container image.

Using a DaemonSet
For additional context on managing containers using Kubernetes DaemonSets, refer to the official Kubernetes DaemonSets documentation.

When you deploy NGINX Ingress Controller as a DaemonSet, Kubernetes creates an Ingress Controller pod on every node in the cluster.

For NGINX, run:

 Copy
kubectl apply -f deployments/daemon-set/nginx-ingress.yaml
For NGINX Plus, run:

 Copy
kubectl apply -f deployments/daemon-set/nginx-plus-ingress.yaml
Update the nginx-plus-ingress.yaml file to include your chosen image from the F5 Container registry or your custom container image.

Confirm NGINX Ingress Controller is running
To confirm the NGINX Ingress Controller pods are operational, run:

 Copy
kubectl get pods --namespace=nginx-ingress
How to access NGINX Ingress Controller
Using a Deployment
For Deployments, you have two options for accessing NGINX Ingress Controller pods.

Option 1: Create a NodePort service
For more information about the NodePort service, refer to the Kubernetes documentation.

To create a service of type NodePort, run:

 Copy
kubectl create -f deployments/service/nodeport.yaml
Kubernetes automatically allocates two ports on every node in the cluster. You can access NGINX Ingress Controller by combining any node’s IP address with these ports.

Option 2: Create a LoadBalancer service
For more information about the LoadBalancer service, refer to the Kubernetes documentation.

To set up a LoadBalancer service, run one of the following commands based on your cloud provider:

GCP or Azure:

 Copy
kubectl apply -f deployments/service/loadbalancer.yaml
AWS:

 Copy
kubectl apply -f deployments/service/loadbalancer-aws-elb.yaml
If you’re using AWS, Kubernetes will set up a Classic Load Balancer (ELB) in TCP mode. This load balancer will have the PROXY protocol enabled to pass along the client’s IP address and port.

AWS users: Follow these additional steps to work with ELB in TCP mode.

Add the following keys to the nginx-config.yaml ConfigMap file, which you created in the Create common resources section.

 Copy
proxy-protocol: "True"
real-ip-header: "proxy_protocol"
set-real-ip-from: "0.0.0.0/0"
Update the ConfigMap:

 Copy
kubectl apply -f deployments/common/nginx-config.yaml
Note:
AWS users have more customization options for their load balancers. These include choosing the load balancer type and configuring SSL termination. Refer to the Kubernetes documentation to learn more.
To access NGINX Ingress Controller, get the public IP of your load balancer.

For GCP or Azure, run:

 Copy
kubectl get svc nginx-ingress --namespace=nginx-ingress
For AWS find the DNS name:

 Copy
kubectl describe svc nginx-ingress --namespace=nginx-ingress
Resolve the DNS name into an IP address using nslookup:

 Copy
nslookup <dns-name>
You can also find more details about the public IP in the status section of an ingress resource. For more details, refer to the Reporting Resources Status doc.

Using a DaemonSet
Connect to ports 80 and 443 using the IP address of any node in the cluster where NGINX Ingress Controller is running.

Uninstall NGINX Ingress Controller
Warning:
Proceed with caution when performing these steps, as they will remove NGINX Ingress Controller and all related resources, potentially affecting your running services.
Delete the nginx-ingress namespace: To remove NGINX Ingress Controller and all auxiliary resources, run:

 Copy
kubectl delete namespace nginx-ingress
Remove the cluster role and cluster role binding:

 Copy
kubectl delete clusterrole nginx-ingress
kubectl delete clusterrolebinding nginx-ingress
Delete the Custom Resource Definitions:

DELETING CRDS FROM SINGLE YAML
DELETING CRDS AFTER CLONING THE REPO
Delete core custom resource definitions: shell kubectl delete -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds.yaml
Delete custom resource definitions for the NGINX App Protect WAF module:
 Copy
 kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds-nap-waf.yaml
 ```

3. Delete custom resource definitions for the NGINX App Protect DoS module:
```shell
 kubectl apply -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/v3.5.1/deploy/crds-nap-dos.yaml
 ```