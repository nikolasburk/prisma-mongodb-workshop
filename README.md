# A Practical Introduction to Prisma

This repository contains the starter project for the **A Practical Introduction to Prisma with MongoDB** workshop by [Nikolas Burk](https://twitter.com/nikolasburk).

## Setup

### 1. Clone this repository

You can clone this repository with the following command:

```
git clone git@github.com:nikolasburk/prisma-mongodb-workshop.git
```

> Alternatively, you can also download the project via the GitHub UI. Click the green **Code**-button in the top-right corner and then click on **Download ZIP**.

### 2. Install dependencies

Navigate into the project directory and install the npm dependencies with the following command:

```
cd prisma-mongodb-workshop
npm install
```

### 3. Start local MongoDB instance via Docker

The `docker-compose.yml` file in this repo lets you spin up an instance of a MongoDB database via [Docker](https://www.docker.com/). Run the following command to start your MongoDB instance:

```
docker-compose up -d
```

# **A Practical Introduction to Prisma with MongoDB**

# Welcome

👋 Welcome to the **A Practical Introduction to Prisma with MongoDB** workshop.

# Resources

**This Github document**

https://pris.ly/mongodb-workshop

**GitHub starter project**

[`https://pris.ly/mongodb-workshop`](https://pris.ly/mongodb-workshop)

# Prerequisites

In order to successfully complete the tasks in the workshop, you should have:

- [Node.js](https://nodejs.org/en/) installed on your machine (12.2.X / 14.X)
- [Docker](https://www.docker.com) installed on your machine *or* a hosted MongoDB instance (e.g. on Atlas)
- It is recommended (but not required) to use [VS Code](https://code.visualstudio.com/) for the practical tasks

That's it 🙌 (*no prior knowledge about MongoDB or Prisma is required*)

# What you'll do

In this workshop, you'll learn about various workflows that are useful to know when using Prisma.

You'll start by setting up Prisma with a MongoDB database, learn about data modeling with Prisma and mapping your Prisma data model to MongoDB (*lesson 1*).

Then, you'll learn about Prisma Client, a type-safe *query builder* that can be used to query your database. You're going to explore various queries, from plain CRUD, to relation queries, to filters and pagination (*lesson 2*).

Next, you'll learn how you can use Prisma Client to implement the routes of a REST API (*lesson 3*).

Finally, we'll cover how you can use Prisma Client to implement the resolvers of a GraphQL API (*lesson 4*).


# Lessons

[1. Set up Prisma](1-setup-prisma.md)

[2. Explore Prisma Client](2-explore-prisma.md)

[3. REST API](3-rest-api.md)

[4. GraphQL API](4-graphql-api.md)

# What does a lesson look like?

A *lesson* is structured in two parts:

1. **Host walkthrough:** At the beginning of each *lesson*, your host will walk you through the different *tasks* you'll encounter in this lesson. Please *be attentive* during that time and follow the host's explanations to be sure that you can accomplish the tasks yourself when you're working on them later. **Do not code along or work on the tasks yourself yet!** Instead, you can think of questions or raise anything that you don't understand (e.g. in the **Q & A** section of Zoom).
2. **Do it yourself:** Once the host is done showing and explaining the different tasks, you get dedicated time to work on the tasks yourself!


# Want to host this workshop yourself?

Hosting workshops is incredibly fun! 😄 It's also a great way to deepen your understanding of the topics you're teaching and giving back to the community by sharing your knowledge.

The materials for this workshop are free to use and can be shared with anyone you know! If you want to host this workshop yourself and want some advice on how to get started, feel free to [reach out](mailto:burk@prisma.io).

# Join the Prisma Slack

Join over 4k developers on the [Prisma Discord](https://pris.ly/t/discord).