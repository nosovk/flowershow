# Mirror redirection tool
We want to send users to redirection tool, that will choose mirror that is a best for exact user (and certainly not blocked).

## Pre requirements:
### Favicons
There should be favicon or any other small (!!!) image on each mirror, like:
- https://tombriches.com/favicon.png
- https://tombriches1.com/favicon.png
- etc.
### Hotlink protection
In common we are aware that [HotlinkProtection](https://developers.cloudflare.com/waf/tools/scrape-shield/hotlink-protection/) should be always enabled, but only for favicon we ask you to disable it. There are two ways of making that — both described in documentation over the link above. For our case there is no difference, will you place image to `hotlink-ok` folder or will use configuration rule. But be aware, if you miss that point — mirror always will be unavailable for end users.

## Core mechanics
When users open `redirector.com/page/stag=xxx` we serve static HTML page.
All urls (with any path and search params) should be routed to one html file.
In that HTML we show loader as a first step.
Then we have to detect visitor country, (it's easy to [use CloudFlare itself as free geoip provider](https://developers.cloudflare.com/network/ip-geolocation/))
Based on a country we should get list of mirrors supported in that country:
```yaml
country:
  DE:
    - tombriches1.com
    - tombriches2.com
  NL:
    - tombriches3.com
    - tombriches4.com
  Default:
    - tombriches.com
    - tombriches1.com
    - tombriches2.com
    - tombriches3.com
    - tombriches4.com
```
From config like the one above, we may check what exact mirrors should be used by user's geo. If no special config provided: we should fall back to default handler.

When we got list of domains, we start loading of all favicons from provided domains.
If we got HTTP status 200 and correct mime from request — it means that mirror is accessible.
We're redirecting user to favicon that was faster to load (better connection).
When we're making redirect, we should preserve both: path and search parameters.
`redirector.com/page/stag=xxx` => `tombriches1.com/page/stag=xxx`
`redirector.com/page/?stag=xxx` => `tombriches1.com/page/?stag=xxx`
`redirector.com/?stag=xxx` => `tombriches1.com/?stag=xxx`

> We must redirect users further as fast as possible, in a best case users would not find out that they are bin proxied via redirector.

### Exclude from indexing
Please add into head block of served html page:
```html
<meta name="robots" content="noindex">
```
to avoid indexing of redirector in search engine.

### Example:
You may check realization over the link: [link](view-source:https://www.kingtraf.com/)