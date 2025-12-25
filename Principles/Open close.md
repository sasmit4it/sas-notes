![[Pasted image 20251225171204.png]]

_**A class should be open for extension but closed for modification.**_

What this means is, class should facilitate extension rather than modification. Implication of this is that, once a code is tested and put to prod, it need not be modified again. If it is modified, it has to be re-tested , it’s dependencies evaluated again etc. causing unnecessary waste of time and effort. So class should be written in such a way that, any new functionality can be accommodated by extending the class.

If we go at code level, this is achieved using inheritance, particularly by interfaces. If you don’t have a common defined interface for collection of interchangeable types, you’ll have to use switch statement or if/else. When a new type is added to your collection, you have to hunt down all such conditional statements and update them with new type. You’ll have to touch all the code that depends on these types even though that code was not actually changed.

example:

You have a rectangle class as below

```
public class Rectangle
{
    public double Width { get; set; }
    public double Height { get; set; }
}
```

```
public class AreaCalculator
{
    public double Area(Rectangle[] shapes)
    {
        double area = 0;
        foreach (var shape in shapes)
        {
            area += shape.Width*shape.Height;
        }

        return area;
    }
}
```
If you want to support circle type, you’ll have to resort to this
```
public double Area(object[] shapes)
{
    double area = 0;
    foreach (var shape in shapes)
    {
        if (shape is Rectangle)
        {
            Rectangle rectangle = (Rectangle) shape;
            area += rectangle.Width*rectangle.Height;
        }
        else
        {
            Circle circle = (Circle)shape;
            area += circle.Radius * circle.Radius * Math.PI;
        }
    }

    return area;
}
```
Because of a new type was added, client code had to be updated. This violates the principle. This can be fixed as below, by using an abstract common type. In below code, even if a new type is added, the area computation is encapsulated in the type itself. Class’s client need not update anything. It just needs to call the unified area method on whatever type that is being passed.

```
public abstract class Shape
{
    public abstract double Area();
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    public override double Area()
    {
        return Width*Height;
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    public override double Area()
    {
        return Radius*Radius*Math.PI;
    }
}

public double Area(Shape[] shapes)
{
    double area = 0;
    foreach (var shape in shapes)
    {
        area += shape.Area();
    }

    return area;
}
```