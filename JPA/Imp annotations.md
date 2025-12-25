@SecondaryTable

This annotation is used to mark secondary table from which a field value will be loaded. Using this annotation entity can load values from many diffrent tables. When value of a column is frawn from secondary table, it should be marked with @column and table attribute.

```
@Entity
@Table(
name = "customer",
uniqueConstraints = {@UniqueConstraint(columnNames = "name")}
)
@SecondaryTable(name = "customer_details")
public class Customer {
@Id
public int id;
public String name;
@Column(table = "customer_details")
public String address;
```

@Basic

By default hibernate handles primitives types automatically. Set of these auto types is collectively called Basics. These includes primitives, wrappers, enums and any type implementing serializable. Behaviour of these basic types can be tweaked using @Basic. This one takes two parameters, Optional and fetch.

```
 @Basic(optional = false, fetch = FetchType.LAZY)
    private String name;
```

- Attributes of the _@Basic_ annotation are applied to JPA entities, whereas the attributes of _@Column_ are applied to the database columns
    
- _@Basic_ annotation’s _optional_ attribute defines whether the entity field can be _null_ or not; on the other hand, _@Column_ annotation’s _nullable_ attribute specifies whether the corresponding database column can be _null_
    
- We can use _@Basic_ to indicate that a field should be lazily loaded