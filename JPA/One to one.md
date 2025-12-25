**Unidirectional one to one**

unidirectional mapping can be done as follows

![](attachments/Pasted%20image%2020251225191940.png)

```
@OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "post_id")
    private ISBN isbnobj;
```

In above example, Book entity is mapped to ISBN in One to One mapping. In this example, foreign key post_id is maintained in Book table.

In case where ISBN has to be fetched long with Book, we can either use eager fetching or go for @Mapsid.

Eager or lazy fetch will always trigger another query which may ot be desirable.

Mapsid basically stores same primary key in the child table as parent table premary key. So both parent row, and child row always have the same primary key.

**Bidirectional One to One**

Bidirectional relation is same as one to many.

Example below
```
@Entity
@Table(name = "users")
public class User {
@OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id", referencedColumnName = "id")
    private Address address;
  }
     
     
@Entity
@Table(name = "address")
public class Address {
    @OneToOne(mappedBy = "address")
    private User user;
    }
```

in above example one user is mapped to one address. In above example oreign key(address_id) is owned by User side of mapping. So child side i.e. Address declares a mapped by attribute. **mapped by basically declares opposite side of the foreign key owner.**

MappedBy basically means which side of the entity will hold the foreign key. Mapped by is always declared by non-owner side of the relation, and it declared owner side field name as mappedBy value. One important thing to remember is mapped by value is mapped in both directions only when owner of the relation is set the value, otherwise only one way value is populated.



```
@Entity
public class Message {
@Id
@GeneratedValue(strategy = GenerationType.AUTO)
Long id;

@OneToOne
Email email;

=====================================================
@Entity
public class Email {
@Id
@GeneratedValue(strategy = GenerationType.AUTO)
Long id;

@OneToOne
// (mappedBy = "email")
Message message;
```

as shown above Message and email are not using mappedBy property. FOr the one to one to populate from both ends we’ll have to do as follows.

```
email = new Email("Proper");
message = new Message("Proper");
email.setMessage(message);
message.setEmail(email);
session.save(email);
session.save(message);
```

If we do as follows
```
email = new Email("Proper");
message = new Message("Proper");
email.setMessage(message);
session.save(email);
session.save(message);
```

message.getEmail() will return null. Only email.getMessage() will be populated, because that’s what we have populated above. So for both side to be populated we have to set values from both ends as above.

If we do mapping as


```
@Entity
public class Email {
@Id
@GeneratedValue(strategy = GenerationType.AUTO)
Long id;

@OneToOne
(mappedBy = "email")
Message message;
```

and we set 
```
email = new Email("Inverse Email");
message = new Message("Inverse Message");
message.setEmail(email);
session.save(email);
session.save(message);
```

only then, one to one mapping will be populated in both ways. If we do the opposite, i.e. set email.setMessage() in this acse, we will get a one sided relation again.

**here owning side of the relation is the Message, so value of the owning side will be reflected in DB and only that will populated mapping both ways. Inverse does not work.**

Who can be marked owner?

![](attachments/Pasted%20image%2020251225192200.png)

Above strategies do mandate that we pull either a value or null value for mapping to work, which can be inconvinient at times. To overcome that join table strategy is used.
