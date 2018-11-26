# What is a service mesh?

## Definitions from the internet

1. [Istio](https://istio.io/docs/concepts/what-is-istio/#what-is-a-service-mesh)

   > The term service mesh is used to describe the network of microservices that make up such applications and the interactions between them. As a service mesh grows in size and complexity, it can become harder to understand and manage. Its requirements can include discovery, load balancing, failure recovery, metrics, and monitoring.

1. [Buoyant (company behind Linkerd)](https://blog.buoyant.io/2017/04/25/whats-a-service-mesh-and-why-do-i-need-one/)

   > A service mesh is a dedicated infrastructure layer for handling service-to-service communication.

   > The service mesh is a networking model that sits at a layer of abstraction above TCP/IP. It assumes that the underlying L3/L4 network is present and capable of delivering bytes from point to point. (It also assumes that this network, as with every other aspect of the environment, is unreliable; the service mesh must therefore also be capable of handling network failures.)

1. [NGINX](https://www.nginx.com/blog/what-is-a-service-mesh/)

   > A service mesh is a configurable infrastructure layer for a microservices application. It makes communication between service instances flexible, reliable, and fast. The mesh provides service discovery, load balancing, encryption, authentication and authorization, support for the circuit breaker pattern, and other capabilities.

## My slides

Slides showing transformation from traditional direct service to service communication to adding proxies and making it a service mesh: <https://docs.google.com/presentation/d/12cWxrlMfePNx2WN3JP0CmsAsYmPPt7hib2WHUQQbTcM/edit?usp=sharing>.

## Istio architecture

![Istio Architecture](https://istio.io/docs/concepts/what-is-istio/arch.svg)

## Istio features

- Traffic management

  - Define traffic routing between services
  - Retries
  - Fault detection and injection
  - Circuit breaker
  - Traffic mirroring
  - Can for example enable canary deployments

- Security

  - Authentication for requests coming into the mesh (JWT)
  - Authentication between services in the mesh (mTLS)
  - Authorization based on service or user properties
  - Block egress traffic by default

- Observability

  - Telemetry (Prometheus)
  - Tracing (Jaeger)
  - Service graphs
