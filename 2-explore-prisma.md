# 2. Explore Prisma Client

## Goal

The goal of this lesson is to get comfortable with the [Prisma Client API](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference) and explore some of the available database queries you can send with it. You'll learn about CRUD queries, relation queries (like nested writes), filtering and pagination. Along the way, you will introduce a second model with a [*relation](https://www.prisma.io/docs/concepts/components/prisma-schema/relations)* to the `User` model that you created before.

## Setup

You can continue working in the same `prisma-mongodb-workshop` project that you set up in lesson 1. The `script.ts` file contains a `main` function that is invoked each time the script is executed. 

## Hints

- ***Type yourself*, don't copy and paste**
    
    To learn and really *understand* what you are doing for each task, be sure to **not copy and paste the solution** but type out the solution yourself (even if you have to look it up). 
    
- **Use the autocompletion**
    
    Prisma Client provides a number of queries that you can send to your database using its API. You can learn about these queries in the [documentation](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client) or explore the API right in your editor using *autocompletion*.
    
    To invoke the autocompletion, you can open `src/index.ts` and type following *inside* of the `main` function (you can delete the comment `// ... your Prisma Client queries will go here` that's currently there):
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await **prisma.** // autocompletion will show up if you type this
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    
    - Expand for a screenshot of the autocompletion
        
        !https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f01f172c-d7a8-42bc-b401-7b11fbcda36a/Untitled.png
        
    
    Once you typed the line `const result = await prisma.` ****into your editor, a little popup will be shown that lets you select the options for composing a query (e.g.  selecting a *model* you want to query or using another top-level function like `$queryRaw` or `$connect`). Autocompletion is available for the *entire* query, including any arguments that you might want to provide!
    
- **Visualize data in Prisma Studio**
    
    Prisma Studio is a GUI for your database that you can use to view and edit the data inside. You can start Prisma Studio by running the following command:
    
    ```tsx
    npx prisma studio
    ```
    

## Tasks

At the end of each task, you can run the script using the following command:

```bash
npm run dev
```

### Task 1: Write a query to return *all* `User` documents

To warm yourself up a bit, go and write a query to return *all* `User` documents from the database.  Print the result to the console using `console.log`.

- Solution
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await prisma.user.findMany()
    	****console.log(result)
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    

### **Task 2: Write a query to create a new** `User` ****document

In this task you'll create another `User` document. In your Prisma Client query, provide only a value for `email` but *not* for `name`:

- `email`: `"alice@prisma.io"`

Can you find the query that lets you do that? 

- Solution
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await prisma.user.create({
        data: {
          email: "alice@prisma.io"
        }
      })
    	console.log(result)
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    

### Task 3: Write a query to update an existing `User` document

In this task, you will update the `User` document you just created and add a value for its `name` field:

- `name`: `"Alice"`

How can you update an existing database document with Prisma Client?

- Solution
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await prisma.user.update({
        where: {
          email: "alice@prisma.io"
        },
        data: {
          name: "Alice"
        }
      })
    	console.log(result)
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    

### Task 4: Add a `Post` collection to your database

To explore more interesting Prisma Client queries, let's expand your Prisma schema with another model and configure a [*relation*](https://www.prisma.io/docs/concepts/components/prisma-schema/relations) between the existing and the new one.

The new `Post` model should be shaped as follows:

- `id`: an auto-incrementing integer to uniquely identify each post in the database
- `title`: the title of a post; this field should be *required* in the database
- `content`: the content/body of the post; this field should be *optional* in the database
- `published`: indicates whether a post is published or not; this field should be *required* in the database; by default any post that is created should *not* be published
- `author` and `authorId`: configures a *relation* from a post to a user who should be considered the author of the post; the relation should be *optional* so that a post doesn't necessarily need an author in the database; note that *all* relations in Prisma are bi-directional, meaning you'll need to add the second side of the relation on the already existing `User` model as well
- Solution
    
    ```graphql
    model User {
      id    String  @id @default(auto()) @map("_id") @db.ObjectId
      name  String?
      email String  @unique
      posts Post[]
    }
    
    model Post {
      id        String  @id @default(auto()) @map("_id") @db.ObjectId
      title     String
      content   String?
      published Boolean @default(false)
      author    User?   @relation(fields: [authorId], references: [id])
      authorId  String?
    }
    ```
    

Once you have adjusted the Prisma schema and your two models are in place, apply the changes against your database:

```graphql
npx prisma db push
```

### **Task 5: Write a query to create a new `Post`** document

In this task, you'll create a first `Post` document with the title `"Hello World"`. 

- Solution
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await prisma.post.create({
        data: {
          title: "Hello World"
        }
      })
    	console.log(result)
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    

### Task 6: Write a query to connect `User` and `Post` documents

You now have several `User` documents and exactly one `Post` document in the database, these can be connected via the `authorId` *reference field* in the database. 

When using Prisma Client, you don't need to manually set *reference fields* but you can configure relations using Prisma Client's type-safe API. Can you figure out how you can ***update*** the `Post` document and ***connect*** it to an existing `User` document via the `email` field?

Use the editor's autocompletion to find out about the query or read the [documentation](https://www.prisma.io/docs/concepts/components/prisma-client/relation-queries#connect-an-existing-record).

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.post.update({
        where: { id: "__OBJECT_ID__" },
        data: {
          author: {
            connect: { email: "alice@prisma.io" },
          },
        },
      });
      console.log(result);
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    

### Task 7: Write a query to retrieve a single `User` document by a unique value

In task 1, you learned how to fetch a list of documents from the database. In this task, you need to retrieve a single `User` document with a Prisma Client query by a *unique* value. 

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.user.findUnique({
        where: { email: "alice@prisma.io" }
      })
      console.log(result)
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    
    Note that you can use any *unique* field of a Prisma model to identify a record via the `where` argument, so in this case you could identify a `User` record by its `id` as well.
    

### Task 8: Write a query that selects only a subset of fields

For this task, you can reuse the same `findMany` query for users that you used in task 1. However, this time your goal is to only select a subset of the fields of the `User` model, specifically all returned objects should only contain the `id` and `name` **but *not* the `email` field. 

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.user.findMany({
        select: {
          id: true,
          name: true
        }
      })
    	console.log(result)
    }
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    

