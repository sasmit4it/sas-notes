**This principle promotes high cohesion between classes.** Principle states

_**A class should have one and only one reason to change.**_

By not using SRP, you put multiple responsibilities in a single class. These responsibilities tend to change for different reasons and at different times.

By putting them in same class, you risk breaking unrelated functionality as a side effect of change of one functionality.

When you follow SRP, classes are cohesive and carry a single responsibility, hence only one reason to change.

Example of this would be a class that compiles report data and prints it. So this example carries two responsibilities

- Compilation of data
    
- Printing the data in correct format
    

here are two reason that would cause this class to change

- Data compilation procedure is changed
    
- Data printing format needs to be updated
    

By not doing SIP, and putting everything in single class , you risk breaking report printing, when you update something in data compilation procedure. That is regression, unpredictable and overall messy. Here is code example for same.


![[Pasted image 20251225171105.png]]


Here’s how you should fix this. Printing responsibility is now delegated to a new class.

![[Pasted image 20251225171133.png]]
