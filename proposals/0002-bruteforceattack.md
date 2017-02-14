# Feature name

* Proposal: 0001-after-find
* Authors: [Jonas DB](https://github.com/jonas-db)
* Review Manager: TBD
* Status: **Awaiting review**

## Introduction
## Motivation

Assume that we want to know the amount of views of a blog post, so everytime a blog post is fetched with a GET request, the amount of views should be updated.

## Proposed solution

Introducing an afterFind cloud code function. This is similar to the existing triggers and provides a uniform way to have triggers for GET/POST/DELETE requests.

## Detailed design

Extending the cloud code with an afterFind function

## Alternatives considered

If i'm correct, there are two ways to solve this problem:

create a cloud function, which you call instead of querying a collection
after the fetch request, save the object again with the counter increased

While the first one is acceptable, the second is not. Although, I think that cloud code is meant for custom code, not for normal query requests. To remain consistent with the other cloud code, I propose the suggestion of afterFind. 
