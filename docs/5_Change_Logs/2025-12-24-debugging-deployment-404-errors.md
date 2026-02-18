# Debugging Deployment 404 Errors

**Date:** 2025-12-24

## Objective
Resolve new 404 errors on deployed application scripts and assets.

## Changes
- Identified "zombie" Service Worker as root cause.
- Inspected `firebase.json` and `package.json` configuration.
- Implemented mechanism to forcefully unregister Service Workers.
- Rebuilt and redeployed application.
