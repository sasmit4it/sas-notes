A driver class holds a common logic , which can be applied to multiple concrete products, the driver does not know exactly which product to use. So move the product creation out of driver class into the concrete products.

Logic is kept at once place, reducing duplicity. but same logic can be applied to different implementations by just swapping the concrete implementation.

Achieves abstraction, since core logic is not aware of implementations, it only knows abstract product.

Also achieves open close principle , since core logic need not change when a new type is to be supported. All you have to do is create a new subclass, and use it, while calling the core logic.
![[Pasted image 20251224233616.png]]

As shown above, pattern has abstract product, abstract creator, and concrete products and concrete creators. Abstract creator holds AnOperation , which is a concrete operation , but it refers to creator factory method, which is abstract. This abstract method is realized by each of many concrete creators. So essentially, object creation is deferred to subclass, away from core logic in AnOperation.
```
//Abstract product
public abstract class Room {
    abstract void connect(Room room);
}

//concrete products
public class MagicRoom extends Room {
    public void connect(Room room) {}
}

public class OrdinaryRoom extends Room {
    public void connect(Room room) {}
}

//abstract creator holding actual room connecting logic and abstract creator method.
public abstract class MazeGame {
     private final List<Room> rooms = new ArrayList<>();

     public MazeGame() {
          Room room1 = makeRoom();
          Room room2 = makeRoom();
          room1.connect(room2);
          rooms.add(room1);
          rooms.add(room2);
     }

     abstract protected Room makeRoom();
}

//concrete creators
public class MagicMazeGame extends MazeGame {
    @Override
    protected MagicRoom makeRoom() {
        return new MagicRoom();
    }
}

public class OrdinaryMazeGame extends MazeGame {
    @Override
    protected OrdinaryRoom makeRoom() {
        return new OrdinaryRoom();
    }
}
//client code
MazeGame ordinaryGame = new OrdinaryMazeGame();
MazeGame magicGame = new MagicMazeGame();
```
As shown above MazeGame constructor holds room connecting logic, and it refers to abstract makeRoom method. So MazeGame does not know which room it will connect. Actual rooms being connected are provided by `MagicMazeGame` and `OrdinaryMazeGame` .
