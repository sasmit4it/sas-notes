
Add a reaction

## Before DIP

Traditionally, applications are divided into two types of modules

1. Policy defining module or High level modules
    
2. Service modules or low level modules
    

Basically high level modules define **what to do,** while low level modules define **how to do.** e.g. Update employee salary data is a high level task, and update employee salary table is a low level task.

So before DIP, high level modules consume services of low level modules. Generally control flow goes from top to bottom as described in image below.

![[Pasted image 20251225172131.png]]

Now let’s see some issues with this approach.

- Code reusability is hampered
    

When high level modules use low level modules directly i.e. using new keyword these modules are tightly coupled, and cannot be reused, without carrying technical debt with them. e.g. you want to use a prewritten piece of code, which does exactly what you want, but that piece of code also brings in a weird library which is completely irrelevant to you.

- Fragility
    

Even worse, if high level modules uses some internal details of a low level modules like a variable, it is susceptible to break when low level module is modified. This can lead to a change trail across several modules, which will waste more time and effort each time you want to change something.

## Enters DIP

Formal definition of DIP is as follows

_**High level modules should no depend on low level modules, instead both should depend on abstraction.**_

So basically we introduce a new layer of abstraction between two talking layers. e.g. an interface.

![[Pasted image 20251225172149.png]]

![[Pasted image 20251225172155.png]]

This interface defines a contract , which describes the API exposed by this interface. This interface is owned by top level modules, meaning interface API is tailored to the needs of top level modules. Low level modules merely implement this API, and high level modules consume it.

### How does this solve the problems?

- Code reusability problem
    

Top level module does not know about the low level module. It merely interacts with abstraction which in this case is an interface. So any implementation of the interface can be used interchangeably. Also any consumer accepting the interface definition can use any of the impls.

- Fragility
    

All interactions are bound by a strict interface, other details are abstracted behind this interface, causing the layers to be decoupled, only talking via defined interface.