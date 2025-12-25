Used primarily when we have to define multi-dimensional classes. e.g. we have a shape interface having square and circle as concrete impls. Now if we want to introduce color property to these shapes, we will have to multiply the number of classes with new color values.

![[Pasted image 20251224233736.png]]
To avoid explosion of classes in this scenario, we do as follows.
![[Pasted image 20251224233754.png]]

new color property is composed inside the shape hierarchy. With this property we also introduce a setter to this property. This setter can be used to switch the color property at runtime.

![[Pasted image 20251224233832.png]]

as described above, implementer is decoupled from abstraction. Implementer can be swapped at runtime.

You can extend the Abstraction and Implementer hierarchies independently.
```
public class BridgePattern {

	public static void main(String[]arg) {
		shape1 shape = new square();
		shape.setColor(new red());
		shape.draw();
	}
}

//implementation and concrete impls
interface color1{
	void paint();
}

class blue implements color1{

	@Override
	public void paint() {
		System.out.println("blue ");
		
	}
}

class red implements color1{

	@Override
	public void paint() {
		System.out.println("red ");
		
	}	
}

//abstraction. Holds reference to impl. Also a setter to swap impl.
abstract class shape1{
	color1 color;
	
	void setColor(color1 color) {
		this.color = color;
	}
	
	abstract void draw();
}

class square extends shape1{

	@Override
	void draw() {
		System.out.println("square ");
		this.color.paint();
		
	}
	
}

class circle extends shape1{

	@Override
	void draw() {
		System.out.println("circle ");
		this.color.paint();
		
	}
	
}
```


