# 4.1 Skill Rating System Data Structure

## Overview

The NNS Skill Rating System is a sophisticated meritocratic framework designed to accurately reflect a player's ability relative to the community. It moves beyond simple point accumulation by employing a customized implementation of the **Glicko-2** rating algorithm. This system accounts for skill uncertainty (Rating Deviation) and consistency (Volatility) to provide fair matchmaking and reliable rankings.

## Core Concepts

### 2.1 The Glicko-2 Algorithm

Unlike Elo, which tracks only a single number (Rating), Glicko-2 tracks three parameters for every player:

1. **Rating (`r`)**: The best estimate of a player's skill.
    - **Default**: 1500
    - **Scale**: Similar to chess ELO, but optimized for volleyball dynamics.
2. **Rating Deviation (`RD`)**: A measure of the uncertainty in the rating.
    - **High RD**: The system is unsure; rating changes will be volatile (large jumps).
    - **Low RD**: The system is confident; rating changes will be stable.
    - **Default**: 350 (High uncertainty).
    - **Decay**: RD increases over time if a player is inactive (the "rust" factor).
3. **Volatility (`σ`)**: A measure of a player's consistency.
    - **Low Volatility**: Player performs predictably.
    - **High Volatility**: Player has erratic results (e.g., beats pros but loses to amateurs).
    - **Default**: 0.06.

### 2.2 Shadow Rating vs. Display Rating

To prevent "ladder anxiety" and confusion, the system maintains two layers:

- **Shadow Rating (Internal)**: The raw Glicko-2 numbers used for seeding and math.
- **Skill Tier (Public)**: A simplified label or badge (e.g., "A", "AA", "Open") derived from the rating range.

## 3. Data Structure

As defined in `src/lib/types.ts`, the rating data is stored directly on the `UserProfile` object:

```typescript
type UserProfile = {
  // ...
  skillRating?: {
    rating: number; // Current Skill Estimate (e.g., 1650.4)
    ratingDeviation: number; // Uncertainty (e.g., 85.2)
    volatility: number; // Consistency (e.g., 0.059)
    lastUpdated: string; // ISO Date of last calculation
    matchCount: number; // Total rated matches played

    // Placement Logic
    placementGamesPlayed: number;
    isRated: boolean; // True if matchCount >= 10
  };
  skillLabel?:
    | "Local Recreational"
    | "VBC 1-Star Competitor"
    | "National/Open Level";
};
```

## 4. Rating Calculation Process

### 4.1 Match Result Processing

Ratings are updated after every verified tournament match. The process logic resides in `src/lib/ranking-system.ts`.

1. **Input**: A match result (Team A vs. Team B, Score).
2. **Team Rating Aggregation**:
    - For doubles, the team rating is the average of the two players' ratings.
    - For 4s, it is the average of the top 3 players (to mitigate carrying).
3. **Expectation Calculation**: The system calculates the probability of Team A winning based on the rating difference.
4. **Update Step**:
    - If the result matches expectation (Favorite wins), ratings change slightly.
    - If an upset occurs (Underdog wins), ratings change significantly.
    - The `RD` decreases for both teams (uncertainty reduces).

### 4.2 Audit Logging

Every change is recorded in the `UserRatingHistory` table for transparency and debugging:

```typescript
type UserRatingHistory = {
  id: string;
  userId: string;
  tournamentId: string;
  oldRating: number;
  newRating: number;
  change: number;
  timestamp: string;
};
```

## 5. Ranking Tiers

The system maps numerical ratings to human-readable tiers for the UI:

| Tier           | Rating Range | Description                                |
| :------------- | :----------- | :----------------------------------------- |
| **Pro / Open** | 2000+        | Professional or top-level national talent. |
| **AAA**        | 1850 - 1999  | Elite amateur competitors.                 |
| **AA**         | 1700 - 1849  | Advanced players with strong fundamentals. |
| **A**          | 1550 - 1699  | Intermediate competitive players.          |
| **BB**         | 1400 - 1549  | Experienced recreational players.          |
| **B**          | < 1400       | Novice or purely recreational players.     |

## 6. Implementation References

- **Logic**: `src/lib/ranking-system.ts`
- **Types**: `src/lib/types.ts`
- **UI Integration**: `ProfileCard` displays the current rating trend.
