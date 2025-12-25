1. classic load balancers
    

old generation load balancers. Being phased out

1. Application LBs
    

An Application Load Balancer makes routing decisions at the application layer (HTTP/HTTPS), supports path-based routing, and can route requests to one or more ports on each container instance in your cluster. Application Load Balancers support dynamic host port mapping

1. Network LBs
    

A Network Load Balancer makes routing decisions at the transport layer (TCP/SSL). It can handle millions of requests per second. After the load balancer receives a connection, it selects a target from the target group for the default rule using a flow hash routing algorithm.

### Classic LB

Supports TCP, SSL and HTTP/S.

Can load balance traffic in same region only.

Can load balance between instances running in different availability zone, but within same region.

Security group of LB should be different from security group of EC2 instances. That is a best practice.

_Classic LB does not support multiple target groups._

#### connection draining

When LB finds an instance to be unhealthy, it goes on to remove it from LB instance group. But the instance may be handling a request, and we don’t want that request to fail abruptly. SO we set connection draining setting in seconds, which basically tells LB to wait that much before removing the instance.

#### Internal Classic LB

This setting tells if LB is internet facing or not.

When you create a load balancer in a VPC, you must choose whether to make it an internal load balancer or an internet-facing load balancer. The nodes of an internet-facing load balancer have public IP addresses. The DNS name of an internet-facing load balancer is publicly resolvable to the public IP addresses of the nodes.

The nodes of an internal load balancer have only private IP addresses

### Application LB

Most frequently used.

Managed Elastic LB.

Supports webSockets and HTTP/S.

Scales automatically based on demand.(auto scaling)

Note that only Elastic load balancer(ELB) is managed and scales. Relevant EC2 instances need to be managed by you.

Can load balance traffic in same region only.

_ALB supports multiple target groups._

Application LB can load balance between EC2, ECS, Web apps(using ip), lambdas etc.

Application LB will require a target group configured. This target group will have list of EC2 instances, ECS nodes, or application ip's configured.

As per best practice, EC2 instances should be only accessible via LB. For that, we need to assign LB’s security group to each instances security group’s inbound rule as follows.

![[Pasted image 20251225001245.png]]

Above rules are from Ec2 instances security group. 

What this does is , it allows HTTP traffic from only the associated ALB and blocks all other HTTP traffic to that EC2 instance.

_This is why you should create different security groups for LBs and instances.






### Listeners

Listeners associate protocol:port combinations to set of targets.This is a ALB listeners configuration. Here HTTP:80 is routed to EC2 target group, and HTTP:8080 is configured to return fixed html response.
![[Pasted image 20251225001608.png]]
Listeners can be configured on live ALBs.

Security groups can be edited on live ALBs


### Target Groups

Set of entities to which traffic is directed to. Target groups contain

1. Ec2 instances : within one vpc instances only
    
2. ip addresses : ip’s from vpc and on premised instances.
    
3. lambda functions. : single lambda function accepted
    
4. other ALBs
    

#### deregistration delay

number of seconds to wait for , so that target completes processing of already in progress requests , before the target is de-registered from target group.(0 to 3600 seconds, defaults to 300 sec.)

#### slow start duration

Gradually increasing request load is routed to newly registered target.(30 to 900 sec. 0 to disable)

#### stickyness

Request from one user/session goes to a specific target.

implemented using cookies.

### ALB routing rules

one ALB can route traffic to multiple target groups. All you have to do is specify routing rules in listener config of the ALB.



![[Pasted image 20251225001627.png]]



Above image shows listener rules for ALB on HTTP:80 combination. You can add different rules for protocol: port combination. In above rules all requests with path /a/* are routed to microservice target group. Other requests are going to default “my-target-group“.

other than path you can set rules based on

1. host
    
2. http header
    
3. query parameters
    
4. ip address
    

### Network load balancers

classic LB operates at application level (level 7) and transport level(level 4)

Application LB can operate only at application level (level 7) .

Network LB operates at transport level(level 4).

Network LB is primarily used in high performance use cases. i.e. millions of requests per second.

Network LB can have static/elastic ip.

Network LB supports

- TCP
    
- TLS
    
- UDP
    
- TCP_UDP
    

Target groups can be of instances, ecs or on prem ips. No lambdas supported here



	

