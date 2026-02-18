# Security Rules Overhaul Walkthrough

**Date:** 2026-01-29

## Summary of Changes
We have completely overhauled the `firestore.rules` file to move from a testing "allow all" state to a production-ready, role-based security model.

## Key Security Concepts Implemented

1.  **Authentication Requirement**: Most data requires a valid logged-in user (`request.auth != null`).
2.  **Role-Based Access Control (RBAC)**:
    - **Admins**: Users with an `'admin'` role in their profile can write to almost any collection.
    - **Owners**: Users can generally only edit their own data (e.g., specific fields in their User Profile).
    - **Organizers**: Organization managers can edit tournaments and details for organizations they manage.
3.  **Public vs. Private Data**:
    - **Public Read**: Tournaments, Organizations, Sessions (to allow sharing and viewing without login).
    - **Authenticated Read**: User profiles (requires login to view others).
    - **Strict Writes**: Strictly restricted to owners, organizers, or admins.

## Collection-Specific Rules

| Collection | Read Policy | Write Policy |
| :--- | :--- | :--- |
| `users` | Authenticated | Owner (Self) or Admin |
| `tournaments` | Public | Admin or Organizer |
| `organizations` | Public | Admin or Organizer |
| `sessions` | Public | Host or Admin |
| `conversations` | Members Only | Members or Admin |
| `messages` | Conversation Members | Sender (Self) or Admin |
| `applications` | Owner or Admin | Owner or Admin |
| `sub_requests` | Public | Creator or Admin |
| `clinics` | Public | Coach or Admin |
| `highlights` | Public | Owner or Admin |

## Files Changed
- `firestore.rules`: Replaced with new version containing helper functions and specific `match` blocks.
