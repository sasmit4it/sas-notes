Why a gateway is needed?

Haha Once a monolith is split into microservices, each microservice runs on it’s own instance and has into own IP / DNS name. Theoretically a web or mobile client can directly call these microservices as and when needed.

e.g. for a product details page on amazon we will need to call following services to render the page completely.

1. shopping cart service
    
2. shipping service
    
3. inventory service
    
4. recommendation service
    
5. order service
    
6. review service
    
7. product catalog service
    

A client can call each of these service by following the standard URL like:

[https://serviceName.api.company.name](https://servicename.api.company.name/ "https://servicename.api.company.name/")

However for one page, client will have to call 7 different services. As complexity increases number of calls will go upwards. This can be a problem especially on mobile network , where connectivity is erratic.

Also not all the services use optimized protocol for client communication e.g. a service only using messaging queues. Such service may be very efficient within your network, but very difficult to communicate with over HTTP/HTTPS.

One more point, since client will depend on your internal micro service architecture, you cannot freely modify it. You’ll have to think twice, when you see a need to split a service or merge two services.

So in summary you need a middle man to handle all these issues. So enters API gateway.

What is API gateway?

API gateway is synonymous to FACADE design pattern. It encapsulates all the services and exposes a client friendly API. Other responsibilities include

request routing

load balancing

protocol translation

caching

Does Techpulse use API gateway?

Not needed. TP has a network load balancer that receives incoming requests, and hands them over to a target group of ECS clusters based on rules. Then ECS cluster is responsible for spawning / killing docker containers routing requests to them based on URL etc.

## Related content