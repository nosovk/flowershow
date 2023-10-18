
## Modules
### Mobile
Mobile part: build with flutter.
Integrated SDKs:

- Firebase Auth
Push
- OneSignal
- Firebase Cloud Messaging
- VoIP pushes for IOS
Analytics:
- Apphud
- Firebase Analytics
- Amplitude
- AppsFlyeer
Video:
- Agora
- Twilio

### Backend
As a core and API gateway we use [hasura](https://hasura.io/) as a solution to make WebSocket subcriptions and provide [GraphQL](https://graphql.org/) API to Client.
For methods that require logic that exceeds fetching data we use [Hasura Actions](https://hasura.io/docs/latest/actions/overview/) implemented in NodeJS with [Fastify](https://fastify.dev/).

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
Here we store everything related to balance 