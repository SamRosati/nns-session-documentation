# 3.1 High-Level Design

## Executive Summary

The NNS Session (NNS) is a progressive web application (PWA) designed to modernize the management of beach volleyball communities. It addresses the fragmentation of current solutions by unifying tournament organization, clinic management, pickup sessions, and social interaction into a single, cohesive platform. The system is built on a robust, modern technology stack leveraging Next.js for the application framework, Supabase for backend services (Authentication, Database), and Firebase App Hosting for scalable deployment.

This document outlines the high-level architecture of the NNS platform, detailing the core components, data flow, and design principles that ensure scalability, maintainability, and a premium user experience.

## Architecture Principles

The architecture of NNS is guided by the following core principles:

1. **Server-First rendering (RSC)**: Leveraging React Server Components (RSC) to minimize Client-Side JavaScript, improve SEO, and ensure fast initial page loads.
2. **Edge-Ready Performance**: Utilizing edge networking for low-latency content delivery and API responses.
3. **Type Safety**: End-to-end type safety using TypeScript, from the database schema (via Supabase) to the API layer and frontend components.
4. **Component-Based UI**: A modular design system built on atomic design principles using Tailwind CSS and Radix UI primitives.
5. **Offline Capability**: Progressive Web App (PWA) architecture to support offline access and native-like installation on mobile devices.

## Technology Stack

### Frontend Layer

- **Framework**: **Next.js 15 (App Router)** - Provides the backbone for routing, server-side rendering, and API handling.
- **Language**: **TypeScript** - Ensures type safety and developer productivity.
- **State Management**:
  - **Server State**: **TanStack Query (React Query)** - Manages data fetching, caching, synchronization, and server state updates.
  - **Client State**: **Zustand** - Handles complex client-side state for interactive features like the Tournament Wizard and 3D Configurator (if applicable).
- **Styling**: **Tailwind CSS v4** - A utility-first CSS framework for rapid UI development.
- **UI Primitives**: **Radix UI** - Unstyled, accessible UI components for building high-quality design systems (Dialogs, Popovers, Accordions).
- **Animations**: **Tailwind CSS Animate** & **Framer Motion** (as needed) for micro-interactions.
- **Forms**: **React Hook Form** + **Zod** - Robust form handling and schema validation.

### Backend Services

- **Database**: **PostgreSQL (via Supabase)** - The primary relational database store.
- **Authentication**: **Supabase Auth** - Handles user registration, login (Email/Password, OAuth providers like Google/Apple), and session management.
- **Real-time**: **Supabase Realtime** - Enables live updates for tournament scores, messaging, and notifications.
- **Storage**: **Firebase Storage / Supabase Storage** - For storing user uploads (profile pictures, tournament media). _Note: Configuration references Firebase Storage buckets._
- **Payments**: **Stripe Connect** - Handles payment processing for tournament registrations and payout management for organizers.

### Infrastructure & Deployment

- **Hosting**: **Firebase App Hosting** - A serverless hosting platform optimized for Next.js, providing automatic scaling and global CDN distribution.
- **CDN**: **Google Cloud CDN** (via Firebase) - Caches static assets and ISR pages at the edge.
- **Environment**: Node.js 20 Runtime.

## System Components Diagram

```mermaid
graph TD
    Client[Client Device\n(Browser/PWA)] -->|HTTPS/WSS| CDN[Firebase CDN]
    CDN -->|Static Assets| Client
    CDN -->|Dynamic Requests| AppServer[Next.js App Server\n(Firebase App Hosting)]

    subgraph "Backend Services"
        AppServer -->|Auth/Data| Supabase[Supabase Platform\n(PostgreSQL + Auth)]
        AppServer -->|Payments| Stripe[Stripe API]
        AppServer -->|Media| Storage[Firebase/Supabase Storage]
    end

    subgraph "Database Layer"
        Supabase -->|Read/Write| DB[(PostgreSQL Database)]
        Supabase -->|Events| Realtime[Realtime Engine]
    end

    Realtime -.->|Live Updates| Client
    Stripe -.->|Webhooks| AppServer
```

## Core Modules

### Tournament Engine