### Task 9: Write a nested query to *include* a relation in the result

You'll now start exploring more [relation queries](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client/relation-queries) of Prisma Client! Let's start with a nested read where you *include* a relation, concretely: Take your query from task 7 and include the relation to the `Post` collection in the result.

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.user.findUnique({
        where: { email: "alice@prisma.io" },
        include: { posts: true },
      });
      console.dir(result, { depth: null })}
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    
    Notice that the `result` of your query is fully typed! The type for it is generated on the fly by Prisma Client, here's what it looks like:
    
    ```tsx
    const result: (User & { posts: Post[]; }) | null
    
    // ... where the `Post` and `User` types look as follows:
    
    type Post = {
      id: number
      title: string
      content: string | null
      published: boolean
      authorId: number | null
    }
    
    type User = {
      id: number
      name: string | null
      email: string
    }
    ```
    

### Task 10: Write a nested write query to create a new `User` document with a new `Post` document

In this task, you'll create a new `User` along with a new `Post` document in a *single* Prisma Client (nested write) query. You can again use the autocompletion to figure out the right query or read the documentation [here](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client/relation-queries#nested-writes). 

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.user.create({
        data: {
          name: "Nikolas",
          email: "burk@prisma.io",
          posts: {
            create: { title: "A practical introduction to Prisma" },
          },
        },
      });
      console.log(result);
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    

### Task 11: Write a query that filters for users whose names start with "A"

