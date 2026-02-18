# 3.2 Codebase Structure & Optimization

## 1. Directory Structure

The NNS codebase follows a standard Next.js App Router structure, organized to separate concerns between routing, core logic, UI components, and global utilities.

### Root Directory
*   `src/`: The main source code directory.
*   `public/`: Static assets (images, icons, PWA manifest).
*   `docs/`: Project documentation.
*   `.firebase/`: Firebase configuration and build artifacts.
*   `next.config.ts`: Next.js configuration (PWA settings, image domains, headers).
*   `tailwind.config.ts`: Tailwind CSS configuration (theme, colors, plugins).

### Source Directory (`src/`)

#### `app/` (Next.js App Router)
Contains the application's routing logic. Each folder represents a route segment.
*   `layout.tsx`: Root layout defining global UI (navbar, footer, fonts).
*   `page.tsx`: The home page.
*   `admin/`: Routes for administrative tasks (User Management, Tournament Creation).
*   `organizer/`: Organizer dashboard and tournament management tools.
*   `tournaments/`: Public tournament listings and details pages.
*   `profile/`: User profile viewing and editing.
*   `auth/`: Authentication routes (Sign In, Sign Up, Callback).
*   `api/`: Backend API routes (Webhooks, internal APIs).

#### `lib/` (Core Logic)
Contains the business logic, types, and utility functions. This layer is framework-agnostic where possible.
*   `types.ts`: **The single source of truth for data models** (UserProfile, Tournament, Match, etc.).
*   `supabase/`: Supabase client initialization and helper functions.
*   `tournament-logic.ts`: Complex logic for bracket generation, scoring, and advancement.
*   `scheduler.ts`: Algorithms for optimizing match schedules.
*   `ranking-system.ts`: Implementation of the skill rating system (Glicko-2 based).
*   `utils.ts`: General utility functions (date formatting, cn).

#### `components/` (UI Components)
Reusable React components, organized by feature or type.
*   `ui/`: Atomic UI primitives (Buttons, Inputs, Modals) using Radix UI + Tailwind.
*   `tournaments/`: Tournament-specific components (BracketViewer, MatchCard).
*   `profile/`: Profile-related components (StatsCard, AvatarUpload).
*   `shared/`: Common components used across multiple features (Navbar, Footer, Loaders).

#### `hooks/` (Custom Hooks)
Encapsulated state logic and side effects.
*   `use-auth.ts`: Authentication state and methods.
*   `use-toast.ts`: Toast notification logic.
*   `use-tournament.ts`: Tournament data fetching and manipulation.

#### `actions/` (Server Actions)
Server-side functions called directly from client components.
*   `auth-actions.ts`: Login, logout, registration actions.
*   `tournament-actions.ts`: Create/Update tournament, register team.

#### `styles/`
Global styles and CSS modules.
*   `globals.css`: Tailwind directives and global variable overrides.

## 2. Optimization Strategies

### 2.1 Server-Side Rendering (SSR) & Server Components (RSC)
*   **Strategy**: Maximize the use of React Server Components (RSC) to reduce the client-side JavaScript bundle.
*   **Usage**: All page roots (`page.tsx`) and non-interactive components are Server Components by default.
*   **Benefits**: Faster First Contentful Paint (FCP), improved SEO, and lower device resource usage.

### 2.2 Image Optimization
*   **Strategy**: Use `next/image` component for all images.
*   **Configuration**:
    *   Automatic format selection (WebP/AVIF).
    *   Lazy loading for images below the fold.
    *   Specific sizes prop for responsive serving.
*   **Remote Patterns**: Configured in `next.config.ts` to securely load images from authorized domains (Google Storage, Placehold.co).

### 2.3 Caching & Revalidation
*   **Strategy**: Leverage Next.js Data Cache to minimize database load.
*   **Implementation**:
    *   `fetch` requests are cached by default.
    *   **On-Demand Revalidation**: Server Actions call `revalidatePath` or `revalidateTag` to purge stale cache entries after mutations (e.g., updating a score).
    *   **Time-Based Revalidation**: Dynamic pages (like live scores) use a short revalidation interval (e.g., `revalidate = 60`) or real-time subscriptions.

### 2.4 Code Splitting & Dynamic Imports
*   **Strategy**: Split code bundles to load only what is needed for the current route.
*   **Implementation**:
    *   Next.js automatically splits code by route segments.
    *   **`next/dynamic`**: Used for heavy client components (e.g., complex charts, map integrations) to load them only when visible or needed.

### 2.5 Font Optimization
*   **Strategy**: Use `next/font` for self-hosting and optimizing Google Fonts.
*   **Usage**: Fonts (e.g., Inter, Outfit) are loaded layout-wide with zero layout shift (CLS).
*   **Optimization**: Only required subsets (e.g., Latin) are downloaded.

### 2.6 Bundle Analysis
*   **Tool**: `@next/bundle-analyzer` (optional dev tool).
*   **Process**: Regularly analyze bundle sizes to identify large dependencies and tree-shaking issues.

## 3. Best Practices

*   **Type Safety**: Strict TypeScript usage throughout the stack. `src/lib/types.ts` is the central definition.
*   **Colocation**: Keep components, tests, and styles collocated where it makes sense (feature-folder architecture).
*   **Server Actions**: Prefer Server Actions over API routes for form submissions and mutations to simplify data flow.
*   **Error Handling**: Use `error.tsx` boundaries to gracefully handle runtime errors without crashing the entire app.
