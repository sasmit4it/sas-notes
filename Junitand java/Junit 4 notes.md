## parametrized jUnits

You can test multiple values in the same test case without repeating the test code. You have to use parametrized test for that.

1. first and @RunWith(Parameterized. Class) annotation to class
    
2. add a static method that returns collection of values and mark it with @Parameters. This acts as parameter store.
    
3. Add private fields to test class
    
4. Write a constructor that initializes these fields.
    

run. You have setup the parameterized tests.

```
@RunWith(Parameterized.class)
public class StringHelperTest {

	StringHelper helper = new StringHelper();
	
	private String input;
	private String expected;
	
	
	public StringHelperTest(String input, String expected) {
		super();
		this.input = input;
		this.expected = expected;
	}

	@Parameters
	public static Collection<String[]> getColl() {
		List<String[]> li = new ArrayList<String[]>();
		li.add(new String[]{"CD", "AACD"});
		li.add(new String[]{"CE", "AACE"});
		return li;
	}
	
	@Test
	public void parametriezedTest() {
		assertEquals(expected, "AA"+input);
	}
```

## Imp annotations

- @Before @After
    

The code marked `@Before` is executed before each test, similarly code marked with @After runs after each test.

- @BeforeClass @AfterClass
    

these methods need to be static. They are executed once, before first test and after the last test.

They act as static initializers, so these execute before or after your test class instance is created.

- expect exception
    

`@Test(expected = NullPointerException.class)`

- verify
    

verifies if call was made to the method. It can be also used with other methods like never(), altleastonce() etc.

```
@Test
	public void testDeleteToDo() {
		ExtCallerApi mockService = Mockito.mock(ExtCallerApi.class);
		List<String> resp = new ArrayList<String>();
		resp.add("spring");
		resp.add("spring 1");
		resp.add("no");
		
		Mockito.when(mockService.getToDos("dummy")).thenReturn(resp);//return dummy resp
		ToDoManager toDoManager = new ToDoManager(mockService); //mock injected
		toDoManager.removeToDo("dummy"); //actual call to service containing mock
		
		Mockito.verify(mockService).deleteToDo("no"); //verify using verify()
		Mockito.verify(mockService, Mockito.atLeastOnce()).deleteToDo("no"); //atLeastonce()
		Mockito.verify(mockService, Mockito.never()).deleteToDo("spring"); //never()
		
	}
	
	
	//ToDoManager ->  removeToDo service impl for reference
	public void removeToDo(String user) {
		List<String> apiResp = extCallerApi.getToDos(user);
		for(String s: apiResp) {
			if(!s.contains("spring")) {
				extCallerApi.deleteToDo(s);
			}
		}
	}
```

- ArgumentCaptors
    

Mockito provides captors to capture arguments passed to mocks. They can be initialized and used as below.

```
@Test
	public void testArgCaptor() {
		ExtCallerApi mockService = Mockito.mock(ExtCallerApi.class);
		ArgumentCaptor<String> strCaptor = new ArgumentCaptor<String>();
		List<String> resp = new ArrayList<String>();
		resp.add("spring");
		resp.add("spring 1");
		resp.add("no");
		
		Mockito.when(mockService.getToDos("dummy")).thenReturn(resp); //mock behaviour
		ToDoManager toDoManager = new ToDoManager(mockService); //inject moc to service
		toDoManager.removeToDo("dummy"); //actual call to service containing mock
		
		Mockito.verify(mockService).deleteToDo(strCaptor.capture()); //verify the call using verify and capture the argument 
		assertEquals(strCaptor.getValue(), "no"); //assert arg value
	}
```


```
@Test
	public void testArgCaptorMultiple() {
		ExtCallerApi mockService = Mockito.mock(ExtCallerApi.class);
		ArgumentCaptor<String> strCaptor = ArgumentCaptor.forClass(String.class);
		List<String> resp = new ArrayList<String>();
		resp.add("spring");
		resp.add("spring 1");
		resp.add("no"); //call 1 for this
		resp.add("no1"); //call 2 for this
		
		Mockito.when(mockService.getToDos("dummy")).thenReturn(resp); //mock behaviour
		ToDoManager toDoManager = new ToDoManager(mockService); // inject mock
		toDoManager.removeToDo("dummy"); //service call
		
		Mockito.verify(mockService, Mockito.times(2)).deleteToDo(strCaptor.capture()); //verify mock was called twice
		assertEquals(strCaptor.getAllValues().get(0), "no"); //get all the captured arg values. compare first value to given value
	}
```
