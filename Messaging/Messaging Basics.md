messaging includes 5 basic steps:
1. create
2. Send
3. Deliver
4. Receive
5. Process
![](attachments/Pasted%20image%2020260421235722.png)

These steps can repeat multiple times through intermediate hosts until final receiver host is reached.

Why do all this though? Why not just send a message directly from sender to receiver through SOAP/REST?
Below are some important benefits of a messaging implementation.
1. **Async**: Messaging is inherently async. Sender can send the message and forget about it
2. **Atomic**: A message is an encapsulated independent entity. It can be retried multiple times until it is safely received by destination, that too exactly once.
3. **Store and forward**: Messaging systems are able to store messages on disk or in memory, to be replayed later. This introduces a layer of durability, which is not there in SOAP/REST call
4. **Variable timing/Throttling**: Producer can produce messages at it's own pace, and consumer can consume at it's own. System is designed to work even if receiver is not available.