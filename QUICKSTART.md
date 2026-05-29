# Bilbrief - Quick Start Guide

> Get your AI quiz platform running in under 10 minutes!

## 📋 Prerequisites

- Node.js 18+ (check with `node --version`)
- PostgreSQL 14+ (running locally or remote connection)
- npm 9+ (comes with Node.js)

## 🚀 5-Minute Setup

### Step 1: Clone & Install

```bash
cd bilbrief
npm install
```

**Time: ~2 minutes**

### Step 2: Setup Backend Database

```bash
cd apps/api

# Copy environment template
cp .env.example .env

# Update .env with your database:
# DATABASE_URL="postgresql://user:password@localhost:5432/bilbrief"

# Create database and run migrations
npx prisma migrate dev --name init

# Generate Prisma client
npx prisma generate

cd ../..
```

**Time: ~2 minutes**

### Step 3: Start Development Servers

```bash
npm run dev
```

This command:
- Starts Next.js frontend on **http://localhost:3000**
- Starts NestJS backend on **http://localhost:3001**

**Time: ~1 minute**

## ✅ Verify Setup

### Frontend Test
```
Open http://localhost:3000 in browser
Expected: Beautiful home page with features visible
```

### Backend Test
```bash
curl http://localhost:3001/health
Expected: 200 response with health status
```

## 🎮 Try It Out

### 1. Register an Account
```
Go to http://localhost:3000
Click "Sign Up"
Fill in: Email, Username, Password, Display Name
Click "Create Account"
```

### 2. Start a Quiz
```
Click "Start Quiz" on home page
Select a category
Choose difficulty level
Click "Start"
```

### 3. View Leaderboard
```
Click "Leaderboard" in bottom navigation
See global rankings (initially shows seeded data)
```

### 4. Check Your Profile
```
Click "Profile" in bottom navigation
See your XP, level, and stats
```

## 📁 Important Files

```
bilbrief/
├── package.json           # Root monorepo config
├── .env                   # Root environment
├── apps/
│   ├── api/
│   │   ├── .env          # Backend environment
│   │   ├── prisma/
│   │   │   └── schema.prisma  # Database schema
│   │   └── src/
│   │       ├── main.ts        # Entry point
│   │       └── app.module.ts  # Root module
│   └── web/
│       ├── .env.local    # Frontend environment
│       └── src/
│           ├── app/      # Pages
│           ├── components/
│           ├── lib/
│           └── store/
```

## 🔧 Essential Commands

```bash
# Development
npm run dev              # Start both servers
npm --workspace apps/web run dev        # Frontend only
npm --workspace apps/api run start:dev  # Backend only

# Building
npm run build           # Build for production

# Database
cd apps/api
npx prisma studio     # Open database UI
npx prisma migrate dev --name feature  # Create migration

# Testing
npm --workspace apps/api run test      # Backend tests
npm --workspace apps/web run test      # Frontend tests
```

## 🌍 Environment Variables

### Backend (apps/api/.env)

**Required:**
```
DATABASE_URL=postgresql://user:password@localhost:5432/bilbrief
JWT_SECRET=your-secret-key-here
```

**Optional:**
```
JWT_EXPIRATION=7d
GOOGLE_CLIENT_ID=your-id
GOOGLE_CLIENT_SECRET=your-secret
OPENAI_API_KEY=your-api-key
NODE_ENV=development
API_PORT=3001
```

### Frontend (apps/web/.env.local)

**Auto-configured, but can customize:**
```
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_SOCKET_URL=http://localhost:3001
```

## 🐛 Common Issues & Fixes

### Issue: "Cannot find module 'prisma'"
```bash
# Fix:
cd apps/api
npm install
npx prisma generate
```

### Issue: "Database connection refused"
```bash
# Check PostgreSQL is running:
# Mac: brew services start postgresql
# Windows: Services > PostgreSQL > Start
# Linux: sudo systemctl start postgresql

# Verify connection string in .env:
# DATABASE_URL=postgresql://user:password@localhost:5432/bilbrief
```

### Issue: "Port 3000 already in use"
```bash
# Kill process or use different port:
npm run dev -- -p 3001  # Frontend on 3001
```

### Issue: "API calls return 401 Unauthorized"
```bash
# Make sure:
1. You're logged in (check localStorage in browser DevTools)
2. Backend is running (http://localhost:3001 accessible)
3. JWT_SECRET is same in .env
```

## 📊 Database Seeding (Optional)

Add initial data:

```bash
cd apps/api

# Create seed file
cat > prisma/seed.ts << 'EOF'
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function main() {
  // Create categories
  const scienceCategory = await prisma.category.create({
    data: {
      name: 'Science',
      slug: 'science',
      description: 'Physics, Chemistry, Biology',
      icon: '🔬',
    },
  });

  console.log('Seeded database!');
}

main()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
EOF

# Run seed
npx ts-node prisma/seed.ts
```

## 🎯 Next Steps After Setup

### Short Term
1. ✅ Verify frontend & backend running
2. ✅ Test auth (register, login)
3. ✅ Test quiz creation
4. ✅ Check database in Prisma Studio

### Medium Term
- [ ] Integrate real AI (OpenAI/Claude)
- [ ] Add more categories & questions
- [ ] Test multiplayer duels
- [ ] Set up PWA

### Production
- [ ] Deploy to hosting (Vercel, Railway, etc.)
- [ ] Setup custom domain
- [ ] Configure production database
- [ ] Enable rate limiting
- [ ] Setup monitoring

## 📚 Documentation

- **README.md** - Project overview & features
- **DEVELOPMENT.md** - Developer guide
- **API Reference** - Endpoint documentation (see apps/api README)
- **Prisma Docs** - Database schema (see prisma/schema.prisma)

## 🆘 Need Help?

### Check These Resources
1. Read the errors carefully - they usually point to the fix
2. Check MongoDB connection string format
3. Ensure Node version is 18+
4. Review environment variables

### File an Issue
If stuck, check the GitHub issues or create a new one with:
- Error message
- Steps to reproduce
- Your OS and Node version
- Relevant logs

## 🎉 You're Ready!

Your Bilbrief quiz platform is now running! 🚀

Start by:
- Creating an account
- Starting a quiz
- Checking the leaderboard
- Exploring the profile page

Happy quizzing! ⚡

---

**Pro Tip**: Keep `npm run dev` running in one terminal and use another for database commands.
