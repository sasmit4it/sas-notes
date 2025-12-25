
![](attachments/Pasted%20image%2020251225185942.png)

Principle states that

A subclass should be substitutable to a parent class, without breaking any of it’s client’s behavior.

A great example illustrating LSP (given by Uncle Bob in a podcast I heard recently) was how sometimes something that sounds right in natural language doesn't quite work in code. Not following this , the code will be unpredictable. More over client has to know internal structure to make things work, causing it to be tightly coupled.

In mathematics, a `Square` is a `Rectangle`. Indeed it is a specialization of a rectangle. The "is a" makes you want to model this with inheritance. However if in code you made `Square` derive from `Rectangle`, then a `Square` should be usable anywhere you expect a `Rectangle`. This makes for some strange behavior.

Imagine you had `SetWidth` and `SetHeight` methods on your `Rectangle` base class; this seems perfectly logical. However if your `Rectangle` reference pointed to a `Square`, then `SetWidth` and `SetHeight` doesn't make sense because setting one would change the other to match it. In this case `Square` fails the Liskov Substitution Test with `Rectangle` and the abstraction of having `Square` inherit from `Rectangle` is a bad one.

```
class Super {
    private int myInt;

    // Precondition: newValue can be anything.
    public void setMyInt(int newValue) {
        myInt = newValue;
    }
}

class Sub extends Super {

    // Stronger precondition: newValue must be > 0.
    @Override
    public void setMyInt(int newValue) {
        if (newValue <= 0) throw new RuntimeException("Must be positive");
        super.setMyInt(newValue);
    }
}
```
here child class is adding additional condition, which is not enforced by parent class. So following code will break

```
 List<Super> types = new ArrayList();
 types.add(new Sub());
 types.add(new Super());
 
 for(Super s: types){
s.setMyInt(-3);
}
```

Another example of violation is here
```
public class Task
{
     public Status Status { get; set; }

     public virtual void Close()
     {
         Status = Status.Closed;
     }
}

public class ProjectTask : Task
{
     public override void Close()
     {
          if (Status == Status.Started) 
              throw new Exception("Cannot close a started Project Task");

          base.Close();
     }
}
```

- Preconditions cannot be strengthened in a subtype.
    
- Postconditions cannot be weakened in a subtype.
    

Client of above code will have to resort to if/else or switch case, causing tight coupling.


```
if(task instanceof ProjectTask)
//check if status is not started
else 
task.close.
```

This can be addressed as follows, by bringing the strengthened pre-condition to the top level of the inheritance hierarchy:
```
public class Task {
    public Status Status { get; set; }
    public virtual bool CanClose() {
        return true;
    }
    public virtual void Close() {
        Status = Status.Closed;
    }
}

public class ProjectTask : Task {
    public override bool CanClose() {
        return Status != Status.Started;
    }
    public override void Close() {
        if (!CanClose()) 
            throw new Exception("Cannot close a started Project Task");
        base.Close();
    }
}
```

