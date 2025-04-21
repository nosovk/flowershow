Goal: describe project milestones and artifacts, describe proposed tech stack.

## Milestones
### 1. Discovery & Planning Stage
Goal of this stage:
- Clarify business objectives and success metrics (Why are we building this? How will we measure success?).
- Define target user personas and monetization strategy (Who are the users? How will the site generate revenue?).
- Define who are our end users (customer portrait)
- Develop detailed user stories covering core features (e.g., registration, bet comparison, notifications, user profiles).
- Create a sitemap and define page layouts/routes. Like: site.com/(lang)/team/(FC-gogo)/stats attached to (layout-team-stats)
- Develop low-fidelity wireframes (using Balsamiq or similar) for all key user flows and page layouts

**Artifacts**
- Project brief/vision document.
- User portrait document.
- Prioritized user story backlog.
- Sitemap and route definitions.
- Low-fidelity wireframes/mockups.
- Initial technical feasibility report.

**Pricing**
Initial offering for first Milestone: [[2025-04-08-bet-compare]]
### 2. Design & Prototyping Stage
Goal of this stage:
- Create a complete visual identity
- Create user interface design system
- Create interactive prototype based on the defined user stories and wireframes

**Artifacts**
- Develop brand guidelines (logo, color palette, typography), in most cases all design work is done in Figma.
- Create a comprehensive UI design system/kit (components, patterns, styles).
- Design high-fidelity mockups for all pages and states based on wireframes.
- Build a clickable prototype simulating key user flows.

> We will make scope prioritization and evaluation after that step.
### 3. MVP Development & Launch Stage
Goal of this stage:
- Build, test, and deploy a functional Minimum Viable Product (MVP) that includes the core user stories identified in the planning stage.

#### Checkpoints
- Set up development, staging, and production environments.
- Establish CI/CD pipelines.
- Develop frontend and backend components based on designs and user stories.
- Implement core features (e.g., user registration, basic bet comparison, data display).
- Integrate with necessary APIs (e.g., betting odds providers via Data Gateway Service).
- Set up database and data persistence.
- Implement logging, monitoring, and alerting (observability).
- Implement backup and recovery procedures.
- Conduct thorough Quality Assurance (QA) testing.
- Deploy the MVP to production.

**Artifacts**
- Configured CI/CD pipelines.
- Source code repository (e.g., Git).
- Deployed MVP application on production environment.
- Established observability dashboards and alerts.
- Backup/restore procedures documentation.
- QA test plans and results.
- Technical documentation (architecture, setup guide).
### 4. Iterative Development & Enhancement Stage
Goal of this stage:
Enhance the product by implementing remaining user stories, incorporating user feedback, fixing bugs, and improving performance based on analytics and monitoring.
**Checkpoint**
- - Gather and analyze user feedback and usage analytics (e.g., from MetaBase dashboards).
- Prioritize and refine the backlog with new features, improvements, and bug fixes.
- Develop, test, and release new features/enhancements in iterative cycles (sprints).
- Optimize application performance and scalability based on monitoring.
- Refine designs and user flows based on feedback and testing.
**Artifacts:**
- Updated source code repository.
- Deployed new features and bug fixes to production

### 5. Maintenance & Support Stage
Goal of this stage:
- Ensure the ongoing stability, security, and performance of the live product according to defined Service Level Agreements (SLAs). Transition from active feature development to primarily maintenance.
**Activities:**
- Monitor system health, performance, and security.
- Perform regular maintenance (updates, patches).
- Manage backups and ensure disaster recovery readiness.
- Address critical bugs and security vulnerabilities promptly.
- Provide ongoing technical support based on SLA.
- Periodically review and update dependencies and infrastructure
## Tech
### Technology Stack  
- **[SvelteKit](https://svelte.dev/docs/kit/introduction)** — Frontend framework
- **[Cloudflare Pages](https://pages.cloudflare.com/)** — Hosting and CDN
- **[Cloudflare KV](https://developers.cloudflare.com/kv/)** — Cache layer for BFF
- **[Strapi](https://strapi.io/)** — Admin panel and API for content
- [Meilisearch](https://www.meilisearch.com/) - search engine
- **[Temporal](https://temporal.io/)** — Workflow management
- [Fastify](https://fastify.dev/) —  performant http server for [NodeJS](https://nodejs.org/)
- [Redis](https://redis.io/) — Cache layer for Fastify\Strapi
- [PostgreSQL](https://www.postgresql.org/) - SQL Database
- **[OVH.ie](https://www.ovhcloud.com/en-ie/)** — Server hosting for Strapi/Temporal
- **[BigQuery](https://cloud.google.com/bigquery)** — Long-term storage
- **[MetaBase](https://www.metabase.com/)** — Dashboards


### Project Structure
  
Project split to 3 parts.  
  
- **Front**: this is a part for end-users, it uses some API and have aggressive caching, but mostly it's all about our users and UI.  
- **Admin panel**: here we can work with content, tags etc. We use strapi for it.
- **Data Gateway Service**: this is a separate service that allows us to fetch data and make some preparation, validation steps, then put that data to DB.

#### Front

This part uses fully custom design to provide the best user experience. We would like to use mobile-first approach in development (it should be discussed with designer during working on second milestone). In most cases we have component based design.

To separate external API from app data layer, we use BFF layer (Backend For Frontend). Here shines [SvelteKit](https://kit.svelte.dev/) and [CloudFlare pages](https://pages.cloudflare.com/). The main benefit of such stack is a built-in integration of BFF layer into app, which allows using strongly typed autogenerated interface between BFF and frontend. Because of that, it's easy to wrap any external API in BFF and decouple its shape from UI. 

Also, we want to use [KV](https://developers.cloudflare.com/kv/) to cache API responses on the edge, to reduce the amount of actual requests to backend.

#### Admin panel

We didn't want to build custom admin panel for content management. But we have to provide some interface to manage content. We suggest to stick with [Strapi](strapi.io) within those tasks. 
Strapi is not designed for handling lots of requests, because of that we have caching layer at BFF layer (which is super performant). Strapi supports i18n for content types. We will host Strapi as normal app in docker container, without CF Pages or other edge solutions.

The main goal of using a system like Strapi is to avoid building an admin panel ourselves. In it, we can create data scheme with point and click, and then use it as data source.

For asset storage we integrated [CloudFlare R2](https://developers.cloudflare.com/r2/). It's a flexible and pretty cheap solution to store lots of images. The egress traffic for R2 is free, that's why we should use it instead of GoogleCloud or other S3 like storage.

Also, we use strapi to pre-process images, like generate AVIF versions and provide different sized versions. AVIF outperforms other formats for big images.

For 3rd party data we try to avoid using strapi as data layer, and make request to Data Gateway Service

#### Data Gateway Service
This is an umbrella for group of workflows, managed by Temporal.

Actually fetching, merging, cleaning, extending, saving of any kind of data — is a workflow. Because of temporal we have a good observability of processes that we run, and It's easy to recover/retry/debug if something goes wrong.

#### Infra
As infrastructure layer we want to use Docker+Loki+Grafana as observability tool chain for Admin Panel and Data Gateway service.

#### Org
For development process we stick to Atlassian products. We use Jira, Bitbucket, Confluence, Bamboo for the development process. Also we use MatterMost as internal messenger for real-time communication.
