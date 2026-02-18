# 4.5 Tournaments

## 1. Overview

Tournaments are the flagship feature of the NNS platform, driving the competitive ecosystem and providing the data that powers the Skill Rating System. The Tournament module is an end-to-end solution handling everything from discovery and registration to live scoring and results processing.

## 2. Discovery & Registration

### 2.1 The Tournament Feed
Users browse tournaments via a robust filtering interface (`src/app/tournaments`).
*   **Filters**: Format (2s, 4s), Gender, Date Range, Location.
*   **Status Indicators**: "Open", "Waitlist", "Sold Out", "In Progress".

### 2.2 Registration Flows
*   **Team Registration**: A user registers themselves and invites a partner. The team is "Incomplete" until the partner accepts the invite.
*   **Free Agent**: Individual players can register as "Looking for Team". Organizers or other free agents can pair up with them.
*   **Payment**: Integration with Stripe handles entry fees. 

## 3. Tournament Day (Live Mode)

### 3.1 The "Live Bracket"
Once a tournament starts, the platform shifts to a "Live Mode".
*   **Visuals**: Interactive bracket diagrams (Single/Double Elimination) and Pool Play tables.
*   **Updates**: Matches turn "Green" (Active) or "Grey" (Finished) in real-time.

### 3.2 Scoring
*   **Self-Refereed**: In many formats, players score their own games.
*   **Role-Based Access**: Winning teams or assigned referees enter scores directly into the app via a mobile-optimized modal.
*   **Validation**: Scores are validated against the format rules (e.g., "Must allow max 21 points").

## 4. The Algorithm (Under the Hood)

### 4.1 Seeding (`seeding.ts`)
The system automatically seeds teams based on their combined Glicko rating points.
*   **Logic**: `(PlayerA.rating + PlayerB.rating) / 2`.
*   **Overrides**: Organizers can manually adjust seeds for "wild cards".

### 4.2 Pool Generation (`tournament-logic.ts`)
Algorithms distribute teams into pools to ensure balanced play (Snake Seeding).
*   **Example**: Pool A gets Seeds 1, 8, 9, 16.

### 4.3 Advancement
*   **Rules Engine**: Organizers define advancement rules (e.g., "Top 2 from each pool go to Gold").
*   **Calculation**: The system calculates "Point Differential" and "Head-to-Head" to break ties automatically.

## 5. Post-Tournament

### 5.1 Results Processing
Once the final match concludes:
1.  **Rankings**: Final placements (1st, 2nd, 3rd) are crystallized.
2.  **Points Awarded**: Leaderboard points are distributed based on the "Broad Meritocracy" formula.
3.  **Rating Updates**: The Glicko-2 engine processes all match results to update player skill ratings.

### 5.2 Media
Participants can upload photos/videos to the "Tournament Gallery", which appear on the event page and participating user profiles.
