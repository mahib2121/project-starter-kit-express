# Express + Prisma Project Starter

A clean and scalable starter template for building REST APIs with **Node.js, Express.js, TypeScript, Prisma ORM, and PostgreSQL**.

This starter is designed to provide a solid foundation for backend projects with database integration, environment configuration, migrations, and a structured project architecture.

---

## 🚀 Tech Stack

- **Node.js** — JavaScript runtime
- **Express.js** — Web framework
- **TypeScript** — Type-safe JavaScript
- **Prisma** — ORM and database toolkit
- **PostgreSQL** — Relational database
- **dotenv** — Environment variable management
- **ESLint** — Code linting
- **Prettier** — Code formatting

---

## 📁 Project Structure

```text
express-prisma-starter/
│
├── prisma/
│   ├── migrations/
│   └── schema.prisma
│
├── src/
│   ├── config/
│   │   └── env.ts
│   │
│   ├── controllers/
│   │
│   ├── middlewares/
│   │
│   ├── routes/
│   │
│   ├── services/
│   │
│   ├── utils/
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

---

## ⚙️ Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/) `>= 20`
- npm
- PostgreSQL
- Git

Check your versions:

```bash
node -v
npm -v
psql --version
```

---

## 📥 Installation

### 1. Clone the repository

```bash
git clone <YOUR_REPOSITORY_URL>
```

Move into the project directory:

```bash
cd express-prisma-starter
```

### 2. Install dependencies

```bash
npm install
```

---

## 🔐 Environment Variables

Create a `.env` file in the root directory:

```bash
cp .env.example .env
```

For Windows, you can simply create a `.env` file manually.

Example:

```env
PORT=5000

DATABASE_URL="postgresql://postgres:password@localhost:5432/my_database?schema=public"

NODE_ENV=development
```

### `.env.example`

```env
PORT=5000

DATABASE_URL="postgresql://postgres:password@localhost:5432/database_name?schema=public"

NODE_ENV=development
```

> Never commit your `.env` file to Git.

---

## 🗄️ Prisma Setup

Initialize Prisma if it has not already been initialized:

```bash
npx prisma init
```

Your Prisma schema will be located at:

```text
prisma/schema.prisma
```

Example:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

---

## 🔄 Database Migration

After modifying `schema.prisma`, create a migration:

```bash
npx prisma migrate dev --name init
```

Generate the Prisma Client:

```bash
npx prisma generate
```

For production migrations:

```bash
npx prisma migrate deploy
```

---

## ▶️ Running the Project

### Development

```bash
npm run dev
```

The server will start at:

```text
http://localhost:5000
```

### Production

Build the project:

```bash
npm run build
```

Start the production server:

```bash
npm start
```

---

## 📜 Available Scripts

| Command                     | Description                            |
| --------------------------- | -------------------------------------- |
| `npm run dev`               | Start development server               |
| `npm run build`             | Build TypeScript project               |
| `npm start`                 | Start production server                |
| `npm run lint`              | Run ESLint                             |
| `npm run format`            | Format code with Prettier              |
| `npx prisma generate`       | Generate Prisma Client                 |
| `npx prisma migrate dev`    | Create and apply development migration |
| `npx prisma migrate deploy` | Apply production migrations            |
| `npx prisma studio`         | Open Prisma Studio                     |

---

## 🧑‍💻 Prisma Studio

Prisma Studio provides a visual interface for viewing and managing your database.

Run:

```bash
npx prisma studio
```

Then open the URL shown in your terminal.

---

## 🌐 API Structure

A typical API can follow this structure:

```text
/api
│
├── /users
│   ├── GET     /
│   ├── GET     /:id
│   ├── POST    /
│   ├── PATCH   /:id
│   └── DELETE  /:id
│
└── /auth
    ├── POST    /register
    └── POST    /login
```

---

## 🧩 Example Prisma Client

Create a reusable Prisma client:

```typescript
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

export default prisma;
```

Example service:

```typescript
import prisma from "../config/prisma";

export const getUsers = async () => {
  return await prisma.user.findMany();
};
```

---

## 🔌 Example Express Route

```typescript
import { Router } from "express";
import { getUsers } from "../services/user.service";

const router = Router();

router.get("/", async (_req, res) => {
  const users = await getUsers();

  res.json({
    success: true,
    data: users,
  });
});

export default router;
```

---

## 🏗️ Recommended Architecture

The project follows a simple layered architecture:

```text
Request
   ↓
Routes
   ↓
Controllers
   ↓
Services
   ↓
Prisma
   ↓
PostgreSQL
```

### Routes

Responsible for defining API endpoints.

### Controllers

Handle HTTP requests and responses.

### Services

Contain business logic.

### Prisma

Handles database queries.

### PostgreSQL

Stores application data.

---

## 🛡️ Error Handling

A centralized error-handling middleware is recommended:

```text
Request
   ↓
Route
   ↓
Controller
   ↓
Service
   ↓
Error
   ↓
Global Error Middleware
   ↓
JSON Response
```

Example response:

```json
{
  "success": false,
  "message": "User not found"
}
```

---

## 📦 Adding a New Model

Add your model to:

```text
prisma/schema.prisma
```

Example:

```prisma
model Product {
  id          Int      @id @default(autoincrement())
  name        String
  description String?
  price       Float
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

Then create a migration:

```bash
npx prisma migrate dev --name add_product
```

---

## 🔍 Useful Prisma Commands

### Check Prisma schema

```bash
npx prisma validate
```

### Format Prisma schema

```bash
npx prisma format
```

### Generate Prisma Client

```bash
npx prisma generate
```

### Create migration

```bash
npx prisma migrate dev --name migration_name
```

### Reset development database

```bash
npx prisma migrate reset
```

> `migrate reset` deletes the development database data. Use it carefully.

### Open Prisma Studio

```bash
npx prisma studio
```

---

## 🧪 API Testing

You can test the API using tools such as:

- Postman
- Insomnia
- Bruno
- REST Client
- cURL

Example:

```bash
curl http://localhost:5000/api/users
```

---

## 🔒 Security Recommendations

For production applications, consider adding:

- Helmet
- CORS configuration
- Rate limiting
- Request validation
- Password hashing with bcrypt/Argon2
- JWT or session-based authentication
- Secure HTTP headers
- Input sanitization
- Proper error handling

---

## 🚀 Deployment

Before deploying:

```bash
npm run build
```

Apply production database migrations:

```bash
npx prisma migrate deploy
```

Then start the application:

```bash
npm start
```

Make sure your production environment contains a valid:

```env
DATABASE_URL="..."
```

---

## 📌 Development Workflow

A typical development workflow:

```bash
# Install dependencies
npm install

# Configure environment
cp .env.example .env

# Update Prisma schema
# prisma/schema.prisma

# Create migration
npx prisma migrate dev --name init

# Start development server
npm run dev
```

---

## 🤝 Contributing

1. Fork the repository.
2. Create a feature branch.

```bash
git checkout -b feature/my-feature
```

3. Make your changes.
4. Run linting and tests.

```bash
npm run lint
```

5. Commit your changes.

```bash
git commit -m "feat: add new feature"
```

6. Push the branch.

```bash
git push origin feature/my-feature
```

7. Open a Pull Request.

---

## 📄 License

This project is available under the **MIT License**.

---

## 👨‍💻 Author

**Mahib Alam Khan**

Computer Science & Engineering
American International University-Bangladesh (AIUB)

---

⭐ If this starter helped you, consider giving the repository a star.
