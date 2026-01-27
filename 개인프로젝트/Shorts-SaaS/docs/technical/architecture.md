# Technical Architecture Document

## 1. System Architecture Overview

### 1.1 High-Level Architecture (Next.js 16 풀스택)
```
┌─────────────────────────────────────────────────────────────┐
│                     Next.js 16 App (Vercel)                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    Client Layer                       │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐     │   │
│  │  │  React 19  │  │  Tailwind  │  │  shadcn/ui │     │   │
│  │  │  Components│  │    CSS     │  │  + Lucide  │     │   │
│  │  └────────────┘  └────────────┘  └────────────┘     │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Server Components & Actions              │   │
│  │  ┌────────────────────────────────────────────────┐  │   │
│  │  │  API Routes (app/api/)                         │  │   │
│  │  │  - /api/analytics/* (YouTube Data API)         │  │   │
│  │  │  - /api/scripts/* (OpenRouter LLM)             │  │   │
│  │  │  - /api/payments/* (Toss Payments)             │  │   │
│  │  │  - /api/webhooks/* (Clerk, Stripe, etc)        │  │   │
│  │  └────────────────────────────────────────────────┘  │   │
│  │                                                        │   │
│  │  ┌────────────────────────────────────────────────┐  │   │
│  │  │  Middleware                                     │  │   │
│  │  │  - Clerk Authentication                         │  │   │
│  │  │  - Rate Limiting                                │  │   │
│  │  └────────────────────────────────────────────────┘  │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│  Supabase    │  │    Clerk     │  │   Vercel     │
│  (Database)  │  │    (Auth)    │  │   (CDN/Edge) │
│              │  │              │  │              │
│ • PostgreSQL │  │ • Google OAuth│ │ • Functions  │
│ • Storage    │  │ • YouTube API│  │ • Analytics  │
│ • Realtime   │  │ • Sessions   │  │ • KV (Redis) │
└──────────────┘  └──────────────┘  └──────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   External Services                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ YouTube  │  │OpenRouter│  │  Toss    │  │ SOLAPI   │   │
│  │   API    │  │   (LLM)  │  │ Payments │  │  (SMS)   │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                 │
│  │  Resend  │  │  Sentry  │  │ PostHog  │                 │
│  │ (Email)  │  │ (Errors) │  │(Analytics)│                 │
│  └──────────┘  └──────────┘  └──────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

**아키텍처 설명**:
- **단일 Next.js 16 앱**: 프론트엔드 + 백엔드 통합
- **Turbopack**: 5-10x 빠른 개발 서버
- **Server Actions**: API Routes 대신 직접 서버 함수 호출 가능
- **Clerk Auth**: Google OAuth + YouTube API 스코프 관리
- **Supabase**: PostgreSQL + Storage + Realtime
- **Vercel Edge**: CDN + Edge Functions + KV (Redis)

### 1.2 Technology Stack (2025 최신)

#### Core Framework
- **Framework**: Next.js 16 (Turbopack, React 19.2)
- **Language**: TypeScript 5.6+
- **Runtime**: Node.js 20.9+ LTS

#### Frontend
- **UI Framework**: React 19.2
- **Styling**: Tailwind CSS 4.0
- **UI Components**: shadcn/ui (Radix UI)
- **Icons**: Lucide React
- **Animation**: Framer Motion
- **State Management**:
  - Zustand (글로벌 상태)
  - TanStack Query v5 (서버 상태)
- **Forms**: React Hook Form + Zod
- **Charts**: Recharts
- **Date**: date-fns
- **Utilities**: es-toolkit, ts-pattern

#### Backend (Next.js 내장)
- **API**: Next.js API Routes + Server Actions
- **Validation**: Zod
- **ORM**: Prisma 6.0+
- **Database**: Supabase (PostgreSQL)
- **Cache**: Vercel KV (Redis)
- **Storage**: Supabase Storage / Vercel Blob

#### Authentication & Authorization
- **Primary**: Clerk
  - Google OAuth
  - YouTube API Scopes
  - Supabase 네이티브 통합
- **Alternative**: NextAuth.js v5 (Auth.js)

#### AI/LLM
- **LLM Provider**: OpenRouter API
  - GPT-4, Claude 3.5, Gemini Pro 통합
  - 무료 모델 지원 (DeepSeek)
- **Client**: OpenAI SDK
- **Alternative**: Vercel AI SDK

#### Payments & Communications
- **Payments**: Toss Payments
  - 정기 결제 (구독)
  - 간편 결제 (카카오페이, 네이버페이)
- **SMS/Alimtalk**: SOLAPI
- **Email**: Resend

#### Infrastructure
- **Hosting**: Vercel
  - Edge Functions
  - CDN (CloudFront)
  - Serverless Functions
- **CI/CD**: GitHub Actions + Vercel
- **Error Tracking**: Sentry
- **Analytics**: PostHog
- **Monitoring**: Vercel Analytics

#### Development Tools
- **Component Dev**: Storybook
- **Testing**: Vitest, Playwright (optional)
- **Linting**: ESLint, Prettier
- **Git Hooks**: Husky (optional)

## 2. Core Services Architecture

### 2.1 Authentication Service
```typescript
// Service Responsibilities
- OAuth 2.0 integration (Google/YouTube)
- JWT token generation and validation
- Session management
- User registration and profile management
- Permission and role management

