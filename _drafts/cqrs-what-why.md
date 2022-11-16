---
layout: post
title: CQRS - What and Why?
tags: cqrs
---

CQRS is really simple. Sometimes the idea gets conflated with other concepts such as Event Sourcing and DDD, but that is not CQRS. So, what is it then?

## The Original CQS

Originally coined by Bertrand Meyer, **Command Query Separation (CQS)** (without the R) is a principle that says all methods should either be queries that read data or commands that write data, but never both.

Or, my favorite interpretation, *"Asking a question should not change the answer"*.

## Dequeue or Pop

An example of violating CQS in the .NET Framework is the `Queue.Dequeue` method, or `Stack.Pop`. These methods return a result *and* change the state of the queue/stack.

![Dequeue method with return value](/images/diagrams/cqrs-dequeue-1.svg)

If we wanted a design that follows CQS:
1. The `Dequeue` method should return `void`. This makes it a **command**.
2. We should use the `Peek` method to read the next item. This is a **query** that does not change the state of the queue.

![Dequeue command and separate Peek query](/images/diagrams/cqrs-dequeue-2.svg)

## The R - Responsibility

**Command Query Responsibility Segregation (CQRS)** takes this concept a step further by treating reads and writes as entirely separate subsystems. Top to bottom. Not just methods, but different classes that can have different models for the same data. This is different from a CRUD design, where read and write to an entity using the same object structure.

![CQRS system with a single database](/images/diagrams/cqrs-single-database.svg)

This approach supports a more task-based design, which is sometimes more natural that trying to force one unified model for both reading and writing.

For example, when registering a new user, we provide the following properties.

```csharp
public class RegisterUserCommand
{
    public string Email { get; set; }

    public string Password { get; set; }

    public DateTime DateOfBirth { get; set; }
}
```

But the model we get from querying this data looks quite different. Our user now has a `UserId` which we can return. And we don't want to return the password.

```csharp
public class UserProjection
{
    public int UserId { get; set; }

    public string Email { get; set; }

    public int Age { get; set; }
}
```

It's easy to imagine other ways we might want to interact with this same data, such as a `LoginCommand` or a `CountActiveUsersQuery`. It doesn't seem intuitive to think of these in a CRUD-like manner. In this case, applying CQRS simplifies the design.

And that's all CQRS is; designing two separate subsystems for reading and writing the same data.

## Optimising Commands and Queries

Commands and queries are very different beasts. Particularly at scale. Splitting them up allows each subsystem to evolve independently.

Optimising query performance usually involves some kind of caching or pre-computation.

- **Caching** results of a query in memory so to reduce load on the database is particularly useful when reading the data is far more common than writing it.
- **Denormalised data structures** can mean simpler SQL `SELECT` statements with fewer joins.
- Database **indexes** 
- Setting up secondary read-only **replicas** of the database to take the load of the primary server.

Queries are naturally idempotent and safe to replay if they experience a temporary outage.

Writing data, on the other hand, brings very different problems to solve. What do we do if you miss the response of a non-idempotent command? What happens if multiple concurrent processes try to modify the same data? Do we have to write changes to all replica databases?

Common solutions to improve performance of commands are:

- Making commands **asynchronous**, just returning `202 Accepted`, *"I promise to do that later"*.
- **Sharding** the database. Storing partitions of data on different servers.

....transactions

## Eventual Consistency

.....

CAP theorem. If we want a distributed system to be highly available, we must accept that it may not be immediately consistent.

## Separating Persistence

Some applications even have different databases tailored to reading and writing.

![CQRS system with separate read and write stores](/images/diagrams/cqrs-separate-stores.svg)


## Relationship to Other Concepts

CQRS is often combined with Domain-Driven Design and Event Sourcing. Neither of these are requirements of CQRS, and CQRS is not a requirement for those either, but they share some common ground. I'll write a follow up post on their similarities and why I believe they are often used together.