# Bilbrief Development Guide

## 🎯 Quick Reference

### Development Commands

```bash
# Installation
npm install

# Development (runs both frontend and backend)
npm run dev

# Frontend only
npm --workspace apps/web run dev

# Backend only
npm --workspace apps/api run start:dev

# Build
npm run build

# Database operations
cd apps/api
npx prisma migrate dev         # Create migration
npx prisma migrate deploy      # Apply migrations
npx prisma studio             # Prisma UI
npx prisma generate           # Generate client
```

### Folder Structure Overview

**Backend Architecture:**
```
apps/api/src/
├── auth/                   # JWT auth, guards, decorators
├── modules/
│   ├── users/             # User profiles & stats
│   ├── quiz/              # Quiz logic & scoring
│   ├── categories/        # Question categories
│   ├── ai/                # AI question generation
│   ├── duel/              # Multiplayer matchmaking
│   └── leaderboard/       # Rankings & caching
├── gateways/              # Socket.io gateway
├── prisma/                # Prisma service
├── app.module.ts          # Root module
└── main.ts                # Entry point
```

**Frontend Architecture:**
```
apps/web/src/
├── app/                   # Next.js pages
│   ├── auth/              # Login/Register
│   ├── categories/        # Category selection
│   ├── quiz/              # Quiz pages
│   ├── leaderboard/       # Rankings
│   ├── profile/           # User profile
│   ├── page.tsx           # Home
│   └── layout.tsx         # Root layout
├── components/
│   ├── layout/            # MainLayout, BottomNav
│   └── ui/                # Button, Card, Skeleton, Alert
├── lib/
│   ├── api/               # API client, Socket.io manager
│   └── hooks/             # Custom hooks
├── store/                 # Zustand stores
└── types/                 # TypeScript types
```

## 🔧 Common Tasks

### Adding a New Backend Endpoint

1. **Create DTO** (data-transfer-object)
```typescript
// apps/api/src/modules/feature/dto/create-feature.dto.ts
export class CreateFeatureDto {
  @IsString()
  name: string;
}
```

2. **Add to Service**
```typescript
// apps/api/src/modules/feature/feature.service.ts
async createFeature(dto: CreateFeatureDto) {
  return this.prisma.feature.create({
    data: dto,
  });
}
```

3. **Add to Controller**
```typescript
// apps/api/src/modules/feature/feature.controller.ts
@Post()
create(@Body() dto: CreateFeatureDto) {
  return this.featureService.createFeature(dto);
}
```

### Adding a New Frontend Page

1. **Create page component**
```typescript
// apps/web/src/app/new-page/page.tsx
'use client';

import { useEffect } from 'react';
import MainLayout from '@/components/layout/MainLayout';

export default function NewPage() {
  return (
    <MainLayout>
      {/* Your content */}
    </MainLayout>
  );
}
```

2. **Add to bottom navigation** (if needed)
```typescript
// apps/web/src/components/layout/BottomNavigation.tsx
// Add new route to the navItems array
```

### Adding New Zustand State

```typescript
// apps/web/src/store/feature.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface FeatureStore {
  feature: any;
  setFeature: (feature: any) => void;
}

export const useFeatureStore = create<FeatureStore>(
  persist(
    (set) => ({
      feature: null,
      setFeature: (feature) => set({ feature }),
    }),
    { name: 'feature-store' },
  ),
);
```

## 📡 API Client Usage

### In React Components

```typescript
'use client';

import { useApi } from '@/lib/hooks/useApi';

export function MyComponent() {
  // Use the pre-built hooks
  const { register, isLoading } = useAuth();
  const { data: profile } = useProfile();

  // Or use API client directly
  const handleFetch = async () => {
    const data = await apiClient.getCategories();
  };

  return (
    // Your JSX
  );
}
```

## 🔌 Socket.io Events

### On Server (apps/api/src/gateways/quiz.gateway.ts)

```typescript
@SubscribeMessage('custom:event')
handleCustomEvent(
  @MessageBody() data: any,
  @ConnectedSocket() client: Socket,
): void {
  // Handle event
  this.server.emit('custom:response', result);
}
```

### On Client (apps/web/src/lib/api/socket.ts)

```typescript
// Emit
socketManager.emit('custom:event', data);

// Listen
socket.on('custom:response', (data) => {
  // Handle response
});
```

## 🎨 Component Pattern

All UI components follow this pattern:

```typescript
interface ComponentProps {
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  children: React.ReactNode;
}

export function Component({ 
  variant = 'primary',
  size = 'md',
  disabled = false,
  children,
  ...props 
}: ComponentProps) {
  return (
    <button
      className={cn(
        // Base styles
        // Variant styles
        // Size styles
        disabled && 'opacity-50 cursor-not-allowed'
      )}
      disabled={disabled}
      {...props}
    >
      {children}
    </button>
  );
}
```

## 🗂️ Database Schema Quick Reference

### Key Tables

**User**
```
id, email, username, displayName, passwordHash, avatar
xpTotal, level, currentStreakCount, bestStreakCount
isVerified, isBanned, bannedReason
createdAt, updatedAt
```

**Question**
```
id, categoryId, text, options (JSON), correctOption
difficulty, aiGenerated, source, verifiedAt
createdAt, updatedAt
```

**Quiz**
```
id, userId, categoryId, mode, difficulty
totalQuestions, answeredCount, correctCount, score, xpEarned
startedAt, completedAt
```

**Achievement**
```
id, slug, name, description, icon, requirement
unlockedAt
```

**DuelRoom**
```
id, status, player1Id, player2Id
player1Score, player2Score, winner
createdAt, startedAt, completedAt
```

## 🔒 Authentication Flow

1. User registers with email/password
2. Password hashed with bcrypt
3. User logs in, receives JWT tokens (access + refresh)
4. Access token used for API requests (Bearer token)
5. Refresh token used to get new token pair
6. Guards check JWT on protected routes

## 📈 Performance Tips

1. **Use React Query** for API calls (caching, background refresh)
2. **Memoize components** that receive props
3. **Lazy load pages** with Next.js dynamic imports
4. **Optimize images** with Next.js Image component
5. **Use Skeleton screens** for better UX during loads
6. **Monitor bundle size** with `next/bundle-analyzer`

## 🐛 Debugging

### Backend
```bash
# Run with debugging
node --inspect-brk dist/main.js

# Or use VS Code debugger
# Set breakpoint in TypeScript file
# Debug console will pause at breakpoint
```

### Frontend
```bash
# Use React DevTools extension
# Use Next.js debug mode in VS Code
# Check browser console for errors
```

### Database
```bash
# Open Prisma Studio
npx prisma studio

# View database directly with psql
psql postgresql://user:password@localhost/bilbrief
```

## 📝 Coding Standards

- **TypeScript**: Strict mode enabled, no `any` types
- **Naming**: camelCase for functions, PascalCase for components
- **Exports**: Use named exports for utilities, default for pages
- **Comments**: JSDoc for functions, TODO for future work
- **Tests**: Write tests for complex logic
- **Error Handling**: Always catch and handle errors gracefully

## 🚀 Deployment Checklist

- [ ] Environment variables configured
- [ ] Database migrations run
- [ ] API endpoint verified
- [ ] Socket.io CORS configured
- [ ] Frontend environment variables set
- [ ] Build tested locally
- [ ] Security headers configured
- [ ] Rate limiting enabled
- [ ] Monitoring setup
- [ ] Backup strategy in place

---

**Need Help?** Refer to the main README.md or check specific framework docs.
