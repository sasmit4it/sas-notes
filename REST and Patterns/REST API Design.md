REST stands for representational state transfer protocol. It means , you have an underlying resource e.g. an employee resource. You share a particular representation of that resource via your API. Your API might be returning JSON response or XML response, or any other format. So that JSON/XML response is the representation of your API of that employee resource.

While designing APIs, you think of them as actual resources rather than actions that the API will do.

Think as if the resource that is being accessed is a static page, and then design the API.

`/service/getuser` is not restful URI. It has verb “get“ in it. Restful URI’s are mainly formed of nouns. For example /service/user is a restful URI for user resource.

Resources are of two types

1. top level resource e.g. Employee
    
2. nested resource or a resource that depends on other resource e.g. address of the employee
    

Table for few use cases and relevant sample URIs

|   |   |   |   |
|---|---|---|---|
|**description**|**URI**|**HTTP method**|**Body (if any)**|
|fetch single employee|/employees/{id}|GET||
|create a single employee|/employees|POST|{"name":"empName"}|
|update a single employee|/employees/{id}|PUT|{"firstname":"empName"}|
|delete single employee|/employees/{id}|DELETE||
|get one address of employee id 10|/employees/10/addresses/1|GET||
|create new address for employee id 10|/employees/10/addresses|POST|{"val":"val"}|
|update address 2 of employee id 10|/employees/10/addresses/2|PUT|{"val":"val"}|
|get list of all employees|/employees|GET||
|delete all addresses of employee id 10|/employees/10/adresses|DELETE||
|get all addresses of employee id 10|/employees/10/addresses|GET||
|update a list of addresses for employee 10|/employees/10/addresses|PUT|[{"val":"val"},{"val":"val"}]|
|get only 5 results|/employees?batchSize=5|GET|5 employees returned|
|filter by employee first name|/employees?name=mukesh|GET|mukesh employee returned|

**Commonly used HTTP methods:**

GET : fetch the resource

POST : create new resource

PUT : update the existing resource with the new one from current request

DELETE : delete the resource

PATCH : update only certain parts of the existing resource and leave the other parts as it is.

OPTIONS : not used in REST

**Idempotence:**

This is the ability of the operation to be performed repeatedly, without causing any side effect.

**GET** is idempotent.

**GET** `/employees/10` You just fetch employee id 10 and nothing is changed on server, even when request is made multiple times.

**DELETE** is idempotent.

**DELETE** `/employees/10` deletes the employee 10. if you re-do the same request , employee 10 is already deleted. Nothing wrong can happen here

**PUT** is idempotent.

**PUT** `/employees/10/addresses/2` requestBody: `{"val":"val"}` will update the employee id10 in first go and set value of val to "val". If you repeat this request value of val will be overwritten with same value i.e. "val".

**POST is non-idempotent.**

**POST** `/employees` requestBody: `{"name":"empName"}` creates a employee with id 1. If you resubmit this request you create employee with id 2 but with exact same content as employee id 1.

**Best Practices:**

- Use plurals for resources, e.g. for Employee resource URI should be `/employees`.
    
- For long nouns, use meaningful hyphens.
    
- Use resource nesting when necessary and logical. e.g. for addresses of the employee URL should be `/employees/10/addresses`. However nesting should not be overdone. Nesting is useful till 2 -3 nesting levels. After that rely on HAETOS type URLs in response.
    
- URLs should be **case insensitive.**
    
- e.g. for getting PO Box of employee URL may look like /employees/10/addresses/2/post-box. but instead return street code URL in address response itself. Typical HAETOS response as follows.
    

`"addressId" : "1",`

`"streetCode" : "5"`

`"links" :{`

`"po-box" : "/adress/1/po-box"`

`}`

- Versioning should be maintained at base of your URL
    

`https://mysite.com/v1/employees` for version 1  
`https://mysite.com/v2/employees` for version 2

- additional parameters like filter, page size page position should be passed in as query parameters.
    

`https://mysite.com/posts?tags=javascript`  
This endpoint will fetch any post that has a tag of JavaScript.

`/employees?lastName=Smith&age=30`

We get:

`[     {         "firstName": "John",         "lastName": "Smith",         "age": 30     } ]`

`http://example.com/articles?sort=+author,-datepublished`

Where `+` means ascending and `-` means descending. So we sort by author’s name in alphabetical order and `datepublished` from most recent to least recent.