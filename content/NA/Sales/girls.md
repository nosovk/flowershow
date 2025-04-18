[RFP](./girls-RFP.pdf)
Goals: make a draft estimation.
## CMS
As a CMS we propose to use [Strapi](https://strapi.io/), which is a ready to go CMS, where we can create entities and generate admin panel for administrators. Also, Strapi provides an interface to work with media assets (media library)[https://strapi.io/features/media-library], which also has built in support of image conversion to different web formats, like avif and png.

Example of strapi data item: ![data:people](https://i.imgur.com/ENUiFbL.png)
## SEO
For SEO we have to generate content. According to RFP most content is only for authorized users (gallery, profiles). We suggest creating blog, and promote it using SEO.
Required seo features:
- json+ld
- sitemap
- rss feed for news
- meta tilte — description

Wasn't able to understand how hotel related content can help us in acquiring new client. Need some further clarification.

## Communication with clients
To structure communication we propose installing [chatwoot](https://www.chatwoot.com/), which supports both [whatsapp](https://www.chatwoot.com/hc/user-guide/articles/1677832735-how-to-setup-a-whats_app-channel) and [telegram](https://www.chatwoot.com/hc/user-guide/articles/1677838569-how-to-setup-a-telegram-channel). It also supports web version of chat, which supports passing info about the client after authorization. In common, it could be used as CRM for communication with different clients. Also, this can help to scale, by building customer success flow with more operators being involved and better quality control. Besides, it's a self-hosted solution, it means that all data is private and stored on yours server.


## Search
To build robust search we propose to use [Meilisearch](https://www.meilisearch.com/), which has built in [integration](https://www.meilisearch.com/blog/strapi-v4-plugin-meilisearch) with strapi. It allows making typo tolerant, facet search. Like: blond [x], height > 170 cm, age < 30, description contains: RHCP etc. Comparing to typical solutions. Its more typo tolerant.

## Analytics
We suggest logging all user events to [bigquery](https://cloud.google.com/bigquery) for further analysys. Its a long term, uncapped storage, where we can store all events that are produced by our users. And we can build dashboards or reports using BI tools. We recommend to stick to [Metabase](https://www.metabase.com/), because it's easy to use for non-technical staff, and also It's easy to embed reports.

## Admin Panel 
Also it's a good idea to move admin to some hidden technical domain, admin panel should be used for managing user statuses and payments (strapi is about content, but not about accounting).
Admin panel should support 2FA as a basic security mechanism.
## Payments
I think that there will be some problems within Strapi, because according to their due diligence process it could be impossible to sell escort services using their merchant. 

## Questions
- Do we need support of different languages?
- Do we need to support right to left language?

## Estimation
This is a draft, this is not commitment, but some kind of range, that should be clarified after having mocks and designs

| position | 1m: price (hours) | 2m  | 3m  | total h | Rate/hour | total |
| -------- | ----------------- | --- | --- | ------- | --------- | ----- |
| BA       | 80                | 0   | 0   | 80      | 36        | 2880  |
| TeamLead | 40                | 40  | 40  | 120     | 72        | 8640  |
| MarkUp   | 168               | 168 | 168 | 504     | 22        | 11088 |
| PM       | 168               | 168 | 168 | 504     | 34        | 17136 |
| FS       | 168               | 168 | 168 | 504     | 57        | 28728 |
| DevOps   | 80                | 40  | 40  | 160     | 72        | 11520 |
| Tester   |                   |     | 168 | 168     | 17        | 2856  |
| Totals   |                   |     |     |         |           | 82848 |

### Gant
```mermaid
gantt
    title girls gant
    dateFormat MM-DD
    tickInterval 1week
    weekday monday
    section BA
        Gather Requirements :ba1, 2025-01-01, 7d
        Create Mocks        :ba2, after ba1, 5d
    section Design
		Create Figma Designs :d1, after ba2, 14d 
	section InfraStrucutre
		Setup server, domains, etc: i1, after ba2, 7d
		Spinup Chatwoot: i2, after dev4, 4d
		Deploy prod: i3, after qa1, 4d
	section Development
		Create application rails: dev1, after ba2, 7d
		Create Blog: dev2, after dev1, 7d
		Create Gallery: dev3, after dev2, 12d
		Create admin panel: dev4, after dev3, 10d
		Integrate analitics: dev5, after dev4, 4d
		Integrate payments: dev6, after dev5, 4d 
		Provide Docs: dev7, after dev6, 2d
		Fixup Bugs: dev8, after dev6, 25d
	section Markup
		Markup: m1, after dev2, 55d
	section Testing
	    search bugs: qa1, after dev6, 14d
	    Check Prod: qa2, after i3, 5d
		
```
