---
layout: post
title: What is CQRS?
tags: cqrs
notes:
  - adsf
---

CQRS is really simple. Sometimes the idea gets conflated with other concepts such as Event Sourcing and DDD, but that is not CQRS. So, what is it then?

# The Original CQS

Originally coined by Bertrand Meyer, **Command Query Separation (CQS)** is a principle that says all methods should either read or write, but never both.

> 

Or, my favorite interpretation, *"Asking a question should not change the answer"*.

## But why?

Getting data and changing it have very different problems to solve. Particularly at scale.

Optimising query performance might involve some layer of caching or pre-computation, for example. At the database level, you might need to set up certain indexes, or denormalise the database structure. You could set up replica secondary databases to read from.

Queries are naturally idempotent and safe to replay if they experience a temporary outage.

Writing data, on the other hand, brings very different problems to solve. Retries might not be as simple if the command isn't idempotent. We may experience concurrency problems. We can't just write to a secondary replica of the data because it might get lost.

One way is to make a command asynchronous, just returning `202 Accepted`, *"I promise to do that later"*, and accepting data may not be immediately consistent when querying it later.

## Dequeue or Pop

An example of violating CQS in the .NET Framework is the `Queue.Dequeue` method, or `Stack.Pop`. These methods return a result *and* change the state of the queue/stack.

# The R - Responsibility

CQRS takes CQS a step further.