**Independant deployebility**

Very core disadvantage of monolith is that it is very large. So it becomes dificult to update , test build and deploy it. That’s why we decompose it into smaller services, i.e. microservices.

Now if we need to deploy all these microservices at once, then it becomes another difficult task. That is why microservices need to be able to deploy independant of each other.

To achieve this, we need to define very stable contracts between services, and internal state of the services should be very well encapsulated behind these cotracts.

**Modeling based on Business domain**

When a new feature or requirement comes, it affects a perticular set of functionality. e.g. in ERP, update in employee payroll will rarely affect something far away like, customer onboarding. So instead of monolith, we arrange teams and microservices around a certain piece of business domain.

This reduces features which involve changes across microservices. Changes across microservices are expensive. You have to carefully coordinate the development and deployment across teams.

So we design microservices such a way that, each microservice owns a piece of business end to end i.e. UI, API and data. This way there is minimal chance of cross team communication.

**Owning there state**

Each microservice should completely own it’s state i.e. DB. If another microservice wants to acess that state, it has to go through API of the owner service. This reduces coupling between services. Zlso this promotes encapsulation.

**Size**

How big a microservice should be? It is relative. Only factor you need to consider is the number of microservices coming out of refactoring the monolith. Very high number of microservices will lead to again unmanagable code base, since microservices themself add some complexity .