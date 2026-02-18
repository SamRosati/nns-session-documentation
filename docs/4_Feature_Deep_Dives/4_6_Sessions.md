# 4.6 Sessions

## 1. Overview

Sessions (or "Pickup Games") represent the casual, decentralized side of beach volleyball. Unlike tournaments (organized, competitive) or clinics (educational), Sessions are about finding people to play with right now or in the near future. This feature empowers any user to be an organizer of casual play.

## 2. Core Functionality

### 2.1 Creation
Any user can create a Session.
*   **Visibility Control**:
    *   **Public**: Open to anyone on the platform. Great for growing a group.
    *   **Followers Only**: Visible only to people who follow the host. Good for maintaining a vetted quality of play.
    *   **Private/Invite Only**: Hidden from feeds; accessible only via direct link or invite.
*   **Level Targeting**: Hosts can tag the session with a skill level (e.g., "AA/Open" or "B/Recreational") to set expectations.

### 2.2 RSVP System
The session manages the "Who's In" list.
*   **Status**: `accepted`, `pending`, `waitlist`.
*   **Min/Max Players**: Hosts can set a minimum number of players needed for the session to go ahead (e.g., "Need 4 for 2s") and a hard cap to prevent overcrowding.

### 2.3 Session Log (Score Tracking)
During or after the session, players can log generic game results.
*   **Purpose**: While these games don't typically affect the official Glicko rating (to prevent abuse), they contribute to "Court Time" stats and activity feeds.

## 3. The "Sub Request" System
A specialized type of Session/Post designed for immediate needs.
*   **Scenario**: "We have a 4s team but one player dropped out last minute."
*   **Mechanism**: A "Sub Request" is broadcast to relevant players (filtered by gender/skill) in the local area via push notification.
*   **Acceptance**: The first qualified player to "Accept" fills the spot.

## 4. Technical Implementation

### 4.1 Data & Context
*   **`Session` Type**: Includes `hostId`, `location`, `dateTime`, `visibility`.
*   **`SessionsContext`**: Manages the subscription to session updates.

### 4.2 Geolocation
Sessions rely heavily on location services.
*   **`geolib`**: Used to calculate distance from the user ("Typically plays at Kitsilano Beach") to the session location, sorting the feed by proximity.
