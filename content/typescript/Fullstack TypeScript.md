---
title: Fullstack Typescript
draft: false
tags:
  - typescript
  - frontendmasters
  - course
---

Notes from Mike North's Fullstack TypeScript course.

Imagine a TS react app with a TS node server, and a relational DB of some sort:

<img src="https://www.typescript-training.com/static/dd387724950089be867964b4bfba4e2a/29114/fullstack-ts.001.png" >

Anytime here when we fetch data and get a response back, we have to assert that the payload is of a particular type. There's nothing that makes sure all of this stays in alignment except your own vigilance. 

In particular, let’s look at a couple of dangerous spots where we lack type checking

- `fetch('/api/*')`
    
    - The payload sent to the API from a client
    - The payload returned to the client from the API
- `sql.query('SELECT * from things')` (or some other equivalent thing)
    
    - The query sent to the DB
    - The result set returned to the API from the DB

Instead, we can create a system that looks more like this:
<img src="https://www.typescript-training.com/static/fcbbefd71d5d4f929bda66cdb8a88c43/29114/fullstack-ts.002.png" >

The green color represents one set of types that are shared between the client and the server. The green color represents one set of types that are shared across the client and server. And then another set of types that represent how our data is actually persisted in ur database - that's the purple color.

The green types (let’s call this “client representation”) help us in the client, along the API call, and even in some business logic on the sever.

The “purple types” (let’s call this the “db representation”) are also visible on the server and also the DB. 

There are 2 main schools of thought around how to have a single source of truth for your types in one place.

1) The schema. A single source of truth will be a GraphQL schema, and we'll generate TS code for our UI and API.
2) The interfaces. Tooling around this setup at time of course is less mature. Define TS types for our schema and generate the GraphQL part using a tool.

## GraphQL Intro

Problems that GQL Solves:
Let’s imagine we’re adding a “list of doctors” page to a hospital UI.

On the screen to the right, we see some evidence that there already exists some API endpoint for a weekly report that has way more detailed information about each doctor than we need for this table, but let’s say it’s missing the “availability” field that determines whether the “Book” button is enabled.

We’d have a few choices about how to proceed

1. **Use the existing endpoint as-is**, except that we add the “availability” field
2. We could **build a new endpoint that gives us just the “availability” field**, and fetch it in parallel with a call to the existing endpoint.
3. **Build an entirely new endpoint that returns all of the data we need**, just for this new page
4. **Modify the existing endpoint so that it operates in multiple “modes”** (maybe depending on a queryParam?)

None of these seem ideal in the long term.

**If we go with (1) we’re going to end up with an ever-growing list of “doctor” fields**. Some of these may also require new DB queries behind the scenes or send large amounts of unnecessary data over the wire, and we’ll pay that added price every time we need a list of doctors for any reason. This is sometimes referred to as over-fetching

**If we go with (2) we’re going to end up with a bunch of tiny little supplemental API endpoints**. Over time they’ll sort of “pile up” and it’ll be difficult to keep track of which one(s) are used and where. We may notice performance issues on client devices due to number of requests (HTTP/2 multiplexing mitigates this), more QPS hitting the API, and an increased tendency for race condition problems.

**If we go with (3) we’re going to end up with an extremely complicated API**, that significantly diverges from the RESTful model (which would seem to indicate that `GET /doctors` should be the single path where we get a list of doctors)

**If we go with (4) we’ll introduce some non-trivial complexity to the existing endpoint**, and introduce the need for more test cases. What happens when we have several orthogonal modes that all can be used in conjunction with each other? We'll end up with 2^N! scenarios since each scenario can be included or excluded. 2 states raised to the N. GraphQL can help us here.

Project Tour

There are three top level folders

- `/client` - A React frontend
- `/server` - A Node.js backend
- `/shared` - Code that can be imported by both the frontend and backend