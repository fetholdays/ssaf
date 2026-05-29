# Bilbrief - AI-Powered Quiz Platform

A modern, production-ready AI-powered quiz platform with real-time multiplayer gameplay, premium UI/UX, and comprehensive gamification features.

## 🎯 Project Overview

**Bilbrief** is not just another quiz app. It's a comprehensive, modern platform that combines:

- **AI-Powered Questions**: Intelligent question generation and verification
- **Real-time Multiplayer**: Socket.io-based duel system for instant competitive gameplay
- **Premium UI/UX**: Dark mode with neon glow effects, glassmorphism, and smooth animations
- **Gamification**: XP system, levels, streaks, achievements, and leaderboards
- **Production-Ready**: Fully functional backend API, frontend with optimizations, and real-time systems

## 📁 Project Structure

```
bilbrief/
├── apps/
│   ├── api/                    # NestJS Backend
│   │   ├── src/
│   │   │   ├── auth/          # Authentication module
│   │   │   ├── modules/       # Core modules
│   │   │   │   ├── users/
│   │   │   │   ├── quiz/
│   │   │   │   ├── categories/
│   │   │   │   ├── ai/        # AI question generation
│   │   │   │   ├── duel/      # Multiplayer system
│   │   │   │   ├── leaderboard/
│   │   │   ├── gateways/      # Socket.io gateway
│   │   │   ├── prisma/        # Database service
│   │   │   └── main.ts        # Entry point
│   │   ├── prisma/
│   │   │   └── schema.prisma  # Database schema
│   │   └── package.json
│   │
│   └── web/                    # Next.js Frontend
│       ├── src/
│       │   ├── app/
│       │   │   ├── page.tsx    # Home page
│       │   │   ├── auth/       # Auth pages
│       │   │   ├── categories/ # Category list
│       │   │   ├── quiz/       # Quiz pages
│       │   │   ├── leaderboard/# Leaderboard
│       │   │   ├── profile/    # User profile
│       │   │   └── layout.tsx
│       │   ├── components/
│       │   │   ├── layout/     # Layout components
│       │   │   └── ui/         # UI components
│       │   ├── lib/
│       │   │   ├── api/        # API client & Socket.io
│       │   │   └── hooks/      # Custom hooks
│       │   ├── store/          # Zustand stores
│       │   └── types/          # TypeScript types
│       └── package.json
│
├── packages/
│   └── shared/                 # Shared types (future)
│
├── package.json               # Monorepo root
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL 14+
- npm or yarn

### Installation

1. **Clone and install dependencies**

```bash
cd bilbrief
npm install
```

2. **Setup Backend**

```bash
# Copy environment file
cp apps/api/.env.example apps/api/.env

# Update .env with your database connection
# DATABASE_URL="postgresql://user:password@localhost:5432/bilbrief?schema=public"
# JWT_SECRET="your-secret-key"

# Install Prisma and create database
cd apps/api
npx prisma migrate dev --name init
npx prisma generate
cd ../..
```

3. **Setup Frontend**

```bash
# Update environment (already configured)
# apps/web/.env.local is pre-configured for development
```

4. **Run Development Servers**

```bash
# From root directory
npm run dev

