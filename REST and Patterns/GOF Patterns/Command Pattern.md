# General Idea

Basic idea beahind the command pattern is of hiring a professional for doing some complex task. I need to get some task done, but I don’t know or care how that task is done. I simply hire a professional who has the know how about that task, and ask him to do it. When I need to get another task done, I go to another professional who has the know how about that task. So I am completely detached from that task. I do not need to worry about steps needed, or sequence of those steps. That is taken care by the professional. So I am the client, professional is the command object, and that commadn object does the actual task.

the **command pattern** is a [behavioral](https://en.wikipedia.org/wiki/Behavioral_pattern "https://en.wikipedia.org/wiki/Behavioral_pattern") [design pattern](https://en.wikipedia.org/wiki/Design_pattern_\(computer_science\) "https://en.wikipedia.org/wiki/Design_pattern_(computer_science)") in which an object is used to [encapsulate](https://en.wikipedia.org/wiki/Information_hiding "https://en.wikipedia.org/wiki/Information_hiding") all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

The Command pattern can turn a specific method call into a stand-alone object. This change opens up a lot of interesting uses: you can pass commands as method arguments, can queue commands and replay them at later time, can implement do -undo operations etc.

![](attachments/Pasted%20image%2020251225190247.png)

In the above [UML](https://en.wikipedia.org/wiki/Unified_Modeling_Language "https://en.wikipedia.org/wiki/Unified_Modeling_Language") [class diagram](https://en.wikipedia.org/wiki/Class_diagram "https://en.wikipedia.org/wiki/Class_diagram"), the `Invoker` class doesn't implement a request directly. Instead, `Invoker` refers to the `Command` interface to perform a request (`command.execute()`), which makes the `Invoker` independent of how the request is performed. The `Command1` class implements the `Command` interface by performing an action on a receiver (`receiver1.action1()`).

The [UML](https://en.wikipedia.org/wiki/Unified_Modeling_Language "https://en.wikipedia.org/wiki/Unified_Modeling_Language") [sequence diagram](https://en.wikipedia.org/wiki/Sequence_diagram "https://en.wikipedia.org/wiki/Sequence_diagram") shows the run-time interactions: The `Invoker` object calls `execute()` on a `Command1` object. `Command1` calls `action1()` on a `Receiver1` object, which performs the request.

```
// An interface for command
interface Command
{
    public void execute();
}
 
// Light class and its corresponding command
// classes
class Light
{
    public void on()
    {
        System.out.println("Light is on");
    }
    public void off()
    {
        System.out.println("Light is off");
    }
}
class LightOnCommand implements Command
{
    Light light;
 
    // The constructor is passed the light it
    // is going to control.
    public LightOnCommand(Light light)
    {
       this.light = light;
    }
    public void execute()
    {
       light.on();
    }
}
class LightOffCommand implements Command
{
    Light light;
    public LightOffCommand(Light light)
    {
        this.light = light;
    }
    public void execute()
    {
         light.off();
    }
}

class SimpleRemoteControl
{
    Command slot;  // only one button
 
    public SimpleRemoteControl()
    {
    }
 
    public void setCommand(Command command)
    {
        // set the command the remote will
        // execute
        slot = command;
    }
 
    public void buttonWasPressed()
    {
        slot.execute();
    }
}

class RemoteControlTest
{
    public static void main(String[] args)
    {
        SimpleRemoteControl remote =
                  new SimpleRemoteControl();
        Light light = new Light();
       
 
        // we can change command dynamically
        remote.setCommand(new
                    LightOnCommand(light));
        remote.buttonWasPressed();
        remote.setCommand(new
                    LightOffCommand(light));
        remote.buttonWasPressed();
  }
```

Notice that the remote control doesn’t know anything about turning on the stereo. That information is contained in a separate command object. This reduces the coupling between them. 

**Advantages:**

- Makes our code extensible as we can add new commands without changing existing code.
    
- Reduces coupling between the invoker and receiver of a command.