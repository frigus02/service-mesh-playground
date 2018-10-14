# Service Mesh Playground

> A place to play with service meshes like [Istio](https://istio.io) and [Linkerd](https://linkerd.io)

## Prerequisites

1. Install Docker (>= 18.06)
2. Optionally increase resource limits
3. Enable Kubernetes

## Istio

_This was tested with Istio 1.0.2._

1. [Download Istio](https://istio.io/docs/setup/kubernetes/download-release/)

1. Install Istio

   ```sh
   kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
   kubectl apply -f install/kubernetes/istio-demo.yaml
   #kubectl apply -f install/kubernetes/istio-demo-auth.yaml
   ```

1. Install Bookinfo sample applicaton

   ```sh
   kubectl label namespace default istio-injection=enabled
   kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
   kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
   kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
   #kubectl apply -f samples/bookinfo/networking/destination-rule-all-mtls.yaml
   ```

1. Cleanup

   ```sh
   samples/bookinfo/platform/kube/cleanup.sh
   kubectl delete -f install/kubernetes/istio-demo.yaml
   #kubectl delete -f install/kubernetes/istio-demo-auth.yaml
   kubectl delete -f install/kubernetes/helm/istio/templates/crds.yaml -n istio-system
   ```
