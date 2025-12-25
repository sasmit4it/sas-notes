**Many to One**

This is the mapping which is controlled by many side of the relation, no matter if relation is bi or unidirectional. Following is an example on **unidirectional** many to one relation.

![](attachments/Pasted%20image%2020251225192302.png)

As you can see in above relation, comment table has post id, but Post is not aware of this relation.

This is mapped as
```
@ManyToOne
@JoinColumn(name = "post_id")
private Post post;
```

In such cases, since hibernate/JPA controls the foreign key(post_id in this case), this mapping is efficient.

**Bidirectional One to Many**

For each birectional @OneToMany there is a reciprocating @ManyToOne association on the other end.

In a bidirectional association, only one side can control the association. Means only one side holds actual foreign key that defines this mapping as database level. For this we use mappedBy property. mappedBy basically tells hibernate that, foreign key is on the other side of the relation. As shown above , comments table holds the foreign key to post.
```
@OneToMany(mappedBy = "post", cascade = CascadeType.ALL, orphanRemoval = true)
private List<PostComment> comments = new ArrayList<>();
```
**Unidirectional @OneToMany**

Unidirectional one to many does not have a many to one on the other side of mapping. Also you do not need to define the mappedBy property. Hibernate keeps all the mappings in a junction table.


```
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
private List<PostComment> comments = new ArrayList<>();
```

Althogh simple in definition, this kind of mapping takes performance hits, due to addition of one more table in between. Sample schema below.

![](attachments/Pasted%20image%2020251225192403.png)

The unidirectional @OneToMany relationship is less efficient both for reading data (three  
joins are required instead of two), as for adding (two tables must be written instead of  
one) or removing (entries are removed and added back again) child entries.

**One to May order Column**

example as below:


```
@Entity(name = "Department")
public static class Department {

    @Id
    private Long id;

    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    @OrderColumn(name = "order_id")
    @LazyCollection( LazyCollectionOption.EXTRA )
    private List<Employee> employees = new ArrayList<>();

```

This order_id column will be added in junction table in case of unidirectional mapping.

While sorting employees collection in department object, hibernate will use value of order_id as sorting criteria. This can be used to optimize querying and data manipulation operations like this. In below exmple ibernate is qerying third element of list from DB.

```
   elchild0_.id as id1_7_0_,
    elchild0_.my_value as my_value2_7_0_, // <-- just some child attribute
    elchild0_.parent_id as parent_i3_7_0_ 
from
    el_child elchild0_ 
where
    and elchild0_.el_child_order_column=2 // <-- 0-based element index
```

**@ElementCollection**

This is used to map premitive tpyes like int or string, or embedded types. Basically anything which cannot stand on it self as an entity.
```
ElementCollection
    @CollectionTable(name = "page_nu")
    private List<Integer> pageNums = new ArrayList<>();
```

Premitive values are stored int a separate table, whose name can be specified as above.


