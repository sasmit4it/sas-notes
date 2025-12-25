A composite id is represented by a primary key class with one or more persistent attributes. Composite ID uses more than one column of the table to identify the record.

**The primary key class must fulfil several conditions**:

- It should be defined using _@EmbeddedId_ or _@IdClass_ annotations.
    
- It should be public, serializable and have a public no-arg constructor.
    
- Finally, it should implement _equals()_ and _hashCode()_ methods.
    

Key class is marked by @`Embeddable` annotation, while the ID in the actual entity is marked by `@EmbeddedId`.

```
@Embeddable
public class OrderEntryPK implements Serializable {

    private long orderId;
    private long productId;

    // standard constructor, getters, setters
    // equals() and hashCode() 
}
```

```
@Entity
public class OrderEntry {

    @EmbeddedId
    private OrderEntryPK entryId;

    // ...
}
```