// API Endpoints
POST   /auth/google/oauth
POST   /auth/refresh-token
POST   /auth/logout
GET    /auth/me
PATCH  /auth/profile
```

### 2.2 Analytics Service
```typescript
// Service Responsibilities
- YouTube Data API integration
- Channel metrics aggregation
- Video performance tracking
- Competitor analysis
- Trend detection and predictions

// API Endpoints
GET    /analytics/channels/:channelId/overview
GET    /analytics/channels/:channelId/videos
GET    /analytics/channels/:channelId/compare
GET    /analytics/trends
GET    /analytics/insights
```

### 2.3 Script Generation Service
```typescript
// Service Responsibilities
- AI prompt engineering
- Script generation via OpenAI API
- Script storage and versioning
- Translation services
- Template management

// API Endpoints
POST   /scripts/generate
GET    /scripts
GET    /scripts/:scriptId
PATCH  /scripts/:scriptId
DELETE /scripts/:scriptId
POST   /scripts/:scriptId/translate
GET    /scripts/templates
```

### 2.4 Video Management Service
```typescript
// Service Responsibilities
- Video metadata fetching
- Thumbnail analysis
- Video download (optional)
- SEO recommendations
- Content categorization

// API Endpoints
GET    /videos/channels/:channelId
GET    /videos/:videoId
POST   /videos/:videoId/download
GET    /videos/:videoId/seo-analysis
POST   /videos/:videoId/thumbnail-analysis
```

### 2.5 Community Service
```typescript
// Service Responsibilities
- User feedback management
- Comment aggregation
- Notification system
- Activity feed
- User interactions

// API Endpoints
GET    /community/feed
POST   /community/feedback
GET    /community/notifications
PATCH  /community/notifications/:id/read
GET    /community/activities
```

## 3. Database Schema

### 3.1 Core Tables

```sql
-- Users Table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email VARCHAR(255) UNIQUE NOT NULL,
  name VARCHAR(255),
  avatar_url TEXT,
  google_id VARCHAR(255) UNIQUE,
  subscription_tier VARCHAR(50) DEFAULT 'free',
  subscription_status VARCHAR(50),
  stripe_customer_id VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  last_login_at TIMESTAMP
);

