
## Modules
### Mobile App
The mobile part is built with Flutter.

Integrated SDKs:
- [Firebase Auth](https://firebase.google.com/docs/auth)
Push
- [OneSignal](https://onesignal.com/)
- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)
- VoIP pushes for iOS
Analytics:
- [AppHud](https://apphud.com/)
- [Firebase Analytics](https://firebase.google.com/docs/analytics)
- [Amplitude](https://amplitude.com/)
- [AppsFlyer](https://www.appsflyer.com/)
Video:
- [Agora](https://www.agora.io/)
- [Twilio](https://www.twilio.com/)

The mobile app has feature flags to disable parts of the functionality across different platforms on request.

Also, the app has built-in AI features, like nudity detection on video or subtitle creation.
### Web version
The web version of the app is built with [SvelteKit](https://svelte.dev/docs/kit/introduction) and Tailwind. It uses the same auth provider, the same [GraphQL WebSocket API](https://hasura.io/docs/2.0/subscriptions/overview/), and is fully interoperable with the mobile app. The web version provides a similar experience with calls, chats, notifications, etc.
![olo-web-gallery.png](./olo-web-gallery.png)

![olo-web-chat.png](./olo-web-chat.png)
### Backend
As a core and API gateway, we use [Hasura](https://hasura.io/) as a solution to make WebSocket subscriptions and provide a [GraphQL](https://graphql.org/) API to the client. We use PostgreSQL as the database.

For methods that require logic exceeding simple data fetching, we use [Hasura Actions](https://hasura.io/docs/latest/actions/overview/) implemented in [Node.js](https://nodejs.org/en) with [Fastify](https://fastify.dev/).

The API is built to serve both mobile and web clients. Some methods (like attribution) have different parameters depending on the platform, but the contracts for all functional methods are shared between platforms.
### Streamer portal
A web panel, built with [SvelteKit](https://kit.svelte.dev/), which is used to show information that is not related to usual user activity, like streamer balance, rules for streamers, etc.
### Admin Panel
A web panel where moderators can manage user balances, streamers, and users. Also, KYC and photo moderation are done here.
![[OLO-panel-userlist.png]]
![[OLO-panel-moderation.png]]
![[OLO-panel-user-profile.png]]
### External Services
#### Firebase
We are pretty strongly dependent on Firebase Services.
- Firebase Auth: We use it to provide multi-platform authorization with Google, Facebook, Apple ID, and phone number. Also, it provides us with JWTs that are used to control access permissions over all subsystems. It couldn't be easily substituted because it provides us with UIDs, etc.
- Firebase Storage: Used to store all static assets and to offload network-consuming requests from the API server. The nice part is that it supports permission rules based on JWTs from the Auth provider. For example, I can see only my own photos, etc. It could be substituted for something else, but the permission schema might be less strict with other providers.
- Firebase Hosting: Used for storing the admin panel, promo website, etc. Could be easily substituted.
#### CloudFlare
- CDN: We use the CloudFlare CDN to hide our origin server IP addresses and to set up additional WAF protection.
- Pages: We use CloudFlare pages to host the client's web app, and we use it to work with the BFF layer of our app.

## Video Providers
- Agora – currently, we mostly use them because their video quality surpasses other competitors.
- Twilio – we implemented it as a fallback provider in cases when Agora is unavailable.
The video part is modular and could easily be extended with another provider.

## Email provider
- SendGrid — we use it to manage subscription statuses and send marketing/event-based messages.
## Analytics
We have external analytics, like — [AppHud](https://apphud.com/), [Firebase Analytics](https://firebase.google.com/docs/analytics), [Amplitude](https://amplitude.com/),[AppsFlyer](https://www.appsflyer.com/).

But we also maintain internal reports based on server-side data in [MetaBase](https://www.metabase.com/).
## Functional domains
- balance
- ads
- billing
- blocklist
- chat
- event_log
- favorites
- gallery
- gift
- interaction
- mission_control
- notification
- panel
- payment
- profile
- settings
- timetable
- user
- video

### balance
Here we store everything related to balance. We use event sourcing for all changes related to balances. Also, for the mobile app, we provide a stream of balance-related events. This way, we can show the actual balance to users.
### ads
This domain is used to store attribution and postback data for ad integrations. Currently, we have AppsFlyer data and Snapchat postbacks integrated.

### billing
Here we store tariffs and billing rules for video calls. Billing is fully dynamic, and we can easily change it on the fly.
Example: For calls from a User to a Streamer in Roulette, there is a separate tariff. For user connections, it costs 9 coins, at 20 seconds another 60 coins are billed, at 1 minute another 60 coins are billed, and so on. For the streamer, there are different checkpoints and coin amounts. At 20 seconds, the streamer will get 30 coins, at 60 seconds the next 30 coins will be deposited to the streamer's account, and so on.
![[OLO-billing-schema.png]]
The existing mechanism is super flexible in providing custom scenarios. The main goal from the start was to allow building different motivation schemas for different kinds of users that participate in a call.

### blocklist
From time to time, users want to blacklist other users. There could be different reasons. If a user blocks somebody, we immediately remove the blocked user from calls, the gallery, etc. We also store the reason for additional analysis. For example, we can issue a penalty to a streamer for Nudity, or, if there are a lot of harassment blocks, we can block a user.
![[OLO-user-blocklist.png]]

### chat
This domain provides a messaging solution for users. In chat, gifts, direct, and roulette calls are also shown. There are also additional mechanics to auto-add users into a chat. For example, during onboarding, a user automatically starts a chat with some of the streamers. Roulette calls also create records in a chat. 
### event_log
This is our application ESB, used for storing events. Other domains can subscribe to those events. For example, the AD domain subscribed to the 'user_sign_up' event and sends data to Snapchat about the new registration.
![[OLO-events-list.png]]
### favorites
This domain provides 'favorites' and 'followers' functionality to users.
### gallery
For each user, we build a personal feed of partners. For different roles, these feeds are built with different rules. For example, male users see streamers that are online, and the ones that are online longer are shown first. But streamers see the feed in the opposite direction, and so on. It's the most loaded part of the app, so we do a lot of tricks to make this part of our app robust.

### gift
Users can send gifts to each other. Gifts cost money, and streamers also get benefits from gifts.

### interaction
This is a special domain to track interactions between users.

### mission_control
This is a domain used to support the admin panel. Here we have methods for unrestricted querying of data, managing user roles, payout requests, and agencies. 

### notification
We have in-app notifications and push notifications.
![[OLO-notifications.png]]
Some kinds of notifications are delivered only in-app, but mostly we use a fallback strategy. We send an in-app notification, if the user is not online, or if we don't get an ACK from the phone — then we send a push message. Also, for call notifications on Apple, we use special VoIP notifications. For some messages, we also send email notifications, like a list of missed calls.

### panel
A domain used for working with the segmentation of users into cohorts, groups, etc. Cohorts are used for labeling users with tags, that are used for improved feed generation in the gallery and other places.

### payment
Here we manage prices, products, and payment intents. We are now integrated with Apple payments and Google payments. Also, we have a subscription integration (but it was not approved by Apple and not launched in prod).

### profile
A domain responsible for user data, and also for the separation of private profile data and the public profile.

### settings
Global app settings, like the minimum allowed version for mobile apps. 
![[OLO-global-settings.png]]

### timetable
This is not a domain, but a plugin to store and launch cron tasks in our application.
### user
Here we manage user sessions, user online/offline status, registration tokens, login attempts, etc.

### video
A domain responsible for video calls, prerecorded videos, and roulette. This is a pretty complex domain, where we send call requests, manage call timeouts, and disconnect users.
