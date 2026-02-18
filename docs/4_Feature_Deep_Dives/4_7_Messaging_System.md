# 4.7 Messaging System

## Overview

The Messaging System facilitates direct communication between players, partners, and organizers. It is crucial for coordinating partners ("Do you want to play in this tourney?"), logistical questions ("What time does check-in start?"), and social banter.

## Conversation Types (`types.ts`)

### Direct Messages (DM)

- **Participants**: 1-on-1 chat.
- **Use Case**: Finding a partner, private coordination.

### Event Chats (Group)

- **Participants**: All registered players of a specific tournament or clinic.
- **Feature**: Automatically created when an event is published.
- **Use Case**: Organizer announcements ("Rain delay until 10 AM"), lost & found, general hype.

### Team Chats (Group)

- **Participants**: Teammates (2 or 4 players).
- **Use Case**: Strategy discussion, carpooling.

## Architecture

### Real-time Engine

Powering the chat is **Supabase Realtime**.

- **Channel Subscription**: The client subscribes to `room:conversation_id`.
- **Event Listeners**: Listens for `INSERT` events on the `messages` table.
- **Delivery**: Messages appear instantly without polling.

### Data persistence

Messages are stored in Postgres for history retrieval.

- **Table**: `messages`
- **Foreign Keys**: `sender_id`, `conversation_id`.
- **Attachments**: Image/Video URLs stored in `attachments` JSON column.

### Hook Logic (`use-chat.ts`)

- **`sendMessage(text, attachments)`**: Optimistically adds the message to the local UI state, then sends to Supabase.
- **`markAsRead(messageId)`**: Updates the read receipt status.
- **`useConversations()`**: Fetches the list of active threads sorted by `lastMessageAt`.

## UI Components

- **`ChatWindow`**: The main interface. Handles scrolling, message bubbles, and input.
- **`ConversationList`**: Sidebar showing active chats with unread indicators.
- **`MessageBubble`**: Renders text, emojis, and media previews. Distinguishes between "Me" (right aligned) and "Them" (left aligned).

## Security & Safety

- **Block Lists**: Users can block others interactions; implemented via RLS policies preventing message delivery.
- **Report**: Ability to flag abusive content for admin review.
