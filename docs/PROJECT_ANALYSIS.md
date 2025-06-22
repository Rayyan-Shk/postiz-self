# Postiz-App Project Analysis

## Overview
This is a monorepo for a social media management/scheduling platform built with:
- **Package Manager**: pnpm with workspaces
- **Monorepo Structure**: Apps + Libraries pattern
- **Project Name**: "gitroom" (in package.json)
- **License**: AGPL-3.0

## Root Structure

### Configuration Files
- `package.json` - Main monorepo configuration with pnpm workspace setup
- `.gitmodules` - Git submodule configuration pointing to `gitroomhq/public-api`
- No turbo.json or nx.json found - using pnpm workspaces directly

### Key Dependencies (Root Level)
- **Frontend**: React 18.3.1, Next.js 14.2.14, Tailwind CSS
- **Backend**: NestJS 10.0.2, Prisma 6.5.0, BullMQ 5.12.12
- **Database**: PostgreSQL (via Prisma)
- **AI/ML**: OpenAI 4.47.1, LangChain ecosystem
- **Social Platforms**: Twitter API, Facebook SDK, Neynar (Farcaster), Nostr
- **Payments**: Stripe 15.5.0
- **File Upload**: Uppy ecosystem, AWS S3
- **Authentication**: JWT, Google Auth, bcrypt

## Apps Architecture

### 1. `/apps/backend/` - Main API Server
- **Framework**: NestJS
- **Purpose**: Core API, authentication, social media integrations
- **Dependencies**: Will analyze in detail

### 2. `/apps/frontend/` - Web Dashboard
- **Framework**: Next.js 14
- **Purpose**: Main user interface for social media management
- **Dependencies**: Will analyze in detail

### 3. `/apps/extension/` - Browser Extension
- **Framework**: Vite + React
- **Purpose**: Browser extension for social media platforms
- **Dependencies**: Will analyze in detail

### 4. `/apps/workers/` - Background Job Processor
- **Framework**: NestJS
- **Purpose**: Handles background tasks and job processing
- **Dependencies**: Will analyze in detail

### 5. `/apps/commands/` - CLI Tools
- **Framework**: NestJS with nestjs-command
- **Purpose**: Command-line interface for administrative tasks
- **Dependencies**: Will analyze in detail

### 6. `/apps/cron/` - Scheduled Tasks
- **Framework**: NestJS
- **Purpose**: Cron jobs and scheduled operations
- **Dependencies**: Will analyze in detail

## Libraries Architecture

### 1. `/libraries/helpers/` - Utility Functions
- **Purpose**: Shared utility functions across all apps
- **Dependencies**: Will analyze in detail

### 2. `/libraries/nestjs-libraries/` - Backend Shared Modules
- **Purpose**: Shared NestJS modules, DTOs, database models
- **Dependencies**: Will analyze in detail

### 3. `/libraries/react-shared-libraries/` - Frontend Shared Components
- **Purpose**: Shared React components and utilities
- **Dependencies**: Will analyze in detail

### 4. `/libraries/plugins/` - Plugin System
- **Purpose**: Plugin architecture for extensibility
- **Dependencies**: Will analyze in detail

## External Dependencies (Git Submodules)
- `libraries/plugins/src/list/public-api` → `git@github.com:gitroomhq/public-api.git`

## TypeScript Path Mapping (tsconfig.base.json)
The `@gitroom/` namespace is NOT an external library - it's an internal monorepo path mapping system:

```json
"paths": {
  "@gitroom/backend/*": ["apps/backend/src/*"],
  "@gitroom/cron/*": ["apps/cron/src/*"],
  "@gitroom/frontend/*": ["apps/frontend/src/*"],
  "@gitroom/helpers/*": ["libraries/helpers/src/*"],
  "@gitroom/nestjs-libraries/*": ["libraries/nestjs-libraries/src/*"],
  "@gitroom/react/*": ["libraries/react-shared-libraries/src/*"],
  "@gitroom/plugins/*": ["libraries/plugins/src/*"],
  "@gitroom/workers/*": ["apps/workers/src/*"],
  "@gitroom/extension/*": ["apps/extension/src/*"]
}
```

## Key Findings from App Analysis

### Dependencies Architecture
- **No individual app dependencies**: All apps use the root `package.json` dependencies
- **Shared dependency pool**: All 285+ dependencies are available to all apps
- **Monorepo approach**: Using pnpm workspaces without turbo.json or nx.json

### Inter-App Dependencies
From the module analysis:
1. **Backend → Libraries**: Heavy use of `@gitroom/nestjs-libraries/*`
2. **Frontend → Libraries**: Heavy use of `@gitroom/react/*` and `@gitroom/helpers/*`
3. **Workers → Backend + Libraries**: Uses both backend and nestjs-libraries
4. **Commands → Libraries**: Primarily uses nestjs-libraries  
5. **Cron → Libraries**: Uses nestjs-libraries and database modules

## Gitroom References Analysis
**IMPORTANT**: `@gitroom/` is NOT an external dependency - it's internal path mapping!

