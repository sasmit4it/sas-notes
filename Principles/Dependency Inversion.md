**The principle of dependency inversion refers to the decoupling of software modules. This way, instead of high-level modules depending on low-level modules, both will depend on abstractions.**

"If the OCP states the goal of OO architecture, the DIP states the primary mechanism".

By not implementing DI, we loose our ability to

1. test the class with unit testing frameworks
    
2. Swap the dependent classes as per our need
    

Following is a bad DI example. Following class creates keyboard and mouse using new.
```
public class Windows98Machine {

    private final StandardKeyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine() {
        monitor = new Monitor();
        keyboard = new StandardKeyboard();
    }

}Copy
```

This code will work, and we'll be able to use the _StandardKeyboard_ and _Monitor_ freely within our _Windows98Computer_ class.

Problem solved? Not quite. **By declaring the** _**StandardKeyboard**_ **and** _**Monitor**_ **with the** _**new**_ **keyword, we've tightly coupled these three classes together.**

Not only does this make our _Windows98Computer_ hard to test, but we've also lost the ability to switch out our _StandardKeyboard_ class with a different one should the need arise. And we're stuck with our _Monitor_ class too.

Following is the solution to above problem

Let's decouple our machine from the _StandardKeyboard_ by adding a more general _Keyboard_ interface and using this in our class:

```
public interface Keyboard { }Copy
```

```
public class Windows98Machine{

    private final Keyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine(Keyboard keyboard, Monitor monitor) {
        this.keyboard = keyboard;
        this.monitor = monitor;
    }
}Copy
```

Here, we're using the dependency injection pattern to facilitate adding the _Keyboard_ dependency into the _Windows98Machine_ class.

Let's also modify our _StandardKeyboard_ class to implement the _Keyboard_ interface so that it's suitable for injecting into the _Windows98Machine_ class:

```
public class StandardKeyboard implements Keyboard { }Copy
```

Now our classes are decoupled and communicate through the _Keyboard_ abstraction. If we want, we can easily switch out the type of keyboard in our machine with a different implementation of the interface. We can follow the same principle for the _Monitor_ class.