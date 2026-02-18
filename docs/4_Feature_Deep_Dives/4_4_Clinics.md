# 4.4 Clinics

## 1. Overview

Clinics serve as the educational pillar of the NNS platform, enabling experienced players and professional coaches to offer training sessions to the community. This feature allows coaches to monetize their expertise while providing players with structured opportunities to improve their skills (and their Glicko rating).

## 2. The Coach Experience

### 2.1 Clinic Creation
Accessible via the "Coach Dashboard", the creation flow is streamlined to allow quick setup of training events.
*   **Essential Details**: Title ("Advanced Defense"), Date, Time, Duration.
*   **Location**: Integrated with Google Places for precise court location.
*   **Capacity**: Strict limit on participants (e.g., "Max 8 players") to ensure quality instruction.
*   **Pricing**: Integration with Stripe Connect allows coaches to set a fee (e.g., $20/player) and receive payouts directly.

### 2.2 Roster Management
Coaches have a real-time view of their clinic roster.
*   **Attendee List**: View names and profiles of registered players.
*   **Communication**: One-click "Message All" feature to send updates (e.g., generic weather alerts or specific instructions).

## 3. The Player Experience

### 3.1 Discovery
Clinics appear alongside tournaments in the main event feed but are visually distinct (often color-coded or using specific icons).
*   **Filtering**: Players can filter for "Clinics Only" or by skill level (e.g., "Beginner Friendly").

### 3.2 Registration
*   **One-Tap Sign Up**: If the clinic is free or the user has a saved payment method.
*   **Waitlist**: If a clinic is full, players can join a waitlist and are auto-notified if a spot opens up.

## 4. Technical Architecture

### 4.1 Data Model
Defined in `types.ts` as `Clinic`:
*   `coachId`: Links to the `UserProfile` of the organizer.
*   `registeredPlayerIds`: Array of User IDs.
*   `earnings`: (Private) Tracks revenue for the coach.

### 4.2 State Management
The `ClinicsContext` handles the loading and caching of clinic data.
*   **Efficiency**: Clinics are often fetched in batches with tournaments to populate the main calendar view.

## 5. Future Enhancements

*   **Recurring Clinics**: "Every Tuesday at 6 PM" configurations.
*   **Feedback Loop**: automatic post-clinic survey sending to attendees to rate the session and provide testimonials for the coach.
