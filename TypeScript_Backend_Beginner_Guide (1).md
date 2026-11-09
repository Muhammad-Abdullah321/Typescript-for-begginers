# TypeScript for Backend Development — Beginner Guide

## What this guide is about

If you already know basic JavaScript and Node.js, TypeScript can feel confusing at first because you suddenly see things like:

```ts
const name: string = "Abdullah";
```

The good news is that TypeScript is **JavaScript with a type system**.

In this guide, we will learn TypeScript through a real backend example:

> **A simple User Management API using Express + TypeScript + PostgreSQL**

We will build:

- `POST /api/users` — create a user
- `GET /api/users` — get all users
- `GET /api/users/:id` — get one user
- `PUT /api/users/:id` — update a user
- `DELETE /api/users/:id` — delete a user

The goal is not to memorize TypeScript syntax. The goal is to understand **why the syntax is used in real backend code**.

---

# 1. JavaScript vs TypeScript

You may already write JavaScript like this:

```js
const name = "Abdullah";
const age = 21;
```

In TypeScript, you can write:

```ts
const name: string = "Abdullah";
const age: number = 21;
```

The part after `:` is the type.

```text
name: string
age: number
```

It tells TypeScript:

> `name` should contain text, and `age` should contain a number.

This is invalid:

```ts
const age: number = "21";
```

because `"21"` is a string.

This is valid:

```ts
const age: number = 21;
```

## Important

TypeScript checks your code while developing. The final Node.js program still runs as JavaScript.

---

# 2. The basic types

## String

Used for text:

```ts
const username: string = "Abdullah";
const email: string = "abdullah@example.com";
```

## Number

Used for integers and decimals:

```ts
const age: number = 21;
const price: number = 1499.99;
```

## Boolean

Used for true/false:

```ts
const isActive: boolean = true;
```

## Array

An array of strings:

```ts
const skills: string[] = ["Node.js", "TypeScript", "PostgreSQL"];
```

An array of numbers:

```ts
const marks: number[] = [80, 75, 91];
```

---

# 3. Type inference — you don't always need to write the type

TypeScript can often figure out the type itself.

```ts
const name = "Abdullah";
const age = 21;
const isActive = true;
```

TypeScript understands:

```text
name      → string
age       → number
isActive  → boolean
```

So this is unnecessary:

```ts
const name: string = "Abdullah";
```

when TypeScript can already understand it.

## Beginner rule

Don't add types everywhere just because you can.

Add them when they make your code clearer or when TypeScript cannot infer what you mean.

---

# 4. Objects

In JavaScript:

```js
const user = {
    name: "Abdullah",
    age: 21,
    email: "abdullah@example.com"
};
```

You can describe the object in TypeScript:

```ts
const user: {
    name: string;
    age: number;
    email: string;
} = {
    name: "Abdullah",
    age: 21,
    email: "abdullah@example.com"
};
```

But this becomes annoying if you have many users.

That's where **interfaces** help.

---

# 5. Interfaces

An interface describes the shape of an object.

```ts
interface User {
    id: number;
    name: string;
    email: string;
    age: number;
}
```

Now you can create a user:

```ts
const user: User = {
    id: 1,
    name: "Abdullah",
    email: "abdullah@example.com",
    age: 21
};
```

If you accidentally write:

```ts
const user: User = {
    id: 1,
    name: "Abdullah",
    email: "abdullah@example.com",
    age: "21"
};
```

TypeScript reports an error because `age` must be a number.

## Real backend example

Suppose your API returns:

```json
{
    "id": 1,
    "name": "Abdullah",
    "email": "abdullah@example.com",
    "age": 21
}
```

You can describe that response with:

```ts
interface User {
    id: number;
    name: string;
    email: string;
    age: number;
}
```

Now your editor and compiler understand what a `User` looks like.

---

# 6. Optional properties

Sometimes a value isn't required.

For example, a user may or may not have a phone number.

```ts
interface User {
    id: number;
    name: string;
    email: string;
    phone?: string;
}
```