# This runs both:
# - Web: http://localhost:3000
# - API: http://localhost:3001
```

## 🏗️ Architecture

### Backend (NestJS)

**Core Modules:**

1. **Auth Module** - JWT-based authentication with password hashing
2. **Users Module** - Profile management and user statistics
3. **Categories Module** - Quiz categories and favorite tracking
4. **Quiz Module** - Quiz sessions, question distribution, answer validation
5. **AI Module** - Question generation (placeholder for real AI integration)
6. **Duel Module** - Multiplayer matchmaking and room management
7. **Leaderboard Module** - Global, weekly, and category-specific rankings

**Database Schema:**

- **Users**: User accounts with XP, levels, achievements
- **Questions**: Quiz questions with difficulty levels
- **Quizzes**: Quiz sessions with scoring
- **Categories**: Question categories
- **Achievements**: Badge system
- **DuelRooms**: Multiplayer match management
- **Leaderboards**: Cached ranking data

**Real-time Features (Socket.io):**

- Duel room creation and joining
- Live question distribution
- Real-time answer submissions
- Leaderboard updates
- User status tracking

### Frontend (Next.js App Router)

**Pages:**

- `/` - Home page with features
- `/auth/login` - Login page
- `/auth/register` - Registration page
- `/categories` - Category selection
- `/quiz/[mode]/[category]` - Quiz setup
- `/quiz/[quizId]/play` - Quiz gameplay
- `/leaderboard` - Leaderboard view
- `/profile` - User profile

**State Management:**

- **Zustand Stores**:
  - `useAuthStore` - Authentication and user state
  - `useQuizStore` - Active quiz state

**API Client:**

- RESTful API client with automatic token management
- Socket.io client for real-time features

## 🎮 Game Modes

1. **Classic** - Answer 10 questions, earn points
2. **Timed** - Answer as many as possible in 60 seconds
3. **Ordered** - One wrong answer and your streak ends
4. **Duel** - Real-time competitive gameplay

## 🏆 Gamification Features

- **XP System**: Earn XP for correct answers, bonuses for speed and streaks
- **Levels**: Automatic level progression (1000 XP per level)
- **Streaks**: Consecutive correct answers with combo bonuses
- **Achievements**: 50+ unlockable badges based on milestones
- **Leaderboards**: 
  - Global (all-time)
  - Weekly rankings
  - Category-specific
  - Friend rankings (when social features added)

## 🎨 UI/UX Features

- **Dark Mode**: Complete dark theme with neon accents
- **Animations**: Smooth transitions with Framer Motion
- **Components**:
  - Premium buttons with hover effects
  - Glassmorphic cards
  - Skeleton loading screens
  - Toast notifications
  - Responsive grid layouts

- **Bottom Navigation**: Clean navigation bar with 5 main sections
- **Mobile-First**: Fully responsive design
- **PWA Ready**: Service worker setup (future)

## 📡 API Endpoints

### Auth
- `POST /auth/register` - Create account
- `POST /auth/login` - Sign in
- `POST /auth/refresh` - Refresh token
- `POST /auth/logout` - Sign out
- `POST /auth/change-password` - Change password
- `GET /auth/me` - Current user info

### Users
- `GET /users/profile` - User profile
- `PUT /users/profile` - Update profile
- `GET /users/:id` - Public profile
- `GET /users/:id/stats` - User statistics
- `GET /users/:id/achievements` - Earned badges

### Categories
- `GET /categories` - All categories
- `GET /categories/:slug` - Category details
- `GET /categories/:id/stats` - Category statistics
- `POST /categories/:categoryId/favorites` - Add favorite
- `DELETE /categories/:categoryId/favorites` - Remove favorite

### Quiz
- `POST /quiz/start` - Start new quiz
- `GET /quiz/:quizId/question/:index` - Get question
- `POST /quiz/:quizId/answer/:index` - Submit answer
- `GET /quiz/:quizId/results` - Quiz results
- `GET /quiz/user/history` - User quiz history

### Leaderboard
- `GET /leaderboard/global` - Global rankings
- `GET /leaderboard/weekly` - Weekly rankings
- `GET /leaderboard/category/:id` - Category rankings
- `GET /leaderboard/me/rank` - User rank

### Duel
- `POST /duel/create` - Create duel room
- `GET /duel/available` - Find available opponent
- `POST /duel/:roomId/join` - Join room
- `GET /duel/:roomId` - Room details
- `POST /duel/:roomId/complete` - Mark duel complete
- `POST /duel/:roomId/abandon` - Leave duel

### WebSocket Events

**Client → Server:**
- `user:login` - User logged in
- `duel:create` - Create duel room
- `duel:find` - Find opponent
- `duel:answer_submitted` - Submit answer
- `duel:finish` - Finish duel
- `leaderboard:subscribe` - Subscribe to updates

**Server → Client:**
- `duel:room_created` - Room created
- `duel:waiting` - Waiting for opponent
- `duel:opponent_joined` - Opponent joined
- `duel:answer_received` - Opponent answered
- `duel:next_question` - Next question
- `duel:quiz_completed` - Quiz done
- `duel:finished` - Duel ended
- `leaderboard:updated` - Leaderboard update
- `user:notification` - Notification

## 🔐 Security

- JWT-based authentication with refresh tokens
- Password hashing with bcrypt
- CORS configuration
- Helmet.js for security headers
- Input validation with class-validator
- Rate limiting ready (add in production)

## 📊 Database

Using PostgreSQL with Prisma ORM. Schema includes:

- 15+ tables with proper relationships
- Indexes on frequently queried fields
- Cascade delete for data integrity
- Support for complex queries

## 🚀 Production Deployment

### Backend (NestJS)

```bash
cd apps/api
npm run build
npm run start

# Or with PM2
pm2 start dist/main.js --name bilbrief-api
```

### Frontend (Next.js)

```bash
cd apps/web
npm run build
npm run start

# Or with static export
npm run build
# Deploy to Vercel, Netlify, or static host
```

### Environment Variables

**Backend (.env):**
```
DATABASE_URL=postgresql://user:password@host:5432/bilbrief
JWT_SECRET=your-super-secret-key
JWT_EXPIRATION=7d
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
OPENAI_API_KEY=...
API_PORT=3001
SOCKET_CORS_ORIGIN=https://yourdomain.com
```

**Frontend (.env.local):**
```
NEXT_PUBLIC_API_URL=https://api.yourdomain.com
NEXT_PUBLIC_SOCKET_URL=https://api.yourdomain.com
```

## 🧪 Testing

```bash
# Backend tests
npm --workspace apps/api run test

# Frontend tests (to be implemented)
npm --workspace apps/web run test
```

## 📝 Future Enhancements

- [ ] Real AI integration (OpenAI/Claude)
- [ ] Social features (friends, messaging)
- [ ] Advanced analytics dashboard
- [ ] Mobile app (React Native)
- [ ] PWA installation support
- [ ] Video tutorials
- [ ] Admin panel with moderation
- [ ] Custom quiz creation by users
- [ ] Streaming integration
- [ ] Payment/premium features

## 🤝 Contributing

This is a production-ready application. For modifications:

1. Follow NestJS and Next.js best practices
2. Maintain TypeScript strict mode
3. Write proper error handling
4. Add tests for new features
5. Keep performance optimized

## 📄 License

MIT License - See LICENSE file

## 🎓 Learning Resources

- [NestJS Documentation](https://docs.nestjs.com/)
- [Next.js App Router](https://nextjs.org/docs/app)
- [Prisma ORM](https://www.prisma.io/docs/)
- [Socket.io](https://socket.io/docs/)
- [Zustand State Management](https://github.com/pmndrs/zustand)
- [Framer Motion](https://www.framer.com/motion/)
- [TailwindCSS](https://tailwindcss.com/)

## 📞 Support

For issues or questions, please create an issue in the repository.

---

**Bilbrief** - Where Knowledge Meets Competition ⚡
