So while beginning to break your monolith to microservices, you’ll need to start to identify aggregates and bounded contexts.

Bounded contexts: These are the areas of your domain, where one entity holds the same meaning to every actor in the system. e.g. There is a subsription service and there is content delivery service. For subscription service a subscription will hold a lot of info, like start date end date, valid or not etc. But for content delivery service, subscription is nothing more than a byte string or String. So these two can be two separate BCs. subscription bounded context and content delivery BC. This in a way achieves abstraction and service level.

Aggregates: Going further, once you decide your BCs, you need to define the aggregates inside those BCs.

One BC can hold multiple aggregates as well as multiple value objects (VOs).

An aggregate is an entity that hold some business notion.

A value object is different.. It does not have business value on it’s own, once this VO is associated with an aggregate then it has business meaning.

Each BC typically hold one root aggregate, many secondary aggregates and many VOs associated with these aggregates.