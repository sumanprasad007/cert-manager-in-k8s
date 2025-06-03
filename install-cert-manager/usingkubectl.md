# kubectl apply

Learn how to install cert-manager using kubectl and static manifests.

## Prerequisites

Install kubectl version >= v1.19.0. (otherwise, you'll have issues updating the CRDs - see v0.16 upgrade notes)
Install a supported version of Kubernetes or OpenShift.
Read Compatibility with Kubernetes Platform Providers if you are using Kubernetes on a cloud platform.
Steps

## 1. Install from the cert-manager release manifest

All resources (the CustomResourceDefinitions and the cert-manager, cainjector and webhook components) are included in a single YAML manifest file:

Install all cert-manager components:

```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.17.2/cert-manager.yaml
```
By default, cert-manager will be installed into the cert-manager namespace. It is possible to run cert-manager in a different namespace, although you'll need to make modifications to the deployment manifests.

Once you've installed cert-manager, you can verify it is deployed correctly by checking the cert-manager namespace for running pods:
## 2. Validate the Cert-manager pods Status:

```
kubectl get pods --namespace cert-manager
```

```
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-5c6866597-zw7kh               1/1     Running   0          2m
cert-manager-cainjector-577f6d9fd7-tr77l   1/1     Running   0          2m
cert-manager-webhook-787858fcdb-nlzsq      1/1     Running   0          2m
You should see the cert-manager, cert-manager-cainjector, and cert-manager-webhook pods in a Running state. The webhook might take a little longer to successfully provision than the others.
```
