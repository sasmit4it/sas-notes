JPA supports 4 types of inheritance mappings.

**@MappedSuperClass**

In this type, there is a top level super class, which contains all the common fields. This top level class is not marked as an entity. The child classes extending this class inherit all the fields, and respective tables of those entitites store these fileds in columns.

```
@MappedSuperclass
public abstract class Publication {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = “id”, updatable = false, nullable = false)
    protected Long id;
 
    @Column
    protected String title;
 
    @Version
    @Column(name = “version”)
    private int version;
 
    @Column
    @Temporal(TemporalType.DATE)
    private Date publishingDate;
 
    …
}

==================================
@Entity(name = “Book”)
public class Book extends Publication {
 
    @Column
    private int pages;
 
    …
}
```

So this mapping is when all we want to do is stare common fields and there mappings with subclasses.

Disavantage of this approach is, this does not support polymorphic queires.

Polymorphic queires are the HQL quries which fetch specified class objects along with all the subclass objects as well.

e.g.

from Cat as cat

returns instances not only of `Cat`, but also of subclasses like `DomesticCat`. Hibernate queries may name _any_ Java class or interface in the `from` clause. The query will return instances of all persistent classes that extend that class or implement the interface.

**Single table mapping**

All the entities of the hierarchy are stored in the same table. While storing hibernate adds on more colmn to data i.e. DType or _DiscriminatorColumn_ to identify which entity this pericular row belongs to. While storing, hibernate populates only those columns which are part of current entity and leaves others as null.

Polymorphic queires work here, but only problem is, you cannot define not null attributes on the child entities. They are allowed on columns from pparent entity.That's because not null attributes are defined at column level, and any one column is shared by multiple entities. Something which is null for current entity may not be null for others.

```
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = “Publication_Type”)
public abstract class Publication {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = “id”, updatable = false, nullable = false)
    protected Long id;
 
    @Column
    protected String title;
 
    @Version
    @Column(name = “version”)
    private int version;
 
    @ManyToMany
    @JoinTable(name = “PublicationAuthor”, joinColumns = { @JoinColumn(name = “publicationId”, referencedColumnName = “id”) }, inverseJoinColumns = { @JoinColumn(name = “authorId”, referencedColumnName = “id”) })
    private Set authors = new HashSet();
    }
    ======================================
    @Entity(name = “Book”)
@DiscriminatorValue(“Book”)
public class Book extends Publication {
 
    @Column
    private int pages;
 
    …
}

@Entity(name = “BlogPost”)
@DiscriminatorValue(“Blog”)
public class BlogPost extends Publication {
 
    @Column
    private String url;
 
    …
}

```

### Table per Class

The table per class strategy is similar to the mapped superclass strategy. The main difference is that the superclass is now also an entity. **Each of the concrete classes gets still mapped to its own database table. If parent is abstract, all it’s fields are mapped to child tabe.** This mapping allows you to use polymorphic queries and to define relationships to the superclass. But the table structure adds a lot of complexity to polymorphic queries, and you should, therefore, avoid them. While querying, hibernate has to do a lot of table joins reducing the query performance.

```
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class Publication {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = “id”, updatable = false, nullable = false)
    protected Long id;
 
    @Column
    protected String title;
 
    @Version
    @Column(name = “version”)
    private int version;
    }
```

_**Difference between mapped super class and table per class**_

_MappedSuperClass must be used to inherit properties, associations, and methods._

_Table per class must be used when you have an entity, and several sub-entities._

_You can tell if you need one or the other by answering this questions: is there some other entity in the model which could have an association with the base class?_

For example:

- You can have several kinds of messages: SMS messages, email messages, or phone messages. And a person has a list of messages. You can also have a reminder linked to a message, regardless of the kind of message. In this case, Message is clearly an entity, and entity inheritance must be used.
    
- All your domain objects could have a creation date, modification date and ID, and you could thus make them inherit from a base AbstractDomainObject class. But no entity will ever have an association to an AbstractDomainObject. It will always be an association to a more specific entity: Customer, Company, whatever. In this case, it makes sense to use a MappedSuperClass.
    

### Joined

The joined table approach maps each class of the inheritance hierarchy to its own database table. This sounds similar to the table per class strategy. But this time, also the abstract superclass _Publication_ gets mapped to a database table. This table contains columns for all shared entity attributes. The tables of the subclasses are much smaller than in the table per class strategy. They hold only the columns specific for the mapped entity class and a primary key with the same value as the record in the table of the superclass.

ach query of a subclass requires a join of the 2 tables to select the columns of all entity attributes. That increases the complexity of each query, but it also allows you to use _not null_ constraints on subclass attributes and to ensure data integrity. The definition of the superclass _Publication_ is similar to the previous examples. The only difference is the value of the inheritance strategy which is _InheritanceType.JOINED_.


```
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Publication {
 
        @Id
        @GeneratedValue(strategy = GenerationType.AUTO)
        @Column(name = “id”, updatable = false, nullable = false)
        protected Long id;
 
        @Column
        protected String title;
 
        @Version
        @Column(name = “version”)
        private int version;
```

