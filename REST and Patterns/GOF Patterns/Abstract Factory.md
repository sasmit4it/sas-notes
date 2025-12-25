# General Idea

- These are also called factory of factory
    
- Abstract factories create concrete factories , which in turn create actual products.
    
- two hierarchies are present, one for factories and one for products.
    
- used to create related families/groups of products.
    
- Client of factory refers to concrete products using abstract reference, so client is decoupled from actual impl.
    
- Each concrete factory generates products of one family, so this pattern can be used when it is important to use products of same family together e..g mac UI and windows UI
    
- Since the client depends on abstraction, rather than implementations, it is easy to switch the implementation families.
    

# Con

Abstract factories generally describe the products in the top level interface itself. So if we want to add a product to it, we will have to modify the entire hierarchy.

# Diagram
![[Pasted image 20251224233219.png]]
In above figure, Widgetfactory is an abstract factory. Motif factory and PM widget factory are concrete factories. On the right side we have a similar product hierarchy for window and scroll bar.

```
public interface Button {
    void paint();
}

public class MacOSButton implements Button {

    @Override
    public void paint() {
        System.out.println("You have created MacOSButton.");
    }
}
===========================================================================
public interface Checkbox {
    void paint();
}

public class MacOSCheckbox implements Checkbox {

    @Override
    public void paint() {
        System.out.println("You have created MacOSCheckbox.");
    }
}
================================================================================

/**
 * Abstract factory knows about all (abstract) product types.
 */
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

================================================================================

/**
 * Each concrete factory extends basic factory and responsible for creating
 * products of a single variety.
 */
public class MacOSFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new MacOSButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacOSCheckbox();
    }
}

=====================================================================================
public class WindowsFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}
==================================================================================
//client using the abstract factory.

public class Application {
    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void paint() {
        button.paint();
        checkbox.paint();
    }
}
```

In above code snippet, there are two products to be created, Button and scroll bar.

So for each product there is a separate create operation defined in the abstract factory i.e. `GUIFactory` .

Both `MacOSFactory` and `WindowsFactory` are concrete factories for respective types. Although they override the operations from abstract factory, they refer products using abstract term i.e. Button and Scroll Bar, and not their specific type.

notice how the client is using the factory. It takes the factory as a parameter in constructor. So this basically achieves DI/IOC. Factory can be swapped without changing anything in the client, if we want to use a different set of products.

Without the factory , client will have to hard code the dependencies. By using abstract factory we are avoiding that.