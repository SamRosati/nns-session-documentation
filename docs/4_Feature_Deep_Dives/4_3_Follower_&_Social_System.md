# 4.3 Follower & Social System

## 1. Overview

The Social System transforms NNS from a purely utility-based tool (tournament management) into a community platform. It drives engagement by allowing players to track their friends, rivals, and favorite organizers, creating a personalized feed of activity.

## 2. Core Features

### 2.1 Following Mechanics
The relationship model is built on a directional "Follow" graph (`UserId -> FollowingId`).
*   **Follow Players**: Get notified when they register for a tournament or post a result.
*   **Follow Organizers**: Receive alerts for new tournament announcements.
*   **Privacy Control**: Users can set their profile to "Private", requiring approval for follow requests (Logic in `use-relationships.ts`).

### 2.2 Activity Feed
Aggregates relevant events into a scrollable timeline on the home screen (`PlayerFeed`).
*   **Event Types**:
    *   `TournamentCreated`: An organizer you follow posted a new event.
    *   `Registration`: A friend registered for a tournament (driving FOMO).
    *   `Result`: A friend won a tournament or placed on the podium.
    *   `Post`: Manual status updates or media shares.

### 2.3 Profile Integration
Social stats are prominent on the `UserProfile`:
*   **Followers Count**: Social proof of influence.
*   **Following Count**: Network size.
*   **Mutuals**: (Planned) "You both know..." indicators.

## 3. Technical Implementation

### 3.1 Data Model
Social connections are stored in the `UserProfile` arrays to avoid expensive join tables for simple reads.
*   `followingIds`: Array of User IDs the user follows.
*   `followerIds`: Array of User IDs following the user.
*   *Note: For scale, this array approach works for thousands of followers. For millions, we would migrate to a dedicated `Relationships` table.*

### 3.2 Real-time Updates
*   **Optimistic UI**: Clicking "Follow" instantly updates the UI button state while the server request processes.
*   **Notifications**: Backend triggers send push notifications or in-app alerts when a follow occurs.

### 3.3 Discovery
*   **Global Search**: Integrated finding of other players by name.
*   **Suggestions**: (Planned) Algorithms recommending players based on mutual tournament participation.
