1. Timeout and retry
    

All the network calls should have predefined timeouts and retry count set. Once these are exausted service should give up retrying.

Timeout and retry values should be set sensibly. A temporary network outage should not be reported as a netwrok or service failure. But after retrying for a while service should give up.

2. BulkHeads
    

This is a shipping analogy, which basically means if water enters one part of the ship, crew should be able to isolate that part, and water should remain contained only in that part.

In microservices, this means one slow service should not affect other healthy services. Following is an example , where A is healthy service while B is slow.v

![](attachments/Pasted%20image%2020251225185740.png)


When there are multiple concurrent requests to Service A, say 10, 5 of them are for endpoint _**/a/b**_ and 5 of them are for endpoint **/a**, there is a chance that Service A might use all its threads to work on the requests for _**/a/b**_ and block all the 5 threads. Even though the remaining requests are for _**/a**_ which does not have any other service dependency, Service A does not have free threads to work on the requests _**(resource exhaustion)**_! This behavior will affect overall performance of the application and might cause poor user experience. Service B slowness indirectly affects Service A performance as well.

3. Circuit breakers
    

This pattern comes into the picture while **communicating between services**. Let’s take a simple scenario. Let’s say we have two services: Service A and B. Service A is calling Service B(API call) to get some information needed. When Service A is calling to Service B, if Service B is down due to some infrastructure outage, what will happen? **Service A is not getting a result and it will be hang by throwing an exception**. Then another request comes and it also faces the same situation. Like this request threads will be blocked/hanged until Service B is coming up! As a result, the network resources will be exhausted with low performance and bad user experience. **Cascading failures** also can happen due to this.

## How the pattern works? 💥

Basically, it will behave same as an electrical circuit breaker. When the application gets remote service call failures more than a given threshold circuit breaker trips for a particular time period. After this timeout expires, the circuit breaker allows a limited number of requests to go through it. If those requests are getting succeeded, then circuit breaker will be closed and normal operations are resumed. Otherwise, it they are failing, timeout period starts again and do the rest as previous.
```
resilience4j:
  circuitbreaker:
    instances:
      loan-service:
        registerHealthIndicator: true
        failureRateThreshold: 50
        minimumNumberOfCalls: 5
        automaticTransitionFromOpenToHalfOpenEnabled: true
        waitDurationInOpenState: 5s
        permittedNumberOfCallsInHalfOpenState: 3
        slidingWindowSize: 10
        slidingWindowType: COUNT_BASED
        
```
