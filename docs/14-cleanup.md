# Cleaning Up

In this lab you will delete the compute resources created during this tutorial.

## Compute Instances

Delete the controller and worker compute instances:

```
gcloud -q compute instances delete \
  controller-0 controller-1 controller-2 \
  worker-0 worker-1 worker-2 \
  --zone $(gcloud config get-value compute/zone)
```

## Networking

Delete the external load balancer network resources:

```
{
  gcloud -q compute forwarding-rules delete kubernetes-forwarding-rule \
    --region $(gcloud config get-value compute/region)

  gcloud -q compute target-pools delete kubernetes-target-pool

  gcloud -q compute http-health-checks delete kubernetes

  gcloud -q compute addresses delete k8s-the-hard-way
}
```

Delete the `k8s-the-hard-way` firewall rules:

```
gcloud -q compute firewall-rules delete \
  allow-ssh-ingress-from-iap \
  k8s-allow-nginx-service \
  k8s-allow-internal \
  k8s-allow-external \
  k8s-allow-lb-health-check
```

Delete the Cloud NAT

```
gcloud -q compute routers delete nat-router
```


Delete the `k8s-the-hard-way` network VPC:

```
{
  gcloud -q compute routes delete \
    k8s-route-10-200-0-0-24 \
    k8s-route-10-200-1-0-24 \
    k8s-route-10-200-2-0-24

  gcloud -q compute networks subnets delete cka

  gcloud -q compute networks delete k8s-the-hard-way
}
```
