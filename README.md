# Facloth

A cross-platform mobile app built with React Native, Expo, and TypeScript where users can showcase their outfits, find style inspiration, and connect with a fashion-focused community.

I started this project to get hands-on experience handling media-heavy feeds on mobile devices, and it grew into a full production app: an AI-powered outfit recommendation engine, a subscription/paywall system, automated content moderation, and a live Google Play Store release. The goal throughout was keeping the app smooth and fast while dealing with deep database nesting, real-time state updates, and several third-party API integrations without draining the phone's performance.

Building Facloth taught me a lot about structuring non-relational databases for social features, integrating an LLM into a real product pipeline (not just a chatbot), and handling user privacy and payment logic securely end to end — from Firestore security rules to a live Play Store submission.

## Features

* **Outfit Discovery Feed:** A ranked, paginated feed of user-posted outfits with a lightweight personalization pass (follows + interaction history) and diversified authorship, so the feed doesn't get dominated by a single prolific poster.
* **AI Outfit Builder:** A guided wizard (budget, style, gender, preferred brands) that searches a live product catalog spanning ~25,000 retailers, then uses Claude to visually re-rank candidate combinations for color and style coherence and write a short styling note — with a deterministic rule-based fallback so a result is always returned even if the AI pass fails or is refused.
* **Subscriptions & Monetization:** A RevenueCat-backed Google Play Billing subscription gating premium Outfit Builder access, with a daily scheduled Cloud Function that reconciles Firestore's "is this user Pro" flag against RevenueCat's own record, so entitlement never silently drifts out of sync after a cancellation.
* **Automated Content Moderation:** Every uploaded photo is screened by Google Cloud Vision's SafeSearch API on upload, and every post/comment's text is screened by Google Cloud Natural Language, before anything goes live — flagged content is auto-removed, the poster is notified in-app, and reports are triaged through a lightweight internal admin console.
* **Inbox & Activity Notifications:** A unified notification center tracking unread likes, comments, and follows in real time via a bounded live Firestore listener, plus push notifications that deep-link a tapped notification straight to the relevant post or profile, even from a cold start.
* **Social Connections Graph:** A follow network that instantly updates follower/following counts across profiles.
* **Privacy & Block Controls:** A reliable moderation system that handles immediate, bidirectional blocking across profiles, posts, and notifications.
* **Localized Pricing:** Outfit Builder budgets display in the user's own device currency via live exchange rates, while all search and filtering logic stays denominated in USD internally.
* **Fashion News Feed:** A scheduled Cloud Function curates fashion headlines into a dedicated in-app feed.

## Tech Stack

* **Mobile:** React Native, Expo SDK, TypeScript, Expo Router
* **Database & Infrastructure:** Cloud Firestore, Firebase Authentication, Firebase Storage, Firebase Cloud Functions (2nd gen)
* **AI / ML:** Anthropic Claude API (outfit visual re-ranking), Google Cloud Vision API (image moderation), Google Cloud Natural Language API (text moderation)
* **Commerce:** RevenueCat, Google Play Billing
* **Animation & Media:** React Native Reanimated, Expo Image
* **Monitoring:** Sentry

## Technical Highlights

### Database & Index Optimization
The app isolates user data into structured sub-collections to keep per-post writes cheap, and uses Firestore collection-group queries (e.g. "every post I've liked," across every post's own `likes/` subcollection) backed by explicit collection-group field-override indexes — supporting cross-post lookups without a single, ever-growing flat collection.

### Automated, Cascading Data Cleanup
Firestore subcollections don't cascade-delete on their own, so deleting a post used to leave orphaned likes and comments behind. An `onDocumentDeleted` Cloud Function trigger now cleans up every subcollection automatically, safely batched under Firestore's 500-write-per-batch limit.

### AI Integration Beyond a Chatbot
The Outfit Builder doesn't just call an LLM and print the response — it feeds Claude a real, in-stock candidate pool (already filtered by price, gender, and style) alongside each product's actual photo, constrains the response to a strict JSON schema so it can only select from ids it was actually given, and falls back to a deterministic rule-based ranker if the AI call fails or is refused — so a broken AI response degrades gracefully instead of breaking the feature.

### Security & Data Protection
All database writes and deletions are strictly bound to verified user tokens via the authentication pipeline — Firestore security rules re-derive and check document ownership server-side rather than trusting the client, and content-creation writes additionally require a verified email, closing off a spam vector unverified accounts would otherwise have.

### Subscription Integrity
Client-reported subscription state is never fully trusted — a scheduled Cloud Function independently re-checks every active subscriber's real status against RevenueCat's own record once a day, correcting Firestore if the two ever disagree (e.g. after a cancellation the Play Store hasn't yet propagated everywhere).

## Screenshots
<img width="319" height="694" alt="image" src="https://github.com/user-attachments/assets/7eb96752-47e6-418e-a140-60eee3ce3525" />
<img width="322" height="688" alt="image" src="https://github.com/user-attachments/assets/c2e0f32e-55c9-4270-af12-4b465a43176f" />
<img width="325" height="694" alt="image" src="https://github.com/user-attachments/assets/7e5cbb8a-0728-49ea-b61a-5f203ab34926" />

## Future Improvements

* **Visual Style Search:** An AI-driven image recognition pipeline to scan an outfit photo and find visually similar pieces of clothing across the internet.
* **Wardrobe Logging:** Letting users log pieces they already own, so the AI Outfit Builder can recommend combinations from what they have instead of only new purchases.
* **Batch Transactions:** Grouping social follow actions into single atomic Firestore write batches to guarantee absolute consistency across profile metrics.
