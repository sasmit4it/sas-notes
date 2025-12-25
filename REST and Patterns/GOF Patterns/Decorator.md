# General Idea

Some times we don’t need hard binding by inheritance. e.g. We have a UI element window. Window by default does not have a scroll bar, most of the times it does not need one. But some times when content is large enough, window will need a scroll bar. This means, we need a mechanism , which will add layers of functionality on objects at run time without binding it to core object.

This is achieved using decorators.

**decorator pattern** is a [design pattern](https://en.wikipedia.org/wiki/Design_pattern_\(computer_science\) "https://en.wikipedia.org/wiki/Design_pattern_(computer_science)") that allows behavior to be added to an individual [object](https://en.wikipedia.org/wiki/Object_\(computer_science\) "https://en.wikipedia.org/wiki/Object_(computer_science)"), dynamically, without affecting the behavior of other objects from the same [class](https://en.wikipedia.org/wiki/Class_\(computer_science\) "https://en.wikipedia.org/wiki/Class_(computer_science)").

Decorator use can be more efficient than subclassing, because an object's behavior can be augmented without defining an entirely new object.

# Structure

- implement the interface of the extended (decorated) object (`Component`) transparently by forwarding all requests to it
    
- perform additional functionality before/after forwarding a request.
    
- Enclose target in a decorator.
    
- This allows chaining of decorators to add more functionality dynamically.

![[Pasted image 20251224234513.png]]
```
// The Window interface class
public interface Window {
    void draw(); // Draws the Window
    String getDescription(); // Returns a description of the Window
}

// Implementation of a simple Window without any scrollbars
class SimpleWindow implements Window {
    @Override
    public void draw() {
        // Draw window
    }
    @Override
    public String getDescription() {
        return "simple window";
    }
}

// abstract decorator class - note that it implements Window
abstract class WindowDecorator implements Window {
    private final Window windowToBeDecorated; // the Window being decorated

    public WindowDecorator (Window windowToBeDecorated) {
        this.windowToBeDecorated = windowToBeDecorated;
    }
    @Override
    public void draw() {
        windowToBeDecorated.draw(); //Delegation
    }
    @Override
    public String getDescription() {
        return windowToBeDecorated.getDescription(); //Delegation
    }
}

// The first concrete decorator which adds vertical scrollbar functionality
class VerticalScrollBarDecorator extends WindowDecorator {
    public VerticalScrollBarDecorator (Window windowToBeDecorated) {
        super(windowToBeDecorated);
    }

    @Override
    public void draw() {
        super.draw();
        drawVerticalScrollBar();
    }

    private void drawVerticalScrollBar() {
        // Draw the vertical scrollbar
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", including vertical scrollbars";
    }
}


```