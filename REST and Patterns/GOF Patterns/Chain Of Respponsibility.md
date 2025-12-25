The idea of this pattern is to decouple senders and receivers by giving multiple objects a chance to handle a request. The request gets passed along a chain of objects until one of them handles it.

To forward the request along the chain, and to ensure receivers remain implicit, each object on the chain shares a common interface for handling requests and for accessing its successor on the chain.

Use Chain of Responsibility when more than one object may handle a request, and the handler isn't known a  
prior, or you want to issue a request to one of several objects without specifying the receiver explicitly.

The pattern frees an object from knowing which other object handles a request. An object only has to know that a request will be handled "appropriately." Both the receiver and the sender have no explicit knowledge of each other.

Since a request has no explicit receiver, there's no guarantee it'll be handled—the request can fall off the end of the chain without ever being handled.

![](attachments/Pasted%20image%2020251225190225.png)

The **Handler** declares the interface, common for all concrete handlers. It usually contains just a single method for handling requests, but sometimes it may also have another method for setting the next handler on the chain.

**Concrete Handlers** contain the actual code for processing requests. Upon receiving a request, each handler must decide whether to process it and, additionally, whether to pass it along t/base he chain.

```
//base handler
abstract class Processor
{
    private Processor nextProcessor;
 
    public Processor(Processor nextProcessor){
        this.nextProcessor = nextProcessor;
    };
     
    public void process(int request){
        if(nextProcessor != null)
            nextProcessor.process(request);
    };
}
===========================================================
class NegativeProcessor extends Processor
{
    public NegativeProcessor(Processor nextProcessor){
        super(nextProcessor);
         
    }
 
    public void process(int request)
    {
        if (request < 0)
        {
            System.out.println("NegativeProcessor : " + request);
        }
        else
        {
            super.process(request);
        }
    }
}
=============================================================
class ZeroProcessor extends Processor
{
    public ZeroProcessor(Processor nextProcessor){
        super(nextProcessor);
    }
 
    public void process(int request)
    {
        if (request == 0)
        {
            System.out.println("ZeroProcessor : " + request);
        }
        else
        {
            super.process(request);
        }
    }
}
==========================================================================
class PositiveProcessor extends Processor
{
 
    public PositiveProcessor(Processor nextProcessor){
        super(nextProcessor);
    }
 
    public void process(int request)
    {
        if (request > 0)
        {
            System.out.println("PositiveProcessor : " + request);
        }
        else
        {
            super.process(request);
        }
    }
}
=========================================================
//client
class TestChain
{
    public static void main(String[] args) {
       Processor p = new NegativeProcessor(new ZeroProcessor(new PositiveProcessor(null)));
         
        //Calling chain of responsibility
        p.process(10);
        p.process(0);
        p.process(-10);
      }
}

```