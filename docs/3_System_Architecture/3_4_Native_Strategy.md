# 3.4 Native Strategy

## 1. Overview

NNS adopts a "Progressive Web App (PWA) First" strategy, leveraging modern web capabilities to deliver a native-like experience on both iOS and Android directly from the browser. This approach allows for rapid feature deployment, unified codebase maintenance, and broad accessibility without the friction of app store verification processes during the initial growth phase.

In the future, the platform is architected to be wrapped in a native container (Capacitor or React Native Web) for App Store distribution when specific native-only features are required.

## 2. PWA Implementation

### 2.1 Configuration
The project uses `@ducanh2912/next-pwa` to automatically generate the service worker and manifest files during the build process.

*   **Manifest**: Defines the app name ("NextN"), icons, theme colors (`#18181b` for dark mode), and display mode (`standalone`).
*   **Service Worker**: Caches critical assets and API responses to enable offline functionality and faster subsequent loads.
*   **Installability**: The app prompts users to "Add to Home Screen" (A2HS) on supported devices.

### 2.2 Native-Like Features
*   **Standalone Mode**: Removes the browser address bar and UI controls, giving the app the full screen real estate.
*   **Push Notifications**: (Planned) Using Firebase Cloud Messaging (FCM) via the service worker to send tournament alerts and chat notifications even when the app is closed.
*   **Touch Interactions**: UI components (swipeable lists, bottom sheets) are designed with touch targets and gestures in mind using Radix UI and Tailwind.
*   **Deep Linking**: Standard web URLs (e.g., `/tournaments/abc-123`) serve as deep links, allowing users to share content easily via SMS or social media.

## 3. iOS Specifics

iOS (Safari) has historically had stricter PWA limitations, but recent updates have improved support.
*   **Overscroll Behavior**: CSS `overscroll-behavior-y: none` prevents the "rubber band" effect on the body, simulating a native scroll view.
*   **Safe Areas**: `safe-area-inset-*` CSS variables are used to ensure content isn't obscured by the notch or home indicator.
*   **Splash Screens**: Custom splash screens are generated to provide a smooth launch experience.

## 4. Android Specifics

*   **WebAPK**: On Android, installing the PWA generates a WebAPK, which designates the app as a top-level native application in the OS, managing its own storage and appearing in the app drawer.
*   **Background Sync**: (Planned) leveraging the Background Sync API to retry failed network requests (e.g., score submission) when connectivity is restored.

## 5. Future Roadmap: Capacitor Migration

As the user base grows and requirements evolve, we plan to use **Capacitor** to wrap the Next.js application into a native binary.

### Why Capacitor?
*   **Single Codebase**: We continue to write standard React/Next.js code.
*   **Native Plugins**: Access to native SDKs like Contacts, Geolocation (high precision), and Haptics.
*   **App Store Presence**: Allows listing on the Apple App Store and Google Play Store for discovery and credibility.

### Migration Steps
1.  **Initialize Capacitor**: `npm install @capacitor/core @capacitor/cli`
2.  **Configure Config**: Point Capacitor to the Next.js build output (`out` or `public`).
3.  **Add Platforms**: `npx cap add ios` / `npx cap add android`.
4.  **Sync**: `npx cap sync` to copy web assets to native projects.
5.  **Build & Deploy**: Use Xcode and Android Studio to build the final IPA/APK.

## 6. Current Limitations & Mitigations

| Limitation | Mitigation |
| :--- | :--- |
| **Push Notifications (iOS)** | Requires iOS 16.4+ and user must "Add to Home Screen". We use in-app toasts and SMS fallback (Twilio, planned) for critical alerts. |
| **Offline Data** | We use `react-query` persistence and `localStorage` to cache tournament data for offline viewing. Full offline-write capability is complex and currently limited to simple actions. |
| **Performance** | Use RSC and Code Splitting to ensure the JS bundle remains small, keeping the app responsive even on mid-range devices. |
