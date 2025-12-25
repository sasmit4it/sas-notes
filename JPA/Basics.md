to setup H2 db, just include H2 dependency in pom.

When H2 is configured as DB, spring boot automatically picks up data data.sql from resources folder.

Once you have Entity in your project, and DB is H2, Spring data automatically creates the tables in db. You just need to pre-populate these tables using data.sql.

**enable h2 console with following property**

spring.h2.console.enabled = true

**show all the jpa statistics**

spring.jpa.properties.hibernate.generate_statistics = true

loggin.level.org.hibernate.stat = debug

**show all queries**

spring.jpa.show-sql = true

spring.jpa.properties.hibernate.format_sql = true

logging.level.org.hibernate.type=trace //this one show param values being set to queries

Object once saved to persistent context should not be saved again. DOing this will cause a duplicate entry of that object in DB , with just the id field different. For such cases always use saveOrUpdate method.

Moreover, once you have an object that is attached to a persistent context, you need not save or saveOrUpdate again. Hibernate does that for you.

If you save an object in a session, and try to fetch it again, hibernate will return exact same object. == condition will be true on it because, it is the same object.

Load vs get


![](attachments/Pasted%20image%2020251225191452.png)
save vs persist

`Save()`

1. Returns generated Id after saving. Its return type is `Serializable`;
    
2. Saves the changes to the database outside of the transaction;
    
3. Assigns the generated id to the entity you are persisting;
    
4. `session.save()` for a detached object will create a new row in the table.
    

`Persist()`

1. Does not return generated Id after saving. Its return type is `void`;
    
2. Does not save the changes to the database outside of the transaction;
    
3. Assigns the generated Id to the entity you are persisting;
    
4. `session.persist()` for a detached object will throw a `PersistentObjectException`, as it is not allowed.
    

**Session.refresh() and merge()**

refresh() copies all the values from db to the entity object.

merge does the opposite. It copies all the values from a detached entity in the db row.