
![](attachments/Pasted%20image%2020251225185915.png)

Principle states:

_**Client should not be made to depend on methods/functionality it does not need.**_

Interfaces that do a lot of things together are called **fat interfaces.** Fat interfaces make your code coupled with methods it does not even need. This will make your code break when something very unrelated was changed. Also your code becomes littered with methods which do nothing.

Solution is to keep your interfaces as lean and concise as possible. Such interfaces are called as **role** interfaces. They basically follow “single responsibility“.

Following is a bad example. here Iworker is a fat interface declaring two methods. These two methods are implemented correctly by `Worker` and `SuperWorker` , but they are not quiet fit for `Robot. So Robot` has a dumb method doing nothing.

```
// interface segregation principle - bad example
interface IWorker {
	public void work();
	public void eat();
}

class Worker implements IWorker{
	public void work() {
		// ....working
	}
	public void eat() {
		// ...... eating in launch break
	}
}

class SuperWorker implements IWorker{
	public void work() {
		//.... working much more
	}

	public void eat() {
		//.... eating in launch break
	}
}

class Robot implements IWorker{
	public void work() {
		//.... working much more
	}

	public void eat() {
		//nothing, since robot does not need break
	}
}
class Manager {
	IWorker worker;

	public void setWorker(IWorker w) {
		worker=w;
	}

	public void manage() {
		worker.work();
	}
}
```

Solution to above problem is as follows

```
interface IWorkable {
	public void work();
}

interface IFeedable{
	public void eat();
}

class Worker implements IWorkable, IFeedable{
	public void work() {
		// ....working
	}

	public void eat() {
		//.... eating in launch break
	}
}

class Robot implements IWorkable{
	public void work() {
		// ....working
	}
}

class SuperWorker implements IWorkable, IFeedable{
	public void work() {
		//.... working much more
	}

	public void eat() {
		//.... eating in launch break
	}
}

class Manager {
	Workable worker;

	public void setWorker(Workable w) {
		worker=w;
	}

	public void manage() {
		worker.work();
	}
}
```