The heart of the platform, the Tournament Engine manages the lifecycle of competitive events.

- **Responsibility**: Creation, scheduling, registration, bracket generation, scoring, and results.
- **Key Components**: `TournamentWizard`, `BracketViewer`, `ScoreEntry`, `Scheduler`.
- **Logic**: Complex logic resides in `src/lib/tournament-logic.ts` and `src/lib/scheduler.ts` to handle seeding, pool assignment, and playoff advancement.

### User & Profile System

Manages user identity, roles, and progression.

- **Responsibility**: Profiles, stats tracking, skill ratings, and role management (organizer, coach, admin).
- **Key Components**: `ProfileDashboard`, `StatsCard`, `RankHistory`.
- **Logic**: `src/lib/types.ts` defines the `UserProfile` schema, including the shadow rating system based on Glicko-2.

### Social Graph

Facilitates connection and community building.

- **Responsibility**: Following users, messaging, activity feeds, and notifications.
- **Key Components**: `FollowButton`, `ChatWindow`, `ActivityFeed`.
- **Logic**: `src/hooks/use-relationships.ts` and `src/hooks/use-inbox.ts` manage social interactions.

### Clinic & Session Management

Tools for coaching and casual play.

- **Responsibility**: Creating and registering for clinics and open pickup sessions.
- **Key Components**: `ClinicCard`, `SessionList`, `RegistrationModal`.
- **Logic**: State management via `ClinicsContext` and `SessionsContext`.

## Data Flow Architecture

### Server-Side Data Fetching

In the App Router, data fetching is primarily performed on the server.

1.  **Request**: User navigates to a page (e.g., `/tournaments`).
2.  **Server Component**: The page component (`page.tsx`) requests data directly from Supabase/DB using helper functions or direct queries.
3.  **Rendering**: Next.js renders the component tree with the fetched data into HTML/RSC payload.
4.  **Response**: The client receives the pre-rendered HTML for fast First Contentful Paint (FCP).

### Client-Side Interactivity & Updates

For interactive features and updates:

1.  **Interaction**: User clicks "Register" or updates a score.
2.  **Server Action**: A Server Action (`src/actions/*`) is invoked.
3.  **Mutation**: The action validates input (Zod) and updates the database via Supabase.
4.  **Revalidation**: The action calls `revalidatePath` to refresh the cached data.
5.  **UI Update**: The client UI updates automatically to reflect the new state without a full page reload.

### Real-time Updates

For critical live features like Tournament Scores:

1.  **Subscription**: Client components subscribe to Supabase Realtime channels (e.g., `matches` table changes).
2.  **Event**: When a score is updated in the DB, Supabase broadcasts an event.
3.  **Update**: The client subscription handler receives the delta and updates the local React Query cache or Zustand store, triggering an immediate UI re-render.

## Security & Privacy

- **Authentication**: Secure session management via Supabase Auth.
- **Authorization**: Row Level Security (RLS) policies in PostgreSQL ensure users can only access data they are permitted to see.
- **Data Validation**: Strict input validation using Zod schemas on both client and server to prevent injection attacks and ensure data integrity.
- **Payment Security**: All payment processing is offloaded to Stripe; no sensitive card data is stored on NNS servers.

## Performance Optimization Strategy

1.  **Route Groups & Layouts**: Efficiently organizing routes to minimize re-rendering of shared layouts.
2.  **Image Optimization**: Using `next/image` for automatic resizing, lazy loading, and format conversion (WebP/AVIF).
3.  **Code Splitting**: Automatic splitting of JavaScript bundles by route to reduce initial load size.
4.  **Dynamic Imports**: Lazily loading heavy components (e.g., Charts, Maps) only when they are needed.
5.  **Prefetching**: Next.js `Link` component prefetches viewport routes for near-instant navigation.

## Future Considerations

- **Native Mobile Apps**: Leveraging Capacitor to wrap the PWA for deployment to iOS App Store and Google Play Store.
- **AI Integration**: Planned integration of AI for automated tournament scheduling, skill assessment analysis, and personalized coaching tips (see details in AI Strategy doc).
- **Advanced Analytics**: Integration of a dedicated analytics platform for deeper user behavior insights.