The `?` means:

> This property is optional.

Both are valid:

```ts
const user1: User = {
    id: 1,
    name: "Ali",
    email: "ali@example.com"
};
```

And:

```ts
const user2: User = {
    id: 2,
    name: "Ahmed",
    email: "ahmed@example.com",
    phone: "03001234567"
};
```

---

# 7. Functions

JavaScript:

```js
function add(a, b) {
    return a + b;
}
```

TypeScript:

```ts
function add(a: number, b: number): number {
    return a + b;
}
```

There are three important pieces:

```ts
a: number
b: number
): number
```

The first two describe the parameters.

The last one describes what the function returns.

Example:

```ts
function calculatePrice(price: number, quantity: number): number {
    return price * quantity;
}

const total = calculatePrice(500, 3);
```

`total` is a number.

---

# 8. Arrow functions

You will see these constantly in Node.js:

```ts
const add = (a: number, b: number): number => {
    return a + b;
};
```

A backend example:

```ts
const greetUser = (name: string): string => {
    return `Hello ${name}`;
};
```

---

# 9. `void`

If a function doesn't return a value:

```ts
function printMessage(message: string): void {
    console.log(message);
}
```

`void` means:

> This function is not returning a useful value.

You'll see `Promise<void>` frequently in Express controllers.

---

# 10. Union types

Sometimes a value can be one of several types.

```ts
let userId: number | string;

userId = 10;
userId = "10";
```

The `|` means **OR**.

A very useful backend example is status:

```ts
type UserStatus = "active" | "inactive";
```

Now:

```ts
let status: UserStatus = "active";
```

This is valid:

```ts
status = "inactive";
```

But this is invalid:

```ts
status = "deleted";
```

This is useful because it prevents accidental values.

---

# 11. `null` and `undefined`

These two values are important in backend development.

```ts
let phone: string | null = null;
```

This means:

> `phone` can contain a string or it can be `null`.

For example, a database might return:

```text
phone = NULL
```

So your TypeScript type may be:

```ts
phone: string | null;
```

---

# 12. `async`, `await`, and Promise

You probably already know JavaScript:

```js
async function getUser() {
    const user = await database.findUser();
    return user;
}
```

TypeScript can describe the returned Promise:

```ts
async function getUser(): Promise<User> {
    const user = await database.findUser();
    return user;
}
```

If the function doesn't return data:

```ts
async function deleteUser(): Promise<void> {
    // delete something
}
```

Think of it simply:

```text
Promise<User>
    ↓
Eventually returns a User

Promise<void>
    ↓
Eventually finishes without returning useful data
```

---

# 13. `interface` vs `type`

You will see both.

Interface:

```ts
interface User {
    id: number;
    name: string;
}
```

Type:

```ts
type User = {
    id: number;
    name: string;
};
```

For now, don't overthink the difference.

A practical beginner rule:

- Use `interface` for object shapes.
- Use `type` when defining unions or composing types.

For example:

```ts
type UserStatus = "active" | "inactive";
```

---

# 14. Generics — understand the idea first

Generics look scary at first.

Example:

```ts
function getFirst<T>(items: T[]): T {
    return items[0];
}
```

The `T` means:

> I don't know the type yet. Let the caller provide it.

For strings:

```ts
const firstName = getFirst<string>(["Ali", "Ahmed"]);
```

For numbers:

```ts
const firstNumber = getFirst<number>([10, 20, 30]);
```

You will see generics when using libraries such as database clients.

Don't worry about mastering advanced generics at the beginning.

---

# 15. Now build a real API

We will build:

```text
User Management API
```

Technology:

```text
Node.js
Express
TypeScript
PostgreSQL
```

The request flow will be:

```text
HTTP Request
      ↓
Route
      ↓
Controller
      ↓
PostgreSQL
      ↓
HTTP Response
```

---

# 16. Create the project

Open a terminal:

```bash
mkdir typescript-api
cd typescript-api
npm init -y
```

Install Express, PostgreSQL client, and dotenv:

