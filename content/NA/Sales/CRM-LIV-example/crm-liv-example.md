## Product: CRM for dating product
https://olopanel.space/
CRM is a part of dating application to manage agencies, streamers, and users. The team should be able to work with profiles, statuses and uploaded content (photos, videos).

## Parts of CRM:
### Agencies block
At first page we have list of agencies with infinite scroll and search filter (search by name, search by id)
![[Agencies-list.png]]
We can click on agency name and get into agency page, where we can edit agency details and observe recent balance updates, can create withdrawal request and other daily opertaions.
![[Agencies-page.png]]
### Streamers block
The streamer interactions are actually the same infinite scroll, but within another set of fields and filter by agency.

From interesting part, we have tables with nested events.
![[User-events.png]]
You see list of events, and open one to see from which elements it consists.
### Users block
Also for users there is photo moderation section available:
![[User-photos.png]]
Where admin can add/remove/approve photos, or set them as cover or profile photo.
### Statistics block
There is also a statistic panel with embedded [metabase](https://www.metabase.com/) dashboard.
![[Statistics.png]]
It's a super handy way to make analytic reports.

### Photo moderation
Infinite scroll feed of user images for manual validations.
![[Photos-Moderation.png]]
### Chat moderation.
Here we can moderate chats (and there is a fast way to block user here)

### Applications for payment
This is part of payment approval workflow (payments could be initiated from streamer/agency profile manually via moderator, or could be automatically created)
![[Applications.png]]


## Tech
For authentication we used [Firebase Auth](https://firebase.google.com/docs/auth).

As web framework, we bet on [SvelteKit](https://kit.svelte.dev/) + plus [urql](https://formidable.com/open-source/urql/).

As design system we choose [IBM carbon](https://carbon-components-svelte.onrender.com/).

As API we used [GraphQL](https://graphql.org/) and [Hasura](https://hasura.io/).

As database we use [PostgreSQL](https://www.postgresql.org/).

As backend we used [NodeJS](https://nodejs.org/) [Fastify](https://fastify.dev/).

As BI System we use [Metabase](https://www.metabase.com/).

All this was done with TypeScript and help from our DevOps to handle automatic migrations, backups etc.

## Timeline
Mentioned scope was done in 2 months within 2 frontend developers, one backend and one DevOps engineer.