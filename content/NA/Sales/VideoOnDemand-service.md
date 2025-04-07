## Glossary
- Video on Demand (VOD) — global terming, that used to refer situations when video streamed upon user request (not like TV or Radio)
- Video Streaming — process of content delivery to device. Most video streaming solutions should be able to:
	- split video to chunks, to allow partial load (for example when I as a user want to skip 10 minutes of a show — they are not loaded)
	- adaptive bitrate, to allow download less to small devices or over slow internet connection. To allow that we usually preprocess video for mostly commonly used dimension (iPhone, tablet, desktop etc.)
	- ability to store processed video at CDN. We want to reduce costs, that we want to save all videos preprocessed to CDN and on an end level operate only with manifests (playlists of chunks), not making separate processing for each customer.
 - [HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) — pretty old, but commonly supported streaming format invented by Apple. It's limited to one codec, h264, and has no DRM support. But it's supported by Apple devices natively :)
 - DASH — new video streaming format, which describes container format, that it agnostic to content. It's mostly backed by Google. It's superior to HLS, but much more complex and harder to implement. In the end — it makes the same, just provides some format with explanation where and when video chunk should be loaded to the player.

https://www.cloudflare.com/learning/video/what-is-mpeg-dash/

- Digital rights management (DRM) — common name for tooling and technologies that control digital media distribution. For example blocking of video in some region or by some provider.
- MPEG-CENC-DRM - DRM format implemented in DASH video streaming. It's required, for example, by SONY.
- Ad bidding auction — process of qualifying which advertisement should be shown. Its goal is to maximize revenue, by opting into the most expensive proposition. For example: we have 30 seconds slot for user that is 30–35 years old, female, loves detectives, watched all James Bond series. We have advertisement to show Rolex to all users from 30 to 40 years old, and it's ok to pay for that AD up to 5 USD. Also, we have advertisement from Aston-Martin to show add for all James-Bond fans, with ad price up to 30usd. When this AD requests comes into bidding platform — both advertisements bid on it till 5usd, but at 5.1 USD, Aston-Martin wins, because maximum bet amount reached by Rolex. In the end, user will see Aston Martin advertisement.
 - [OpenRTB](https://openrtb.github.io/OpenRTB/) — set of specifications, that allow to provide your ads on other RTB platforms. (There are some open source solutions there also, like PreBid)


## Description
We have two main scenarios
### Free users
Users should be able to watch some limited library of videos. They should be interrupted by advertisement time to time. AD should be not skippable. We should implement AD bidding, and we should implement OpenRTB to sell this adds on different marketplaces.

### Paid users
User can buy subscription, to avoid AD placement in their stream

### Buy content
User can rent some content to watch without AD