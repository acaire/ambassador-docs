---
title: "It's a layer 7 world: Why does an edge proxy matter?"
---
# It's a layer 7 world: Why does an edge proxy matter for developers?

Intra-microservice communication happens over [layer 7](/learn/kubernetes-glossary/layer-7/) (L7) protocols. And because a developer wants to create _highly available and scalable_ distributed applications, it's important to be aware of the assumptions, and fallacies, that underpin distributed computing. In standard programming, maybe a developer does not need to care about network reliability or security, bandwidth, latency or transport costs (just as a start). Yet, all of these factors come into play with a distributed system, meaning a developer needs to understand traffic management to some degree and potentially code with these considerations in mind. 

[Cloud-native](/learn/kubernetes-glossary/load-balancer/) organizations have moved away from hardware-based load balancers for traffic management toward a software-based programmable edge, and edge proxies, which should be easier for developers to understand on a theoretical level, even when they may not be expected to manage traffic hands-on. 

## Smart L7 traffic management

Managing L7 traffic is critical to modern cloud-native applications. L7 traffic management is an approach to making sure that the challenges of distributed apps don't become roadblocks. In the context of modern application development, layers 4 and 7 (L4, L7) are frequently mentioned as relevant to load balancing, but L7 is the layer closest to the end user, helping to distribute traffic. L7 is at the very edge of the network where users interact directly with the application using protocols like HTTPS and gRPC. 

Smart L7 traffic management includes resilience patterns, such as [rate limiting](/docs/edge-stack/latest/topics/running/services/rate-limit-service/), timeouts, deadlines, circuit breakers, load balancing and more. These patterns are neither the typical domain for developers, nor are they simple to implement. Service/edge proxies, such as [Envoy Proxy](/resources/getting-started-envoyproxy-microservices-resilience/), are designed to implement smart L7 traffic management to take away some of the complications of resilience management, but when though solutions like Envoy do much of the heavy lifting, they are sophisticated enough that they pose some management challenges themselves (which is why developers and operators use a control plane for it).

With software now developed in an L7-driven world, developers need at least to understand, if not configure, L7 load balancing. With the range of protocols at work, such as [TLS](/docs/edge-stack/latest/topics/running/tls/), physical HTTP/1 or HTTP/2 as well as logical HTTP (headers, body data, etc.) and [gRPC](/docs/edge-stack/latest/howtos/grpc/) and REST messaging protocols, among others, load balancing is growing more complicated, and L7 proxies have evolved to manage this complexity. It becomes clearer why Envoy makes sense to simplify some of the complexity.

Developers should understand this landscape in order to see how their applications are running throughout the life cycle. This, in part, explains why more sophisticated solutions for managing L7 communications appear all the time. For example, using tooling like Ambassador, which acts as an edge-focused control plane for the Envoy Proxy, along with new workflow approaches like [GitOps](/docs/argo/latest/concepts/gitops/), the developer does not have to worry about functionality (ops function) that isn't germane to the developer experience. 
