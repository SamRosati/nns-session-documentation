# Debugging Auth and SW

**Date:** 2025-12-21

## Objective
Resolve authentication issues and broken Service Worker blocking app function.

## Changes
- Investigated `ReferenceError` in `sw.js`.
- Fixed `useUser` hook to correctly fetch user data.
- Cleared broken Service Worker caches.
