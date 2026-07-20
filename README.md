# Facloth

A cross-platform mobile app built with React Native, Expo, and TypeScript where users can showcase their outfits, find style inspiration, and connect with a fashion-focused community.

I started this project to get hands-on experience handling media-heavy feeds on mobile devices. The main goal was keeping the app smooth and fast while dealing with automated video playback, deep database nesting, and real-time state updates without draining the phone's performance.

Building Facloth taught me a lot about structuring non-relational databases for social features and handling user privacy logic securely. It gave me practical experience managing data updates on the client side, building smooth navigation transitions, and configuring structural queries to avoid database slowdowns.

## Features

* **Outfit Discovery Feeds:** Visual grids that load high-resolution outfit photos quickly and efficiently.
* **Inline Showcase Playback:** Video integration that automatically loops short video showcases on mute directly inside the feed.
* **Social Connections Graph:** A follow network that instantly updates follower and following counts across profiles.
* **Privacy & Block Controls:** A reliable moderation system that handles immediate, bidirectional blocking across profiles, posts, and activities.

## Tech Stack

* **Mobile:** React Native, Expo SDK, TypeScript
* **Database & Infrastructure:** Cloud Firestore, Firebase Authentication, Firebase Storage
* **Animation & Media:** React Native Reanimated, Expo Video, Expo Image Picker

## Technical Highlights

### Database & Index Optimization
The app avoids slow database operations by isolating user data into structured sub-collections. To look up data across these nested paths safely without query failures, I manually set up Single Field Index Exemptions with a Collection Group Scope in the Firebase console. This ensures that lookups filter user keys efficiently without running into missing index errors.

### Custom Interceptor Lifecycles
To keep the design completely consistent, I replaced native OS alerts with custom modal components. The warning interfaces rely on simple React state hooks (`showBlockWarning`, `showDeleteWarning`) to toggle visibility. The modal container uses absolute dimensions and explicit z-index layering to temporarily block underlying layout interactions while a user confirms an action.

### Security & Data Protection
All database writes and deletions are strictly bound to verified user tokens via the authentication pipeline. Before changing any data, the app verifies that the target document’s owner ID matches the active user's session token, ensuring users can only modify their own content.

## Screenshots

## Future Improvements

* **Serverless Backend Migration:** Moving complex multi-document deletions and cascading data cleanups to automated Firebase Cloud Functions using backend recursive scripts.
* **AI Outfit Picker:** Implementing an intelligent recommendation system to suggest outfit combinations based on the user's logged wardrobe.
* **Visual Style Search:** Integrating an AI-driven image recognition pipeline to scan outfits and find similar pieces of clothing across the internet.
* **Batch Transactions:** Grouping social follow actions into single atomic Firestore write batches to guarantee absolute consistency across profile metrics.
