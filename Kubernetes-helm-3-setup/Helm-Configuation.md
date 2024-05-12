## Kubernetes cluster  helm 3 Configuration and setup steps.

## Overview
This guide provides detailed instructions for setting up a Kubernetes cluster using Kubeadm. The guide includes instructions for installing and configuring containerd and Kubernetes, disabling swap, initializing the cluster, installing Flannel, and joining nodes to the cluster.

## Installing Helm
This guide shows how to install the Helm CLI. Helm can be installed either from source, or from pre-built binary releases.

## From the Binary Releases
Every release  https://github.com/helm/helm/releases  of Helm provides binary releases for a variety of OSes. These binary versions can be manually downloaded and installed.

- Download your desired version from (https://github.com/helm/helm/releases)
- Unpack it (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
- Find the helm binary in the unpacked directory, and move it to its desired destination (mv linux-amd64/helm /usr/local/bin/helm)
  
From there, you should be able to run the client and add the stable repo:  helm help link  (https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository )

## Note:

Helm automated tests are performed for Linux AMD64 only during CircleCi builds and releases. Testing of other OSes are the responsibility of the community requesting Helm for the OS in question.

## From Script
Helm now has an installer script that will automatically grab the latest version of Helm and install it locally.

You can fetch that script, and then execute it locally. It's well documented so that you can read through it and understand what it is doing before you run it.

```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

## Through Package Managers
The Helm community provides the ability to install Helm through operating system package managers. These are not supported by the Helm project and are not considered trusted 3rd parties.

## From Homebrew (macOS)
Members of the Helm community have contributed a Helm formula build to Homebrew. This formula is generally up to date.
```
brew install helm
```
(Note: There is also a formula for emacs-helm, which is a different project.)

## From Chocolatey (Windows)
Members of the Helm community have contributed a Helm package build to Chocolatey. This package is generally up to date.
```
choco install kubernetes-helm
```
## From Scoop (Windows)
Members of the Helm community have contributed a Helm package build to Scoop. This package is generally up to date.
```
scoop install helm
```
## From Winget (Windows)
Members of the Helm community have contributed a Helm package build to Winget. This package is generally up to date.

```
winget install Helm.Helm
```
## From Apt (Debian/Ubuntu)

Members of the Helm community have contributed a Helm package for Apt. This package is generally up to date.

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

```
## More ways for downloading helm on different system follow 
Follow for more guidence
- https://helm.sh/docs/intro/install/

## Conclusion 

In most cases, installation is as simple as getting a pre-built helm binary. This document covers additional cases for those who want to do more sophisticated things with Helm.