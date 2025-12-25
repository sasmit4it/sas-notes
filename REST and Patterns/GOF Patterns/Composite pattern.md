# General Idea

When dealing with Tree-structured data, programmers often have to discriminate between a leaf-node and a branch. This makes code more complex, and therefore, more error prone. The solution is an interface that allows treating complex and primitive objects uniformly. In [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming "https://en.wikipedia.org/wiki/Object-oriented_programming"), a composite is an object designed as a composition of one-or-more similar objects, all exhibiting similar functionality.

Composite should be used when clients ignore the difference between compositions of objects and individual objects.[[1]](https://en.wikipedia.org/wiki/Composite_pattern#cite_note-GangOfFour-1 "https://en.wikipedia.org/wiki/Composite_pattern#cite_note-GangOfFour-1") If programmers find that they are using multiple objects in the same way, and often have nearly identical code to handle each of them, then composite is a good choice

![[Pasted image 20251224234614.png]]

Both leaf and composite extend a common interface, but composite related operations are only available on composite and not on leaf.

```
/** "Component" */
interface Graphic {
    //Prints the graphic.
    public void print();
}

/** "Composite" */
class CompositeGraphic implements Graphic {
    //Collection of child graphics.
    private final List<Graphic> childGraphics = new ArrayList<>();

    //Adds the graphic to the composition.
    public void add(Graphic graphic) {
        childGraphics.add(graphic);
    }
    
    //Prints the graphic.
    @Override
    public void print() {
        for (Graphic graphic : childGraphics) {
            graphic.print();  //Delegation
        }
    }
}

/** "Leaf" */
class Ellipse implements Graphic {
    //Prints the graphic.
    @Override
    public void print() {
        System.out.println("Ellipse");
    }
}

/** Client */
class CompositeDemo {
    public static void main(String[] args) {
        //Initialize four ellipses
        Ellipse ellipse1 = new Ellipse();
        Ellipse ellipse2 = new Ellipse();
        Ellipse ellipse3 = new Ellipse();
        Ellipse ellipse4 = new Ellipse();

        //Creates two composites containing the ellipses
        CompositeGraphic compositGraphic2 = new CompositeGraphic();
        compositGraphic2.add(ellipse1);
        compositGraphic2.add(ellipse2);
        compositGraphic2.add(ellipse3);
        
        CompositeGraphic compositGraphic3 = new CompositeGraphic();
        compositGraphic3.add(ellipse4);
        
        //Create another graphics that contains two graphics
        CompositeGraphic compositGraphic = new CompositeGraphic();
        compositGraphic.add(compositGraphic2);
        compositGraphic.add(compositGraphic3);

        //Prints the complete graphic (Four times the string "Ellipse").
        compositGraphic.print();
    }
}
```