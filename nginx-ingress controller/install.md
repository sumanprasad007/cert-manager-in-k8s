# Quick start 

## Install using Helm chart:
If you have Helm, you can deploy the ingress controller with the following command:

```
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```
It will install the controller in the ingress-nginx namespace, creating that namespace if it doesn't already exist.

Info
This command is idempotent:

if the ingress controller is not installed, it will install it,
if the ingress controller is already installed, it will upgrade it.
If you want a full list of values that you can set, while installing with Helm, then run:

```
helm show values ingress-nginx --repo https://kubernetes.github.io/ingress-nginx
Helm install on AWS/GCP/Azure/Other providers

```

The ingress-nginx-controller helm-chart is a generic install out of the box. The default set of helm values is not configured for installation on any infra provider. The annotations that are applicable to the cloud provider must be customized by the users.
See AWS LB Controller.
Examples of some annotations recommended (healthecheck ones are required for target-type IP) for the service resource of --type LoadBalancer on AWS are below:
```

  annotations:
    service.beta.kubernetes.io/aws-load-balancer-target-group-attributes: deregistration_delay.timeout_seconds=270
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-path: /healthz
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-port: "10254"
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-protocol: http
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-success-codes: 200-299
    service.beta.kubernetes.io/aws-load-balancer-scheme: "internet-facing"
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: tcp
    service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: "true"
    service.beta.kubernetes.io/aws-load-balancer-type: nlb
    service.beta.kubernetes.io/aws-load-balancer-manage-backend-security-group-rules: "true"
    service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: "true"
    service.beta.kubernetes.io/aws-load-balancer-security-groups: "sg-something1 sg-something2"
    service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name: "somebucket"
    service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix: "ingress-nginx"
    service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval: "5"

```
If you don't have Helm or if you prefer to use a YAML manifest, you can run the following command instead:

## Install using Kubectl:
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.2/deploy/static/provider/cloud/deploy.yaml
```

Info: The YAML manifest in the command above was generated with helm template, so you will end up with almost the same resources as if you had used Helm to install the controller.

Attention
If you are running an old version of Kubernetes (1.18 or earlier), please read this paragraph for specific instructions. Because of api deprecations, the default manifest may not work on your cluster. Specific manifests for supported Kubernetes versions are available within a sub-folder of each provider.

Firewall configuration ¶

To check which ports are used by your installation of ingress-nginx, look at the output of 
```
kubectl -n ingress-nginx get pod -o yaml.

```
In general, you need:
Port 8443 open between all hosts on which the kubernetes nodes are running. This is used for the ingress-nginx admission controller.
Port 80 (for HTTP) and/or 443 (for HTTPS) open to the public on the kubernetes nodes to which the DNS of your apps are pointing.
Pre-flight check ¶

A few pods should start in the ingress-nginx namespace:
```
kubectl get pods --namespace=ingress-nginx
```
After a while, they should all be running. The following command will wait for the ingress controller pod to be up, running, and ready:

```
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s
```