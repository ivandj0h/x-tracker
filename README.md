# X Tracker Application

## Description

This is a simple web-based expense tracker application that allows users to
record, categorize, and visualize their expenses.

## Getting Started

### The Stacks

This Application use several stacks, such as :

- Nextjs 14: API routes and ServerActions
- Auth with Clerk
- Typescript
- Tailwindcss / Shadcn UI
- SQLite / Vercel PostgreSQL
- Prisma as ORM
- React-query
- Recharts

## The Prisma Models

Here is the `Prisma Schema` that i use to generate the `SQL Queries` of Databse and Tables

```
model UserSettings {
  userId   String @id
  currency String
}

model Category {
  createdAt DateTime @default(now())
  name      String
  userId    String
  icon      String
  type      String   @default("income")

  @@unique([name, userId, type])
}

model Transaction {
  id        String   @id @default(uuid())
  createdAt DateTime @default(now())
  updateAt  DateTime @default(now())

  amount      Float
  description String
  date        DateTime
  userId      String
  type        String   @default("income")

  category     String
  categoryIcon String
}

model MonthHistory {
  userId  String
  day     Int
  month   Int
  year    Int
  income  Float
  expense Float

  @@id([day, month, year, userId])
}

model YearHistory {
  userId  String
  month   Int
  year    Int
  income  Float
  expense Float

  @@id([month, year, userId])
}
```

To generate the Queries, just typing this command 

```
npx prisma migrate dev --name add_user_settings

```

find the `migrations` folder under `prisma` folder, then find
the `migration.sql` file, and here what you get :

```
-- CreateTable
CREATE TABLE "UserSettings" (
    "userId" TEXT NOT NULL PRIMARY KEY,
    "currency" TEXT NOT NULL
);

-- CreateTable
CREATE TABLE "Category" (
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "name" TEXT NOT NULL,
    "userId" TEXT NOT NULL,
    "icon" TEXT NOT NULL,
    "type" TEXT NOT NULL DEFAULT 'income'
);

-- CreateTable
CREATE TABLE "Transaction" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updateAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "amount" REAL NOT NULL,
    "description" TEXT NOT NULL,
    "date" DATETIME NOT NULL,
    "userId" TEXT NOT NULL,
    "type" TEXT NOT NULL DEFAULT 'income',
    "category" TEXT NOT NULL,
    "categoryIcon" TEXT NOT NULL
);

-- CreateTable
CREATE TABLE "MonthHistory" (
    "userId" TEXT NOT NULL,
    "day" INTEGER NOT NULL,
    "month" INTEGER NOT NULL,
    "year" INTEGER NOT NULL,
    "income" REAL NOT NULL,
    "expense" REAL NOT NULL,

    PRIMARY KEY ("day", "month", "year", "userId")
);

-- CreateTable
CREATE TABLE "YearHistory" (
    "userId" TEXT NOT NULL,
    "month" INTEGER NOT NULL,
    "year" INTEGER NOT NULL,
    "income" REAL NOT NULL,
    "expense" REAL NOT NULL,

    PRIMARY KEY ("month", "year", "userId")
);

-- CreateIndex
CREATE UNIQUE INDEX "Category_name_userId_type_key" ON "Category"("name", "userId", "type");

```

that's the `Queries`.

## Development

To run this application, first download this repository by clicking the "Code" button on GitHub and selecting "Download
ZIP". Alternatively, you can open your terminal or command prompt and type:

```bash
git clone https://github.com/ivandi1980/x-tracker.git
```

Once the repository is downloaded to your local machine, the first step is to find the `.env.example` file and rename it
to `.env.local` Then, open the `.env.local` file and fill in the following details:

```
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=

NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/wizard

POSTGRES_URL=
POSTGRES_PRISMA_URL=
POSTGRES_URL_NO_SSL=
POSTGRES_URL_NON_POOLING=
POSTGRES_USER=
POSTGRES_HOST=
POSTGRES_PASSWORD=
POSTGRES_DATABASE=

```

You don't need to fill in the Postgres details because it will run locally using SQLite.

Since the authentication in this application uses the Clerk provider, you need to have credentials from Clerk to use
this application. If you don't have Clerk credentials, please visit the official Clerk website at

```

https://clerk.com/ 
```

and create a project there for free.
Once you have obtained the credentials, simply enter them into the `.env.local` file in this application.
Next, run the command 
```
yarn install 
```
to install all the `dependencies` needed for the application to run.

After installing the dependencies, run the command 
```
yarn dev 
```
in your terminal or command prompt, then open your browser and go to:

```
http://localhost:3000
```

## Screen Shoot

Here are some of the Screen shoot of this application :

### Setup The Currency
You have to setup the `currency` before using this Application,
you can type `IDR` for Rupiah, or leaving it to default is `USD`

![Currency](/public/images/create_currency.png)

### Category
Here you can create the `Categories` fo this Application

![Categories](/public/images/create_category.png)

### Dashboard - Overview
Here is the `Dashboard - Overview` page, you can manage transaction here

![Currency](/public/images/dashboard_overview.png)

### Dashboard - History

Here is the `Dashboard - History` page, you can find Chart of your transaction here

![Currency](/public/images/dashboard_history.png)


### Create Transaction
Here in this page, you can `Create Transaction`

![Currency](/public/images/create_transaction.png)

### Create Expenses

Here in this page, you can `Create Expenses`

Thank you!

Credits :

[ivandjoh](https://www.linkedin.com/in/ivandjoh/)
