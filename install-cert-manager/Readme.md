## Prerequisites

Install Helm version 3 or later.

```
https://helm.sh/docs/intro/install/
```
Install a supported version of Kubernetes or OpenShift.
```
https://cert-manager.io/docs/releases/
```
Read Compatibility with Kubernetes Platform Providers if you are using Kubernetes on a cloud platform.
Installing cert-manager

## 1. Add the Helm repository

This repository is the only supported source of cert-manager charts. There are some other mirrors and copies across the internet, but those are entirely unofficial and could present a security risk.

Notably, the "Helm stable repository" version of cert-manager is deprecated and should not be used.

```
helm repo add jetstack https://charts.jetstack.io --force-update
```
## 2. Install cert-manager

To install the cert-manager Helm chart, use the Helm install command as described below.

```
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.17.2 \
  --set crds.enabled=true
```

## 3. (optional) Verify installation

Once you have deployed cert-manager, you can verify the installation.

```
kubectl get cert-manager -A
```
