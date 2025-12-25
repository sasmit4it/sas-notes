In essence, micoservice is just another layer of modular decomposition, which we have been using in OOP principles, like information hiding, cohesion etc. Following are the points which govern a good boundary.

**Information Hiding**

Consider microservice as a single encapsulated unit which which interacts with other such units to get your business work as expected.

So once you write a service behind a strong interface/contract, you achieve all the relevant benefits.

- Flexibility: You are able to make changes within the service, within touching other parts, and without breaking anything unexpectedly.
    
- Comprehensive code base: Since service is a unit itself, it can be isolated and understood completely in isolation
    

**Cohesion**

Boundary should let code with silimar purpose sit together, and let others sit in different section. This way, if anything needs to be updated in current set of functionality, it can be done local to one current service, without disturbing anything else.

**Coupling**

Coupling basically means , how much services know about eachother, and how much they interact to perform the task. More the coupling, more the services depend on each other, and more they are difficult to change. Even though, some coupling is inevitable, we should strive to reduce it.

Following are various types of couplings.

- Domain coupling
    

This is type oc coupling where, service interaction is mandated by the domain requirement. this form of coupling is inevitable, and is considered loose coupling. However, a service depending on multiple downstream service can be a cause of concern.

- Temporal coupling
    

A time bound coupling between two services is called temporal coupling. Good article [here](https://betterprogramming.pub/temporal-coupling-in-code-e74899f7a48f "https://betterprogramming.pub/temporal-coupling-in-code-e74899f7a48f"). Follwing is an example of the same

![[Pasted image 20251225000635.png]]

here, main functionality is to complete the purchase. So function calculates the total, and calls the CC gateway to do actual payment. Once, payment is done, core work of this function is done, but as you can see here, other dependant tasks on the purchase activity , like generate coupon code or lottery ticket, are also linked here. Because of this style, purchase has to wait for other non relevant tasks to finish. This degrades performance of main task.

This can be solved by even driven design as follows.

![[Pasted image 20251225000653.png]]

once main event is completed, function emits an event which can be subscribed by other interested parties, and use as per their need.

- Common coupling
    

Here, two services share a common global resource. They are aware that the resource is shared. Since it is a shared resource, servieces can overload it with lot of traffice. Also data can be left in inconsistent state by one service, causing other service to fail completely. To mitigata this, both services have to maintain duplicate validation logic.


![](attachments/Pasted%20image%2020251225185633.png)



![](attachments/Pasted%20image%2020251225185640.png)


Order table is shared, and each order goes through the state machine described above. It can happen that , orderprocessor changes order state to such a state that, it breaks the warehouse service ,or vice versa.

This can be mitigated by using a wrapper service around the share table as follows.

![](attachments/Pasted%20image%2020251225185648.png)

Now both processor and warehouse services ahve to go through order service. JOb of the order service is to maintain the state machine sequence for each order. if any request comes from either of client services, which violates the state, it will simply reject the request.

State maintanance logic is now stored at sigle place, and client servies are not bothered about it.

**Content coupling**

This is same as common coupling, but only difference is resource is shared unintentionally. To fix this all you have to do is make each call to shared resource go through the service interface.

![](attachments/Pasted%20image%2020251225185656.png)

**Pass through coupling**

here 3 or more services are involved. First service produces and hands over the data to second service. Second service just passes that data to third service which is actual consumer of that data.


![](attachments/Pasted%20image%2020251225185704.png)

This is dangerous, because any change in data consumption logic will affect all three services.

there are two ways to mitigate this.

1. Make 1st service talk to 3rd service directly.
    
2. Make 2nd service opaque about the data. 2nd service just treats the data as BLOB, and hands it over. Even if there is any change in data format, it is not bothered about it.