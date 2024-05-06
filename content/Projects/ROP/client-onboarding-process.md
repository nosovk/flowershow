## Env Setup
- We ask to buy 2 domains. One domain used for client side, second domain used for API and Admin Panel. We insist on splitting admin and client side.
	- You can by domains at [namecheap.com](https://namecheap.com) or any other domain registrar service (but please avoid GoDaddy)
	- For example: my-real-casino.com and my-secret-admin.com
- After providing domain names, we will provide NS servers that you should use for those domains.
	- for example, there will be something like that: 

| na  | value                  |
| --- | ---------------------- |
| NS1 | june.ns.cloudflare.com |
| NS2 | luke.ns.cloudflare.com |

- After you set NS on domains to values that we provide, and domains settled on OUR Cloudflare account, we can start environment creation.
	- for example at NameCheap panel:
![[namecheap-ns-setup.png]]
- Google Cloud Setup: we have to store audit logs of all bets and client profile modification, and we should store that data in yours account. Please register account, add project, and share ownership rope over that project to dev@nodeart.io
	- registration link: https://cloud.google.com/ 
	- Press “Start Free” and follow instructions, if you will stuck on that step, feel free to ask for support.
	- 
## RLS Colors
- You can choose one of existing RLS themes, or you can [build your own (video guide)](https://youtu.be/QCfXKaZYKYc).
![[rls-theme-1.png]]

![[rls-theme-2.png]]
![[rls-theme-3.png]]
![[rls-theme-4.png]]

## Design
- we need product logo in *vector* format.
- if you have some branding docs, show them to us
- or you can just draw some design from scratch, respecting url structure that exists in example.

## Docs
- we ask for text, for T&C page, and Privacy page
	- please provide docs in [markdown](https://commonmark.org/) format (you can use https://hackmd.io/ to edit them online like Google Docs)
### Translations
- If you want to have some language that differs from English, we will ask you to upload translations. We use [weblate](https://weblate.org/) to manage translations