```bash
npm install express pg dotenv
```

Install TypeScript tools:

```bash
npm install -D typescript ts-node-dev @types/node @types/express
```

Create TypeScript configuration:

```bash
npx tsc --init
```

---

# 17. Project structure

Start with:

```text
typescript-api/
│
├── src/
│   ├── config/
│   │   └── database.ts
│   │
│   ├── controllers/
│   │   └── user.controller.ts
│   │
│   ├── routes/
│   │   └── user.routes.ts
│   │
│   ├── types/
│   │   └── user.types.ts
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── package.json
└── tsconfig.json
```

Don't worry if this structure looks unfamiliar.

We will build it one file at a time.

---

# 18. Configure TypeScript

A simple `tsconfig.json`:

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "commonjs",
        "rootDir": "./src",
        "outDir": "./dist",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true
    }
}
```

The most important setting for now is:

```json
"strict": true
```

It makes TypeScript stricter and helps catch mistakes.

---

# 19. Set up PostgreSQL

Create a database:

```sql
CREATE DATABASE typescript_api;
```

Then create the `users` table:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    age INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Our table:

```text
users
--------------------------------
id
name
email
age
created_at
```

Example record:

```text
1 | Abdullah | abdullah@example.com | 21 | 2026-09-14
```

---

# 20. Environment variables

Create `.env`:

```env
PORT=5000

DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_password
DB_NAME=typescript_api
```

Why use `.env`?

Because you shouldn't put passwords directly into your source code.

Bad:

```ts
const password = "mypassword123";
```

Better:

```ts
process.env.DB_PASSWORD
```

Also add `.env` to `.gitignore`:

```text
node_modules/
dist/
.env
```

Never push database passwords to GitHub.

---

# 21. Connect PostgreSQL

Create:

```text
src/config/database.ts
```

Add:

```ts
import { Pool } from "pg";
import dotenv from "dotenv";

dotenv.config();

const pool = new Pool({
    host: process.env.DB_HOST,
    port: Number(process.env.DB_PORT),
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME
});

