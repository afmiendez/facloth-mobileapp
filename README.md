# Facloth

A cross-platform mobile app built with React Native, Expo, and TypeScript where users can showcase their outfits, find style inspiration, and connect with a fashion-focused community.

I started this project to get hands-on experience handling media-heavy feeds on mobile devices. The main goal was keeping the app smooth and fast while dealing with automated video playback, deep database nesting, and real-time state updates without draining the phone's performance.

Building Facloth taught me a lot about structuring non-relational databases for social features and handling user privacy logic securely. It gave me practical experience managing data updates on the client side, building smooth navigation transitions, and configuring structural queries to avoid database slowdowns.

## Features

* **Outfit Discovery Feeds:** Visual grids that load high-resolution outfit photos quickly and efficiently.
* **Showcases (Short Video Page):** Dedicated interface rendering full-screen short-form videos for quick outfit logging and inspiration.
* **Real-time Messaging & DMs:** Private direct messaging subsystem utilizing active listeners to deliver messages instantly within target chat rooms.
* **Inbox & Activity Notifications:** A unified notification center that tracks unread messages, direct follows, and social alerts in real-time.
* **Social Connections Graph:** A follow network that instantly updates follower and following counts across profiles.
* **Privacy & Block Controls:** A reliable moderation system that handles immediate, bidirectional blocking across profiles, posts, chats, and notifications.

## Tech Stack

* **Mobile:** React Native, Expo SDK, TypeScript
* **Database & Infrastructure:** Cloud Firestore, Firebase Authentication, Firebase Storage
* **Animation & Media:** React Native Reanimated, Expo Video, Expo Image Picker

## Technical Highlights

### Database & Index Optimization
The app avoids slow database operations by isolating user data into structured sub-collections. To look up data across these nested paths safely without query failures, I manually set up Single Field Index Exemptions with a Collection Group Scope in the Firebase console. This ensures that lookups filter user keys efficiently without running into missing index errors.

### Security & Data Protection
All database writes and deletions are strictly bound to verified user tokens via the authentication pipeline. Before changing any data, the app verifies that the target document’s owner ID matches the active user's session token, ensuring users can only modify their own content.

### Production Lifecycle & Heavy Testing
Subjected the entire application layer to thorough system integration and black-box testing routines to map layout bottlenecks, media playback latency, and asynchronous data conflicts. Successfully handled the end-to-end bundling, compilation, and distribution release cycle directly to the Google Play Store environment.

## Screenshots
<img width="383" height="837" alt="image" src="https://github.com/user-attachments/assets/9952bcac-0e94-4ae0-86e6-2006ad2df890" /> <img width="373" height="831" alt="image" src="https://github.com/user-attachments/assets/7807a523-79af-470e-96cb-3ac28bd54f3a" />



## Future Improvements

* **Serverless Backend Migration:** Moving complex multi-document deletions and cascading data cleanups to automated Firebase Cloud Functions using backend recursive scripts.
* **AI Outfit Picker:** Implementing an intelligent recommendation system to suggest outfit combinations based on the user's logged wardrobe.
* **Visual Style Search:** Integrating an AI-driven image recognition pipeline to scan outfits and find similar pieces of clothing across the internet.
* **Batch Transactions:** Grouping social follow actions into single atomic Firestore write batches to guarantee absolute consistency across profile metrics.
