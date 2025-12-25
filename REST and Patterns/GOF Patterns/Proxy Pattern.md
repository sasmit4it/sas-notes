1. Use proxy when you want to lazy instantiate a costly object. A lightweight proxy takes it’s place for time being. When you actually need the heavy object, proxy instantiates it and routes your calls to it.
    
2. Use proxy when you want to add additional check before of after the object call. So before calling object, your call is routed via proxy which does the needed checks, and based on those checks it decides weather to call or not call original object.
![[Pasted image 20251224233958.png]]

- [Adapter](https://refactoring.guru/design-patterns/adapter "https://refactoring.guru/design-patterns/adapter") provides a different interface to the wrapped object, [Proxy](https://refactoring.guru/design-patterns/proxy "https://refactoring.guru/design-patterns/proxy") provides it with the same interface, and [Decorator](https://refactoring.guru/design-patterns/decorator "https://refactoring.guru/design-patterns/decorator") provides it with an enhanced interface.
    
- [Facade](https://refactoring.guru/design-patterns/facade "https://refactoring.guru/design-patterns/facade") is similar to [Proxy](https://refactoring.guru/design-patterns/proxy "https://refactoring.guru/design-patterns/proxy") in that both buffer a complex entity and initialize it on its own. Unlike _Facade_, _Proxy_ has the same interface as its service object, which makes them interchangeable.
    
- [Decorator](https://refactoring.guru/design-patterns/decorator "https://refactoring.guru/design-patterns/decorator") and [Proxy](https://refactoring.guru/design-patterns/proxy "https://refactoring.guru/design-patterns/proxy") have similar structures, but very different intents. Both patterns are built on the composition principle, where one object is supposed to delegate some of the work to another. The difference is that a _Proxy_ usually manages the life cycle of its service object on its own, whereas the composition of _Decorators_ is always controlled by the client.

```
interface Image {
    public void displayImage();
}

// On System A
class RealImage implements Image {
    private final String filename;

    /**
     * Constructor
     * @param filename
     */
    public RealImage(String filename) {
        this.filename = filename;
        loadImageFromDisk();
    }

    /**
     * Loads the image from the disk
     */
    private void loadImageFromDisk() {
        System.out.println("Loading   " + filename);
    }

    /**
     * Displays the image
     */
    public void displayImage() {
        System.out.println("Displaying " + filename);
    }
}

// On System B
class ProxyImage implements Image {
    private final String filename;
    private RealImage image;
    
    /**
     * Constructor
     * @param filename
     */
    public ProxyImage(String filename) {
        this.filename = filename;
    }

    /**
     * Displays the image
     */
    public void displayImage() {
        if (image == null) {
           image = new RealImage(filename);
        }
        image.displayImage();
    }
}

class ProxyExample {
   /**
    * Test method
    */
   public static void main(final String[] arguments) {
        Image image1 = new ProxyImage("HiRes_10MB_Photo1");
        Image image2 = new ProxyImage("HiRes_10MB_Photo2");

        image1.displayImage(); // loading necessary
        image1.displayImage(); // loading unnecessary
        image2.displayImage(); // loading necessary
        image2.displayImage(); // loading unnecessary
        image1.displayImage(); // loading unnecessary
    }
}
```