For this task, you can again reuse the same `findMany` query for users that you used in task 1. Only that this time, you don't want to return *all* `User` documents, but only those that have a `name` which starts with the letter `"A"`. Can you find the right [operator](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference/#filter-conditions-and-operators) that lets you express this condition?

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.user.findMany({
        where: {
          name: {
            startsWith: "A",
          },
        },
      });
      console.log(result);
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    

### Task 12: Write a pagination query

Prisma Client provides several ways to paginate over a list of objects. Use the `findMany` query from before to return only the *third* and *fourth* `User` documents in the database.

- Solution
    
    ```tsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.user.findMany({
        skip: 2,
        take: 2,
      });
      console.log(result);
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    

### Task 13: Write an atomic update

For this task, you’ll need to adjust your Prisma schema again. Add a new field to the `Post` model:

- `viewCount`: tracks how often a post has been viewed, it should be *required* and use a default value of `0`
- Solution
    
    ```graphql
    model User {
      id    String  @id @default(auto()) @map("_id") @db.ObjectId
      name  String?
      email String  @unique
      posts Post[]
    }
    
    model Post {
      id        String  @id @default(auto()) @map("_id") @db.ObjectId
      title     String
      content   String?
      published Boolean @default(false)
      viewCount Int     @default(0)
      author    User?   @relation(fields: [authorId], references: [id])
      authorId  String?
    }
    ```
    

Once you have adjusted the Prisma schema and your new field is in place, apply the changes against your database:

```bash
npx prisma db push
```

Prisma Client supports *atomic operations* on integers. Write a Prisma Client query that updates an existing `Post` document by increasing the value of its `viewCount` by 1. Run the query several times to check that it worked.

- Solution
    
    ```jsx
    import { PrismaClient } from "@prisma/client";
    
    const prisma = new PrismaClient();
    
    async function main() {
      const result = await prisma.post.update({
        where: { id: "__OBJECT_ID__" },
        data: {
          viewCount: {
            increment: 1,
          },
        },
      });
      console.log(result);
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect());
    ```
    

### Task 14: Add a `Profile` embedded document

Prisma allows you to define an embedded document inside another record using composite types (known as embedded documents in MongoDB). Composite types have a similar structure to models.

For this task, you’ll need to adjust your Prisma schema again. 

Define a composite type by using the `type` keyword shape as follows:

- `imageURL`: the image URL of user

Update the `User` model by adding a new field to configure the embedded document and shape:

- `profile`: configures an embedded document in the `User` model. This field should not be required.
- Solution
    
    ```groovy
    **type Profile {
      imageUrl String
    }**
    
    model User {
      id      String   @id @default(auto()) @map("_id") @db.ObjectId
      name    String?
      email   String   @unique
      **profile Profile?**
      posts   Post[]
    }
    
    model Post {
      id        String  @id @default(auto()) @map("_id") @db.ObjectId
      title     String
      content   String?
      published Boolean @default(false)
      author    User?   @relation(fields: [authorId], references: [id])
      authorId  String?
    }
    ```
    

Once you have adjusted the Prisma schema and your two models are in place, apply the changes against your database:

```bash
npx prisma db push 
```

Next, write a query to update a `User` document and add a value for its `profile` field:

- `imageUrl` : [`https://randomuser.me/api/portraits/med/women/75.jpg`](https://randomuser.me/api/portraits/med/women/75.jpg)
- Solution
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await prisma.user.update({
        where: {
          email: "alice@prisma.io"
        },
        data: {
          profile: {
            imageUrl: 'https://randomuser.me/api/portraits/med/women/75.jpg'
          }
        }
      })
    	console.log(result)
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    

### Next steps

With these tasks, you only scratched the surface of what's possible with the Prisma Client API. Feel free to explore more queries and try out some of the ordering, upserts, raw MongoDB queries, or other features.

Next lesson: [3. REST API](3-rest-api.md)