### Major Categories of Internal Dependencies:
1. **@gitroom/nestjs-libraries/** - Backend shared modules (biggest)
2. **@gitroom/react/** - Frontend shared components
3. **@gitroom/helpers/** - Cross-platform utilities
4. **@gitroom/frontend/** - Frontend-specific modules
5. **@gitroom/backend/** - Backend-specific modules
6. **@gitroom/plugins/** - Plugin system
7. **@gitroom/workers/** - Worker-specific modules
8. **@gitroom/cron/** - Cron-specific modules
9. **@gitroom/extension/** - Extension-specific modules

## Complete Gitroom References Catalog

**All `@gitroom/` references are INTERNAL monorepo imports - NOT external dependencies**

### By Usage Frequency:
1. **@gitroom/nestjs-libraries/** (~80+ references) - Backend shared modules
   - Database/Prisma models and services
   - DTOs (Data Transfer Objects)
   - Social media integrations  
   - Email services, upload services
   - Authentication, permissions
   - Agent/AI services, MCP services

2. **@gitroom/react/** (~40+ references) - Frontend shared components
   - Form components (Button, Input, Select, etc.)
   - Translation services
   - Helper utilities
   - Toaster notifications

3. **@gitroom/helpers/** (~30+ references) - Cross-platform utilities
   - Authentication services
   - Fetch utilities
   - Timer utilities
   - Custom decorators

4. **@gitroom/frontend/** (~25+ references) - Frontend-specific modules  
   - Layout components
   - Launch components
   - User context
   - Modal components

5. **@gitroom/backend/** (~10+ references) - Backend-specific modules
   - API controllers
   - MCP services
   - Auth middleware

## External API Integrations & Dependencies

### Social Media Platforms (24 Integrations):
1. **X (Twitter)** - `twitter-api-v2`, `https://twitter.com/`
2. **Facebook** - Facebook Graph API v20.0, `https://graph.facebook.com/`
3. **Instagram** - Facebook Graph API (Instagram Business)
4. **LinkedIn** - LinkedIn API v2, `https://api.linkedin.com/`
5. **YouTube** - Google APIs, `https://www.googleapis.com/`
6. **TikTok** - TikTok API, `https://www.tiktok.com/`
7. **Pinterest** - Pinterest API v5, `https://api.pinterest.com/`
8. **Reddit** - Reddit API, `https://oauth.reddit.com/`
9. **Discord** - Discord API
10. **Slack** - Slack API, `https://slack.com/api/`
11. **Threads** - Meta Threads API, `https://graph.threads.net/`
12. **Mastodon** - Mastodon API (configurable instance)
13. **Bluesky** - AT Protocol, `@atproto/api`
14. **Farcaster** - Neynar SDK, `@neynar/nodejs-sdk`
15. **Telegram** - Telegram Bot API
16. **VK** - VK API, `https://api.vk.com/`
17. **Nostr** - Nostr protocol, `nostr-tools`
18. **Lemmy** - Lemmy instances
19. **Dribbble** - Dribbble API
20. And more...

### Article/Blog Platforms:
1. **Dev.to** - `https://dev.to/api/`
2. **Hashnode** - `https://gql.hashnode.com`
3. **Medium** - Medium API

### Third-Party Services:
1. **OpenAI** - `openai` package for AI features
2. **Stripe** - Payment processing
3. **AWS S3** - File storage (`@aws-sdk/client-s3`)
4. **Cloudflare R2** - Alternative storage
5. **Resend** - Email delivery
6. **GitHub** - GitHub API for trending/stars
7. **Short Link Services**:
   - Short.io (`https://api.short.io/`)
   - Dub.co (`https://api.dub.co/`)
   - Kutt.it
8. **Beehiiv** - Newsletter service

### Database & Infrastructure:
- **PostgreSQL** - Main database (via Prisma)
- **Redis** - Caching and job queues (`ioredis`)
- **BullMQ** - Job processing
- **Prisma** - Database ORM

### Development & Monitoring:
- **PostHog** - Analytics
- **Plausible** - Web analytics  
- **Facebook Pixel** - Tracking

## Database Schema Overview
Based on the Prisma schema:
- **Organization** - Multi-tenant structure
- **User** - Authentication and profiles
- **Integration** - Social media connections
- **Post** - Content management
- **Media** - File management
- **Subscription** - Billing/payments
- **Marketplace** - Plugin marketplace
- **Webhooks** - Event notifications
- **Auto Post** - Scheduling
- **Analytics tracking** - Stars, trending

## Architecture Summary
- **Type**: Monorepo with shared dependencies
- **Pattern**: Apps + Libraries architecture
- **Inter-dependency**: Heavy use of shared libraries via TypeScript path mapping
- **External APIs**: 20+ social media platforms + various SaaS services
- **Not External**: All `@gitroom/` imports are internal monorepo modules

## Quick Reference: What's Internal vs External

### ✅ INTERNAL (Part of this Repository)
- All `@gitroom/` namespace imports
- All apps: backend, frontend, extension, workers, commands, cron
- All libraries: helpers, nestjs-libraries, react-shared-libraries, plugins
- TypeScript path mappings in tsconfig.base.json

### ❌ EXTERNAL (Third-party Dependencies)
- **Social APIs**: Twitter, Facebook, Instagram, LinkedIn, TikTok, etc.
- **SaaS Services**: OpenAI, Stripe, AWS S3, Cloudflare R2
- **Infrastructure**: PostgreSQL, Redis, BullMQ
- **npm packages**: 285+ dependencies in root package.json
- **Git Submodule**: `libraries/plugins/src/list/public-api` → `gitroomhq/public-api`

### 🔗 BORDERLINE (Configured External)
- **Git Submodule**: `gitroomhq/public-api` → `libraries/plugins/src/list/public-api`
  - **Status**: Configured but **NOT INITIALIZED** (empty directory)
  - **Purpose**: External plugin API system for extensibility
  - **Repository**: Part of broader Gitroom/Postiz ecosystem
  - **Functionality**: Public API extensions, custom endpoints, plugin marketplace 