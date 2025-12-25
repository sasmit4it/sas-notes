However simple an application can be, over the time , it grows into an unmanageable monolith. With each passing sprint, people keep adding features and bug fixes, which eventually makes the code base large enough.

pain points of a large code base

- system is so large that it cannot be completely understood by one developer. Due to this, adding new feature or fixing a bug becomes to and from, time consuming task.
    
- As code size increases, start up time increase too. If an application needs 20 min to start up, and as a developer if your typically deploying 4-5 times a day, your almost 2 hours are wasted just waiting for it to start up.
    
- cant use optimized hardware i.e. cant scale hardware as per need. For example if one part of monolith uses more memory, which other needs more processing power, there is no way to attain both in a single set of hardware.
    
- reliability. Since entire monolith runs in single process(same process on different instances in case of multiple nodes) one bug will bring entire system down
    
- Non adaptable. When you have multi-million lines of code running on one set of tech stack, the sheer size of code itself becomes a deterrent to move to a newer, more optimized tech stack.
    
- Changing a small portion of code leads to entire monoliths regression testing. There is no way to segregate one small part and modify/test it.
    

All above problems are typically solved by using microservices architecture.

**While breaking a monolith typical question is “how small should an individual service be?“**

**answer: The goal of microservices is to sufficiently decompose the application in order to**  
**facilitate agile application development and deployment**

Don't just go in an endless loop of further and further decomposing. Find a sensible grain level based on your business model.

Drawbacks of using microservices.

- since it is a distributed system , you will need to figure out inter process communication mechanism, fault handling etc. In monolith , this is provided to you by the language itself.
    
- DB transaction: since each service typically has it’s own DB, maintaining transaction is difficult. Each service might have a different type of DB. Generally microservices depend on eventual consistency model.
    
- Testing becomes difficult, since you’ll have to bring up all the necessary dependent services, to test any one service.
    
- deployment is complex too. it’s not just copying one jar file to prod like monolith