-- YouTube Channels Table
CREATE TABLE youtube_channels (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  youtube_channel_id VARCHAR(255) UNIQUE NOT NULL,
  title VARCHAR(255),
  description TEXT,
  thumbnail_url TEXT,
  subscriber_count BIGINT,
  video_count INTEGER,
  view_count BIGINT,
  access_token TEXT,
  refresh_token TEXT,
  is_primary BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Channel Analytics Table
CREATE TABLE channel_analytics (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  channel_id UUID REFERENCES youtube_channels(id) ON DELETE CASCADE,
  date DATE NOT NULL,
  subscribers_gained INTEGER,
  subscribers_lost INTEGER,
  views BIGINT,
  watch_time_minutes BIGINT,
  average_view_duration DECIMAL(10,2),
  engagement_rate DECIMAL(5,2),
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(channel_id, date)
);

-- Videos Table
CREATE TABLE videos (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  channel_id UUID REFERENCES youtube_channels(id) ON DELETE CASCADE,
  youtube_video_id VARCHAR(255) UNIQUE NOT NULL,
  title VARCHAR(500),
  description TEXT,
  thumbnail_url TEXT,
  published_at TIMESTAMP,
  duration INTEGER, -- in seconds
  view_count BIGINT,
  like_count INTEGER,
  comment_count INTEGER,
  tags TEXT[],
  category VARCHAR(100),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Scripts Table
CREATE TABLE scripts (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  title VARCHAR(500),
  content TEXT NOT NULL,
  language VARCHAR(10) DEFAULT 'ko',
  video_type VARCHAR(50), -- 'short', 'long'
  tone VARCHAR(50), -- 'casual', 'professional', 'educational'
  topic VARCHAR(255),
  word_count INTEGER,
  version INTEGER DEFAULT 1,
  parent_script_id UUID REFERENCES scripts(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Script Templates Table
CREATE TABLE script_templates (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(255) NOT NULL,
  description TEXT,
  template_content TEXT NOT NULL,
  category VARCHAR(100),
  language VARCHAR(10),
  is_public BOOLEAN DEFAULT false,
  usage_count INTEGER DEFAULT 0,
  created_by UUID REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- User Activities Table
CREATE TABLE user_activities (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  activity_type VARCHAR(100) NOT NULL,
  resource_type VARCHAR(100),
  resource_id UUID,
  metadata JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Notifications Table
CREATE TABLE notifications (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  type VARCHAR(100) NOT NULL,
  title VARCHAR(255),
  message TEXT,
  is_read BOOLEAN DEFAULT false,
  action_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  read_at TIMESTAMP
);

-- API Usage Tracking
CREATE TABLE api_usage (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  service VARCHAR(100), -- 'openai', 'youtube'
  endpoint VARCHAR(255),
  tokens_used INTEGER,
  cost DECIMAL(10,6),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Subscriptions Table
CREATE TABLE subscriptions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  stripe_subscription_id VARCHAR(255) UNIQUE,
  tier VARCHAR(50) NOT NULL, -- 'free', 'pro', 'business'
  status VARCHAR(50), -- 'active', 'canceled', 'past_due'
  current_period_start TIMESTAMP,
  current_period_end TIMESTAMP,
  cancel_at_period_end BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### 3.2 Indexes
```sql
-- Performance optimization indexes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_google_id ON users(google_id);
CREATE INDEX idx_channels_user_id ON youtube_channels(user_id);
CREATE INDEX idx_channels_youtube_id ON youtube_channels(youtube_channel_id);
CREATE INDEX idx_analytics_channel_date ON channel_analytics(channel_id, date DESC);
CREATE INDEX idx_videos_channel_id ON videos(channel_id);
CREATE INDEX idx_videos_published ON videos(published_at DESC);
CREATE INDEX idx_scripts_user_id ON scripts(user_id);
CREATE INDEX idx_scripts_created ON scripts(created_at DESC);
CREATE INDEX idx_activities_user_id ON user_activities(user_id, created_at DESC);
CREATE INDEX idx_notifications_user_read ON notifications(user_id, is_read);
CREATE INDEX idx_api_usage_user_date ON api_usage(user_id, created_at DESC);
```

## 4. API Design

### 4.1 RESTful API Conventions
```
Base URL: https://api.yourplatform.com/v1

Authentication: Bearer Token (JWT)
Content-Type: application/json
Rate Limiting: 1000 requests/hour (free), 10000 requests/hour (pro)
```

### 4.2 Standard Response Format
```typescript
// Success Response
{
  success: true,
  data: {
    // Response data
  },
  meta: {
    page: 1,
    limit: 20,
    total: 100
  }
}

// Error Response
{
  success: false,
  error: {
    code: "VALIDATION_ERROR",
    message: "Invalid input parameters",
    details: [
      {
        field: "email",
        message: "Invalid email format"
      }
    ]
  }
}
```

### 4.3 Key API Endpoints

#### Analytics Endpoints
```typescript
// Get channel overview
GET /analytics/channels/:channelId/overview?period=30d
Response: {
  subscribers: { total: 15000, change: +500, changePercent: 3.4 },
  views: { total: 1200000, change: +50000, changePercent: 4.3 },
  watchTime: { total: 25000, change: +2000, changePercent: 8.7 },
  engagement: { rate: 4.5, change: +0.3 }
}

// Get video performance
GET /analytics/channels/:channelId/videos?sort=views&limit=10
Response: {
  videos: [
    {
      id: "video-uuid",
      youtubeId: "dQw4w9WgXcQ",
      title: "Video Title",
      views: 50000,
      likes: 2500,
      comments: 150,
      engagement: 5.3,
      publishedAt: "2024-10-15T10:00:00Z"
    }
  ]
}
```

#### Script Generation Endpoints
```typescript
// Generate script
POST /scripts/generate
Request: {
  topic: "How to start a YouTube channel",
  videoType: "long", // or "short"
  tone: "educational",
  language: "ko",
  duration: 600, // seconds
  targetAudience: "beginners"
}
Response: {
  scriptId: "uuid",
  content: "Generated script content...",
  wordCount: 850,
  estimatedDuration: 600,
  sections: [
    { type: "intro", content: "..." },
    { type: "main", content: "..." },
    { type: "outro", content: "..." }
  ]
}

// Translate script
POST /scripts/:scriptId/translate
Request: {
  targetLanguage: "en"
}
Response: {
  translatedScriptId: "uuid",
  originalLanguage: "ko",
  targetLanguage: "en",
  content: "Translated content..."
}
```

## 5. Security Architecture

### 5.1 Authentication Flow
```
1. User clicks "Sign in with Google"
2. Redirect to Google OAuth consent screen
3. User authorizes YouTube data access
4. Google redirects back with authorization code
5. Exchange code for access/refresh tokens
6. Create/update user in database
7. Generate JWT with user claims
8. Return JWT to client
9. Client stores JWT in httpOnly cookie
```

### 5.2 Authorization Levels
```typescript
enum Role {
  FREE = 'free',
  PRO = 'pro',
  BUSINESS = 'business',
  ADMIN = 'admin'
}

// Permission matrix
const permissions = {
  'scripts:generate': {
    free: { limit: 5, period: 'month' },
    pro: { limit: -1 }, // unlimited
    business: { limit: -1 }
  },
  'analytics:advanced': {
    free: false,
    pro: true,
    business: true
  },
  'channels:multi': {
    free: { max: 1 },
    pro: { max: 1 },
    business: { max: 5 }
  }
}
```

### 5.3 Data Encryption
- **At Rest**: AES-256 encryption for sensitive data (tokens, API keys)
- **In Transit**: TLS 1.3 for all communications
- **API Keys**: Stored in AWS Secrets Manager
- **Database**: Encrypted RDS instance

### 5.4 Rate Limiting
```typescript
// Rate limit configuration
const rateLimits = {
  free: {
    api: 1000,      // requests per hour
    scripts: 5,     // per month
    analytics: 100  // requests per day
  },
  pro: {
    api: 10000,
    scripts: -1,    // unlimited
    analytics: 1000
  },
  business: {
    api: 50000,
    scripts: -1,
    analytics: 5000
  }
}
```

## 6. Scalability & Performance

### 6.1 Caching Strategy
```typescript
// Redis caching layers
const cacheConfig = {
  // User session: 24 hours
  'session:{userId}': { ttl: 86400 },

  // Channel analytics: 1 hour
  'analytics:{channelId}:{period}': { ttl: 3600 },

  // YouTube API responses: 30 minutes
  'youtube:channel:{channelId}': { ttl: 1800 },
  'youtube:video:{videoId}': { ttl: 1800 },

  // Script templates: 1 day
  'templates': { ttl: 86400 },

  // User quota: 1 hour
  'quota:{userId}:{resource}': { ttl: 3600 }
}
```

### 6.2 Database Optimization
- **Connection Pooling**: Max 20 connections per service
- **Query Optimization**: Indexed queries, avoid N+1
- **Read Replicas**: Separate read/write operations
- **Partitioning**: Time-based partitioning for analytics data
- **Archiving**: Move data older than 2 years to cold storage

### 6.3 CDN Strategy
```
Static Assets: CloudFront
- JavaScript bundles
- CSS files
- Images, fonts
- Video thumbnails

Cache-Control Headers:
- Static assets: max-age=31536000 (1 year)
- API responses: no-cache
- User content: max-age=3600 (1 hour)
```

### 6.4 Load Balancing
```
Application Load Balancer (ALB)
├── Target Group: Web App (Next.js)
│   ├── Instance 1 (auto-scaling)
│   ├── Instance 2
│   └── Instance N
├── Target Group: API Service
│   ├── Instance 1 (auto-scaling)
│   └── Instance N
└── Target Group: AI Service
    ├── Instance 1 (GPU-enabled)
    └── Instance 2
```

## 7. Monitoring & Logging

### 7.1 Application Monitoring
```typescript
// Key metrics to track
const metrics = {
  performance: [
    'api_response_time',
    'page_load_time',
    'database_query_time',
    'external_api_latency'
  ],
  business: [
    'script_generation_count',
    'active_users_daily',
    'subscription_conversions',
    'api_usage_by_tier'
  ],
  errors: [
    'error_rate',
    'failed_requests',
    'timeout_count',
    'external_api_failures'
  ]
}
```

### 7.2 Logging Strategy
```typescript
// Log levels and destinations
const logging = {
  error: {
    destination: ['Sentry', 'CloudWatch'],
    include: ['stack_trace', 'user_id', 'request_id']
  },
  warn: {
    destination: ['CloudWatch'],
    include: ['message', 'context']
  },
  info: {
    destination: ['CloudWatch'],
    include: ['event', 'user_id']
  },
  debug: {
    destination: ['CloudWatch'],
    enabled: process.env.NODE_ENV === 'development'
  }
}
```

### 7.3 Alerting Rules
```yaml
alerts:
  - name: High Error Rate
    condition: error_rate > 5%
    window: 5m
    severity: critical

  - name: API Response Time
    condition: p95_latency > 2s
    window: 5m
    severity: warning

  - name: Database Connection Pool
    condition: connection_usage > 80%
    window: 5m
    severity: warning

  - name: External API Failure
    condition: youtube_api_error_rate > 10%
    window: 5m
    severity: critical
```

## 8. Deployment Architecture

### 8.1 CI/CD Pipeline
```yaml
# GitHub Actions workflow
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    - Unit tests
    - Integration tests
    - E2E tests
    - Security scan (Snyk)

  build:
    - Build Docker images
    - Push to ECR
    - Tag with version

  deploy:
    - Deploy to staging
    - Run smoke tests
    - Deploy to production (blue-green)
    - Health check
    - Rollback on failure
```

### 8.2 Infrastructure as Code
```terraform
# Terraform modules
modules/
├── vpc/              # Network configuration
├── eks/              # Kubernetes cluster
├── rds/              # PostgreSQL database
├── elasticache/      # Redis cache
├── s3/               # Object storage
├── cloudfront/       # CDN
└── monitoring/       # CloudWatch, alarms
```

### 8.3 Environment Configuration
```
Development:
- Local Docker Compose
- Hot reload enabled
- Debug logging
- Mock external APIs

Staging:
- AWS ECS (Fargate)
- Separate database
- Limited rate limits
- Full external API integration

Production:
- AWS EKS (Kubernetes)
- Multi-AZ deployment
- Auto-scaling enabled
- Full monitoring and alerting
```

## 9. Disaster Recovery

### 9.1 Backup Strategy
```yaml
Database Backups:
  Automated:
    - RDS automated backups (daily)
    - Retention: 30 days
    - Point-in-time recovery

  Manual:
    - Weekly full backup
    - Monthly archival
    - S3 cross-region replication

Object Storage:
  - S3 versioning enabled
  - Cross-region replication
  - Lifecycle policies
```

### 9.2 Disaster Recovery Plan
```
RTO (Recovery Time Objective): 4 hours
RPO (Recovery Point Objective): 15 minutes

Procedures:
1. Database failure
   - Failover to RDS replica (automatic)
   - Promote read replica if needed

2. Regional outage
   - Redirect traffic to backup region
   - Restore from cross-region backup

3. Data corruption
   - Point-in-time recovery
   - Data validation checks
```

---

**Document Version**: 1.0
**Last Updated**: 2025-11-04
**Owner**: Engineering Team
**Status**: Draft
