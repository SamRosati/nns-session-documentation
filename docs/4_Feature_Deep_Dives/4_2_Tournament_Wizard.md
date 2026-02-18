# 4.2 Tournament Wizard

## Overview

The Tournament Wizard is a sophisticated, multi-step form interface designed to guide organizers through the complex process of creating and configuring a volleyball tournament. Given the myriad of variables involved in tournament logistics (court availability, team sizes, pool structures, playoff formats), the Wizard abstracts this complexity into logical, digestible steps.

## User Flow

The Wizard is implemented as a stateful process using `Zustand` to manage the draft tournament configuration across steps.

### Step 1: Basic Information (`step-1-basics.tsx`)

Establishes the fundamental identity of the event.

- **Event Details**: Name, Date, Start Time.
- **Format Selection**:
  - **Size**: 2s (Doubles) or 4s.
  - **Gender/Mix**: Men's, Women's, Coed, Reverse Coed.
- **Location**: Integrated with Google Places API for precise geolocation (used for map views).
- **Description**: Rich text editor for event details (rules, prizes).

## Step 2: Constraints & Logistics (`step-2-constraints.tsx`)

The "math engine" of the tournament. This step is critical for ensuring the event runs on time.

- **Capacity Planning**:
  - Number of Courts.
  - Maximum Teams.
- **Time Management**:
  - Registration Open/Close times.
  - Match Duration estimates (e.g., 30 mins per game).
- **Pool Configuration**:
  - **Automatic Calculation**: The system suggests optimal pool sizes (e.g., "For 24 teams on 6 courts, use 6 pools of 4").
  - **Wave Management**: Supports morning/afternoon waves if team count exceeds court capacity.
- **Playoff Structure**:
  - **Divisions**: Split into Gold/Silver/Bronze brackets based on pool performance.
  - **Seeding method**: Power scedule vs Pool Play results.

## Step 3: Financials & Registration (Planned)

- **Entry Fee**: Per team or per player.
- **Payout Structure**: Automated prize pool calculation.
- **Registration Rules**: Waitlist policy, partner requirements.

## Technical Implementation

### State Management (`wizard-store.ts`)

A dedicated Zustand store (`useWizardStore`) holds the partial `Tournament` object. This allows users to navigate back and forth between steps without losing data.

- **Persistence**: The store effectively acts as a draft buffer before the final submission to the database.

### Dynamic Validation

The wizard employs real-time validation to prevent impossible configurations.

- **Example**: If an organizer selects "8 Courts", "30 Minutes per game", and "128 Teams", the system will flag that the tournament would take 16+ hours to complete, prompting the user to either add courts, reduce teams, or shorten games.

### Database Interaction

Upon completion:

1. The `Tournament` object is constructed from the store state.
2. A `createTournament` Server Action is triggered.
3. The tournament is saved to Supabase with the status `upcoming`.
4. The organizer is redirected to the `Tournament Dashboard` to begin managing registrations.

## UI/UX Considerations

- **Progress Indicator**: A stepper component visually indicates the current stage.
- **Smart Defaults**: Fields are pre-populated with sensible defaults (e.g., "Standard 21-point games") to speed up creation.
- **Mobile Responsiveness**: The wizard is fully responsive, allowing organizers to create events from a tablet or phone at the beach.
