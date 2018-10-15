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

   open http://localhost/productpage
   ```

1. Open telemetry tools

    ```sh
    # Prometheus - https://istio.io/docs/tasks/telemetry/querying-metrics/
    kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &
    open http://localhost:9090/graph

    # Jaeger - https://istio.io/docs/tasks/telemetry/distributed-tracing/
    kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
    open http://localhost:16686

    # Grapaha - https://istio.io/docs/tasks/telemetry/using-istio-dashboard/
    kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
    open http://localhost:3000/dashboard/db/istio-mesh-dashboard
    open http://localhost:3000/dashboard/db/istio-service-dashboard
    open http://localhost:3000/dashboard/db/istio-workload-dashboard

    # Service Graph - https://istio.io/docs/tasks/telemetry/servicegraph/
    kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088 &
    open http://localhost:8088/force/forcegraph.html
    open http://localhost:8088/force/forcegraph.html?time_horizon=15s&filter_empty=true
    ```

1. Play around with routing

    ```sh
    # Route all traffic to v1 of all services
    kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

    # Traffic shifting: apply any of these configs and reload the product page
    #kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-*

    # Fault injection: apply any of these configs and reload the product page
    #kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-*
    ```

1. Cleanup

   ```sh
   samples/bookinfo/platform/kube/cleanup.sh
   kubectl delete -f install/kubernetes/istio-demo.yaml
   #kubectl delete -f install/kubernetes/istio-demo-auth.yaml
   kubectl delete -f install/kubernetes/helm/istio/templates/crds.yaml -n istio-system
   ```