export default pool;
```

Let's understand the important part.

```ts
import { Pool } from "pg";
```

We import PostgreSQL's connection pool.

Then:

```ts
const pool = new Pool({
```

creates the connection pool.

This:

```ts
port: Number(process.env.DB_PORT)
```

converts the environment variable into a number.

Environment variables are normally strings.

So:

```text
"5432"
```

becomes:

```text
5432
```

---

# 22. Create our TypeScript types

Create:

```text
src/types/user.types.ts
```

First define what a database user looks like:

```ts
export interface User {
    id: number;
    name: string;
    email: string;
    age: number | null;
    created_at: Date;
}
```

Now define the data needed when creating a user:

```ts
export interface CreateUserRequest {
    name: string;
    email: string;
    age?: number;
}
```

Why are these different?

The database creates:

```text
id
created_at
```

The client doesn't need to send them.

A request might contain:

```json
{
    "name": "Abdullah",
    "email": "abdullah@example.com",
    "age": 21
}
```

The database response might contain:

```json
{
    "id": 1,
    "name": "Abdullah",
    "email": "abdullah@example.com",
    "age": 21,
    "created_at": "2026-09-14T..."
}
```

That's why having separate types is useful.

---

# 23. Create the controller

Create:

```text
src/controllers/user.controller.ts
```

Add:

```ts
import { Request, Response } from "express";
import pool from "../config/database";
import { CreateUserRequest, User } from "../types/user.types";

export const createUser = async (
    req: Request,
    res: Response
): Promise<void> => {

    const { name, email, age }: CreateUserRequest = req.body;

    const result = await pool.query<User>(
        `
        INSERT INTO users (name, email, age)
        VALUES ($1, $2, $3)
        RETURNING *
        `,
        [name, email, age ?? null]
    );

    const user = result.rows[0];

    res.status(201).json({
        success: true,
        data: user
    });
};
```

This may look complicated.

Let's break it down.

---

# 24. Understanding `Request` and `Response`

Express normally has:

```js
(req, res) => {}
```

TypeScript lets us specify:

```ts
(req: Request, res: Response)
```

This tells TypeScript:

```text
req → Express Request
res → Express Response
```

Your editor can now give better autocomplete and TypeScript can catch mistakes.

---

# 25. Understanding `CreateUserRequest`

This:

```ts
const { name, email, age }: CreateUserRequest = req.body;
```

means:

> We expect the request body to contain the properties described by `CreateUserRequest`.

So TypeScript knows:

```text
name  → string
email → string
age   → number | undefined
```

---

# 26. Understanding the SQL

This:

```sql
INSERT INTO users (name, email, age)
VALUES ($1, $2, $3)
RETURNING *
```

inserts a new user.

And:

```ts
[name, email, age ?? null]
```

provides the values.

For example:

```text
$1 → Abdullah
$2 → abdullah@example.com
$3 → 21
```

The `$1`, `$2`, `$3` placeholders are important because they help prevent SQL injection.

---

# 27. Why `age ?? null`?

Our type says:

```ts
age?: number;
```

So age might be:

```text
number
```

or:

```text
undefined
```

But PostgreSQL can use `NULL`.

Therefore:

```ts
age ?? null
```

means:

> If age is missing/null, use `null`.

---

# 28. Create the GET API

Add to the controller:

```ts
export const getUsers = async (
    req: Request,
    res: Response
): Promise<void> => {

    const result = await pool.query<User>(
        "SELECT * FROM users ORDER BY id DESC"
    );

    res.json({
        success: true,
        data: result.rows
    });
};
```

Because we wrote:

```ts
pool.query<User>
```

TypeScript understands that the returned rows are:

```text
User[]
```

That means:

```text
an array of User objects
```

---

# 29. Get one user

Add:

```ts
export const getUserById = async (
    req: Request,
    res: Response
): Promise<void> => {

    const id = Number(req.params.id);

    const result = await pool.query<User>(
        "SELECT * FROM users WHERE id = $1",
        [id]
    );

    if (result.rows.length === 0) {
        res.status(404).json({
            success: false,
            message: "User not found"
        });

        return;
    }

    res.json({
        success: true,
        data: result.rows[0]
    });
};
```

If the client calls:

```text
GET /api/users/5
```

then:

```ts
req.params.id
```

is `"5"`.

Notice it is a string.

So we convert it:

```ts
Number(req.params.id)
```

Now we have:

```text
5
```

as a number.

---

# 30. Update a user

Add:

```ts
export const updateUser = async (
    req: Request,
    res: Response
): Promise<void> => {

    const id = Number(req.params.id);

    const { name, email, age }: CreateUserRequest = req.body;

    const result = await pool.query<User>(
        `
        UPDATE users
        SET name = $1,
            email = $2,
            age = $3
        WHERE id = $4
        RETURNING *
        `,
        [name, email, age ?? null, id]
    );

    if (result.rows.length === 0) {
        res.status(404).json({
            success: false,
            message: "User not found"
        });

        return;
    }

    res.json({
        success: true,
        data: result.rows[0]
    });
};
```

---

# 31. Delete a user

Add:

```ts
export const deleteUser = async (
    req: Request,
    res: Response
): Promise<void> => {

    const id = Number(req.params.id);

    const result = await pool.query<User>(
        "DELETE FROM users WHERE id = $1 RETURNING *",
        [id]
    );

    if (result.rows.length === 0) {
        res.status(404).json({
            success: false,
            message: "User not found"
        });

        return;
    }

    res.json({
        success: true,
        message: "User deleted successfully"
    });
};
```

Now we have all five operations.

---

# 32. Create the routes

Create:

```text
src/routes/user.routes.ts
```

```ts
import { Router } from "express";

import {
    createUser,
    getUsers,
    getUserById,
    updateUser,
    deleteUser
} from "../controllers/user.controller";

const router = Router();

router.post("/", createUser);

router.get("/", getUsers);

router.get("/:id", getUserById);

router.put("/:id", updateUser);

router.delete("/:id", deleteUser);

export default router;
```

The route's job is simple:

> Decide which controller should handle each URL.

For example:

```ts
router.post("/", createUser);
```

means:

```text
POST /api/users
       ↓
createUser()
```

---

# 33. Create the Express app

Create:

```text
src/app.ts
```

```ts
import express from "express";
import userRoutes from "./routes/user.routes";

const app = express();

app.use(express.json());

app.use("/api/users", userRoutes);

export default app;
```

This:

```ts
app.use(express.json());
```

allows Express to read JSON request bodies.

For example:

```json
{
    "name": "Abdullah",
    "email": "abdullah@example.com",
    "age": 21
}
```

Then you can access it using:

```ts
req.body
```

---

# 34. Start the server

Create:

```text
src/server.ts
```

```ts
import app from "./app";
import dotenv from "dotenv";

dotenv.config();

const PORT = Number(process.env.PORT) || 5000;

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```

---

# 35. Add npm scripts

In `package.json`:

```json
{
    "scripts": {
        "dev": "ts-node-dev --respawn --transpile-only src/server.ts",
        "build": "tsc",
        "start": "node dist/server.js"
    }
}
```

Run the development server:

```bash
npm run dev
```

You should see:

```text
Server running on port 5000
```

---

# 36. Test the API

You can use Postman, Insomnia, Thunder Client, or another API client.

## Create user

Request:

```text
POST http://localhost:5000/api/users
```

Body:

```json
{
    "name": "Abdullah",
    "email": "abdullah@example.com",
    "age": 21
}
```

Expected response:

```json
{
    "success": true,
    "data": {
        "id": 1,
        "name": "Abdullah",
        "email": "abdullah@example.com",
        "age": 21,
        "created_at": "2026-09-14T..."
    }
}
```

---

# 37. Get all users

Request:

```text
GET http://localhost:5000/api/users
```

Example response:

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Abdullah",
            "email": "abdullah@example.com",
            "age": 21
        }
    ]
}
```

---

# 38. Get one user

Request:

```text
GET http://localhost:5000/api/users/1
```

---

# 39. Update a user

Request:

```text
PUT http://localhost:5000/api/users/1
```

Body:

```json
{
    "name": "Muhammad Abdullah",
    "email": "abdullah@example.com",
    "age": 22
}
```

---

# 40. Delete a user

Request:

```text
DELETE http://localhost:5000/api/users/1
```

Response:

```json
{
    "success": true,
    "message": "User deleted successfully"
}
```

---

# 41. The complete request flow

Suppose you send:

```text
POST /api/users
```

with:

```json
{
    "name": "Abdullah",
    "email": "abdullah@example.com",
    "age": 21
}
```

The request travels like this:

```text
Client
  │
  │ POST /api/users
  ▼
Express
  │
  ▼
user.routes.ts
  │
  ▼
createUser()
  │
  ▼
PostgreSQL
  │
  ▼
User created
  │
  ▼
JSON response
```

This is the important mental model.

---

# 42. What TypeScript is doing for us

Look at this:

```ts
interface User {
    id: number;
    name: string;
    email: string;
    age: number | null;
    created_at: Date;
}
```

You have created a contract:

```text
A User must have:
id          → number
name        → string
email       → string
age         → number or null
created_at  → Date
```

Then:

```ts
pool.query<User>()
```

tells TypeScript:

> The database rows are expected to represent Users.

And:

```ts
(req: Request, res: Response)
```

tells TypeScript:

> These are Express request and response objects.

So TypeScript is helping you understand the data moving through your application.

---

# 43. Very important: TypeScript is NOT runtime validation

This is a common beginner misunderstanding.

You might write:

```ts
interface CreateUserRequest {
    name: string;
    email: string;
    age?: number;
}
```

You might think this prevents a client from sending:

```json
{
    "name": 123,
    "email": true,
    "age": "hello"
}
```

It doesn't.

Why?

Because the client is external.

TypeScript cannot control what an external client sends.

For production APIs, you need **runtime validation** as well.

A popular choice is Zod.

The idea becomes:

```text
Incoming request
       ↓
Runtime validation
       ↓
TypeScript/business logic
       ↓
Database
```

This is something you should learn after becoming comfortable with the basics above.

---

# 44. What to learn next

Don't jump into advanced TypeScript immediately.

For your Node.js backend path, learn in this order:

## Stage 1 — TypeScript basics

Learn:

- `string`
- `number`
- `boolean`
- arrays
- objects
- interfaces
- optional properties
- union types
- `null` / `undefined`
- functions
- `async` / `Promise`

## Stage 2 — TypeScript for Express

Learn:

- `Request`
- `Response`
- typing request bodies
- typing params
- typing query parameters
- middleware types
- error handling

## Stage 3 — Database

Learn:

- PostgreSQL
- SQL
- joins
- indexes
- constraints
- transactions
- migrations
- connection pooling

## Stage 4 — Production API

Learn:

- Zod
- centralized error handling
- authentication
- JWT
- password hashing
- authorization
- pagination
- logging
- environment configuration

## Stage 5 — Better architecture

Move from:

```text
Route
 ↓
Controller
 ↓
Database
```

toward:

```text
Route
 ↓
Controller
 ↓
Service
 ↓
Repository
 ↓
Database
```

---

# 45. A realistic learning exercise

Don't just copy the User API.

After you understand it, change it.

Create a **Job Application API**.

For example:

```text
POST   /api/jobs
GET    /api/jobs
GET    /api/jobs/:id
PUT    /api/jobs/:id
DELETE /api/jobs/:id
```

A job might look like:

```ts
interface JobApplication {
    id: number;
    company: string;
    role: string;
    status: JobStatus;
    appliedAt: Date;
    resumeName: string;
}
```

And:

```ts
type JobStatus =
    | "applied"
    | "interview"
    | "rejected"
    | "ghosted"
    | "offer";
```

That forces you to actually use:

- interfaces
- types
- arrays
- functions
- Express
- PostgreSQL
- SQL
- TypeScript
- API design

rather than just watching tutorials.

---

# 46. TypeScript cheat sheet

```ts
// String
const name: string = "Abdullah";

// Number
const age: number = 21;

// Boolean
const active: boolean = true;

// Array
const skills: string[] = ["Node.js", "TypeScript"];

// Object
interface User {
    id: number;
    name: string;
}

// Optional property
interface User {
    phone?: string;
}

// Union
let id: number | string;

// Specific values
type Status = "active" | "inactive";

// Nullable
let phone: string | null = null;

// Function
function add(a: number, b: number): number {
    return a + b;
}

// Async function
async function getUser(): Promise<User> {
    // ...
}

// No useful return
function logMessage(message: string): void {
    console.log(message);
}
```

---

# 47. The main idea to remember

Don't think:

> "I have to learn a huge new programming language called TypeScript."

Think:

> "I already know JavaScript. TypeScript lets me describe what kind of data my JavaScript code is working with."

For backend development, the most important transition is:

```text
JavaScript

const user = req.body;
```

to:

```text
TypeScript

const user: CreateUserRequest = req.body;
```

and:

```text
Database result
      ↓
User[]
```

instead of:

```text
Database result
      ↓
¯\_(ツ)_/¯
```

That's the practical value of TypeScript.

---

# Suggested practice

Build the User API yourself **without copying the controller**.

Try to recreate:

1. PostgreSQL connection
2. `User` interface
3. `CreateUserRequest` interface
4. `POST /users`
5. `GET /users`
6. `GET /users/:id`
7. `PUT /users/:id`
8. `DELETE /users/:id`

If you get stuck, look at the original example and understand **why** the syntax is there rather than memorizing it.

After that, the next useful project is a **Job Application API using Service + Repository + Controller architecture**, because it will take you from basic TypeScript syntax toward the kind of structure used in real backend projects.
