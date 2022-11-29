---
layout: post
title: CQRS Command Return Values
tags: cqrs
---

You may have heard that commands in CQRS should not return anything.

## Returning void

In C#, it's simple to think a command must return `void`. But what does that mean?

The fact that the method has returned means it has completed something. Does that mean the command has been handled? The method could just hang until processing has completed.

All methods can throw exceptions too, which is returning some kind of information.

The key is, in both cases we are not returning **business data**, but returning **information about the command execution**. The domain model remains fully encapsulated by the commands, and business data is only returned through our query projections.

## Asynchronous or Queued

A `Task` returned from a C# async method does a very similar thing to `void`. It tells us whether the command has actually completed, and if there was an error.

...

Whereas dropping the command on a message queue will not indicate if the command has actually been handled yet.

## Transactions and half done

## Rows affected

NonQuery

## Auto-incremented ID

- GUID