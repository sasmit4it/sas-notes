## Lambdas

### what are lambdas?

1. lambda is basically a piece of code that stands itself, i.e. without a method or class.
    
2. It is basic building block for functional programming.
    
3. You can assign a block of code to a variable and can pass that around as that variable.
    

### Why lambdas?

1. Enables functional programming
    

[Functional programming](https://en.wikipedia.org/wiki/Functional_programming)

1. Readable and concise code
    

Code becomes really short, avoiding all the boilerplate.

1. Enables support for parallel processing
    

### Basic lambda example
```
public static void main(String[][]args) {
		Greeter lambda = () -> System.out.print("hey");
		lambda.greet();
	}
	
interface Greeter {	
	void greet();
}
```

As shown above, we have declared a single line lambda in main method. Type of the lambda is Greeter interface.

e do not need to declare the type in lambda definition, the type is automatically inferred by compiler using interface definition.

```
public static void main(String[][]args) {
		StringLen lambda = s -> return s.length();
		lambda.greet();
	}
	
interface StringLen{	
	void getStringLength(String input);
}
```

In above example, we have not declared type of lambda variable s. Complier is smart enough to associate the variable type with the type of only method’s parameter of the associated interface i.e. StringLen. This is called type inference. We don’t have to specify type, yet complier is able to find it based on the inferred type.

### Functional interfaces

Type of a lambda is always a functional interface.

Functional interface is an interface , which has only one abstract method. It can have multiple default methods, but it has to have only one abstract method.

We can mark an interface functional interface by adding annotation @FuntionalInterface. This annotation prevents accidental addition of another abstract method to existing functional interface.

Notice you need a functional interface for this lambda to work. Java has provided some out of the box functional interfaces, which match most of the common requirements. Details as below.

`package java.util.function`

[java.util.function (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)

one detailed example given below.

```
public static void main(String[]args) {
		List<Person> people = Arrays.asList(new Person("Charles","Dickens",60),
				new Person("Lewis","Carroll",42));		
		//1.sort list by las name
		Collections.sort(people , (p1, p2)-> p1.getLastName().compareTo(p2.getLastName()) );
		//2. print all
		print(people, p -> true);
		System.out.println("============================");
		//3. print first name starting with C
		print(people, p -> p.getFirstName().startsWith("C"));
	}
	public static void print(List<Person>li, Predicate<Person> condition) {
		for(Person p : li) {
			if(condition.test(p)) {
				System.out.println(p);
			}
		}
	}
```
### Closures

Closures are basically enclosing methods around the lambda. Lambdas can use variables from enclosing methods/scopes, only when the variable is final or effectively final i.e. it’s value is never modified, in or out of the lambda.

```
public static void main(String[]args) {
    int var = 5;
	Consumer<Integer> cons= (p)-> System.out.println(p+var);
	cons.accept(5);
}
```

invalid case
```
public static void main(String[]args) {
    int var = 5;
    var++;
	Consumer<Integer> cons= (p)-> System.out.println(p+var);
	cons.accept(5);
}
```