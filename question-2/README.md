# Question-2

## Installation

### kOps

kOps started guide details [reference](https://kops.sigs.k8s.io/getting_started/aws/)

kOps AWS IRSA usage [reference](https://dev.to/olemarkus/irsa-support-for-kops-1doe)

1. set up domain name on route53
2. set up domain TLS certificate on [ACM](https://aws.amazon.com/certificate-manager/)
3. create kubernetes state s3 bucket
4. create IRSA s3 bucket
5. deploy cluster

```
$ export NAME=rico.ninja
$ export KOPS_STATE_STORE=s3://monitor-cluster-s3-bucket
$ kops replace -f kops.yaml
$ kops update cluster --name ${NAME} --yes --admin
```

### Helm chart

```
$ kubectl create namespace monitor
$ kubectl create namespace ingress-nginx
```

```
$ helm upgrade --install ingress-nginx --version 4.0.1 ingress-nginx/ingress-nginx -f nginx-values.yaml -n ingress-nginx
$ helm upgrade --install external-dns --version 5.4.0 bitnami/external-dns -f external-dns-values.yaml -n kube-system
$ helm upgrade --install monitor --version 18.0.5 prometheus-community/kube-prometheus-stack -f prometheus-operator-values.yaml -n monitor
```

## Image

kOps default images are using official Ubuntu 20.04 images. [ref](https://github.com/kubernetes/kops/blob/master/docs/operations/images.md)
