
## Modules
### Mobile App
Mobile part: build with flutter.

Integrated SDKs:
- [Firebase Auth](https://firebase.google.com/docs/auth)
Push
- [OneSignal](https://onesignal.com/)
- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)
- VoIP pushes for IOS
Analytics:
- [AppHud](https://apphud.com/)
- [Firebase Analytics](https://firebase.google.com/docs/analytics)
- [Amplitude](https://amplitude.com/)
- [AppsFlyeer](https://www.appsflyer.com/)
Video:
- [Agora](https://www.agora.io/)
- [Twilio](https://www.twilio.com/)

### Web version
Web version of app, build with [SvelteKit](https://svelte.dev/docs/kit/introduction) and Tailwind. It uses the same auth provider, the same [GraphQL WebSocket API](https://hasura.io/docs/2.0/subscriptions/overview/), and fully interoperable with mobile app. Web version provides similar experience with calls, chats, notifications etc. 
### Backend
As a core and API gateway we use [Hasura](https://hasura.io/) as a solution to make WebSocket subscriptions and provide [GraphQL](https://graphql.org/) API to Client. As an DB we use PostgreSQL.

For methods that require logic that exceeds fetching data we use [Hasura Actions](https://hasura.io/docs/latest/actions/overview/) implemented in [NodeJS](https://nodejs.org/en) with [Fastify](https://fastify.dev/).

API build to serve both mobile and web client, some methods (like attribution) have different parameters, depending on a platform, but for all functional methods contracts are shared between platforms.
### Streamer portal
Web panel, which is used to show information that is not related to usual user activity, like streamer balance, rules for streamers etc. It's build within [SvelteKit](https://kit.svelte.dev/).
### Admin Panel
Web panel, where moderator can manage user balances, manage streamers and users. Also, KYC and photo moderation made here.
![[OLO-panel-userlist.png]]
![[OLO-panel-moderation.png]]
![[OLO-panel-user-profile.png]]
### External Services
#### Firebase
We are pretty strongly depend on Firebase Services.
- Firebase Auth: we use it to provide multi-platform authorization, with Google, Facebook, Apple ID, phone number.  Also, it provides us with JWTs that used to control access permissions over all subsystems. It couldn't be easily substituted, because it provides is UIDs etc.
- FireBase Storage: used to store all static assets, to offload network consuming requests from API server. The nice part is that it supports permission rules based on JWT's from Auth provider. For example, I can see only my own photos etc. It could be substituted to something else, but permission schema can be less strict at other providers.
- FireBase hosting: used for storing admin panel, promo website etc., Could be easily substituted.

## Video Providers
- Agora – currently we mostly use them, because their video quality surpasses other competitors
- Twilio – we implemented it as fallback provider, in cases when Agora is unavailable.
Video part is modular, and easily could be extended with other provider.

## Email provider
- SendGrid — we use it to manage subscription statuses, and send marketing\event based messages.
## Analytics
We have external analytics, like — [AppHud](https://apphud.com/), [Firebase Analytics](https://firebase.google.com/docs/analytics), [Amplitude](https://amplitude.com/),[AppsFlyeer](https://www.appsflyer.com/).

But we also maintain internal reports based on server side data in [MetaBase](https://www.metabase.com/).
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
Here we store everything related to balance, actually we use event sourcing for all changes related to balances. Also for mobile app we provide stream of balance related events. This way we can show actual balance to users.
### ads
This domain used to store attribution and postback data for ad integrations. Currently, we have appsflyer data, and snapchat postbacks integrated.

### billing
Here we store tariffs and billing rules for video calls. Billing is fully dynamic, and we easily can change it on a fly.
Example: for calls from User to Streamer in Roulette there are separate tariff, where for user connections costed 9 coins, at 20 second another 60 coins billed, at 1 minute another 60 coins billed and so on. For streamer there are different checkpoints and coin amounts. At 20 second streamer will get 30 coins, at 60 second next 30 coins will be deposited to streamer account and so on.
![[OLO-billing-schema.png]]
Existing mechanism is super flexible in providing custom scenarios. The main goal from the start was to allow building different motivation schemas for different kinds of users that participate in a call.

### blocklist
Time to time users want to blacklist some other users. There could be different reasons. If a user blocks somebody we immediately remove the blocked user from calls, gallery etc. And also we store the reason, for additional analysis. For example, we can issue penalty to streamer for Nudity, or, if there are lots of harassment blocks, we can block a user.
![[OLO-user-blocklist.png]]

### chat
This domain provides messaging solution for users. In chat also shown gifts, direct and roulette calls. There are also additional mechanics to auto add users into chat. For example during onboard user automatically starts chat with some of the streamers. Roulette calls also create records in a chat. 
### event_log
This is our application ESB, used for storing events. Other domains could subscribe for those events. For example AD domain subscribed to 'user_sign_up' event, and sends data to Snapchat about new registration.
![[OLO-events-list.png]]
### favorites
This domain provides favorites and followers functionality to users.
### gallery
For each user we build personal feed of partners. And for different roles those feeds built within different rules. For example male users see streamers that are online and ones that longer online shown first. But streamers see feed in opposite direction and so on. It's a most loaded part of the app, we do lots of tricks to make this part of our app robust.

### gift
Users can send gifts to each other. Gifts cost money, and streamers also got benefits from gifts.

### interaction
This is special domain to track interactions between users.

### mission_control
This is domain used to support admin panel. Here we have methods for unrestricted querying of data, managing user roles, payout requests, agencies. 

### notification
We have in-app notification and push notifications.
![[OLO-notifications.png]]
Some kinds of notifications delivered only like in-app, but mostly we use fallback strategy. We send in-app, if user not online, or we don't get ACK from phone — then we send push message. Also for call notifications at apple we use special voip notifications. For some messages we also send email notifications, like a list of missed calls.

### panel
Domain used for working with segmentation of users to cohorts, groups etc. Cohorts used for labeling users with tags, that used for improved feed generation in gallery and other places.

### payment
Here we manage prices, products, payment intents. Now we integrated with apple payments and google payments. Also, we have subscriptions integration (but it was not approved by apple and not launched in prod).

### profile
Domain responsible for user data, and also for separation of private profile data and public profile.

### settings
Global app settings, like minimum allowed version for mobile apps. 
![[OLO-global-settings.png]]

### timetable
This is not a domain, but plugin to store and launch cron tasks in our application.
### user
Here we manage user sessions, user online/offline status, registration tokens, login attempts etc.

### video
Domain responsible for video calls and prerecorded videos, and roulette. This is pretty complex domain, where we send call requests, manage call timouts, and disconnect users.
