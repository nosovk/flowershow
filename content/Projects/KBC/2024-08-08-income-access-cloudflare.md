# Problem:
We have wrong geo reports and wrong ip stats in IncomeAccess

### Reason
We use CloudFlare to manage SSL on top of IncomeAccess.

### History of Question
May–September of 2020 we have a list of emails and calls with [Dimitar Gavrilov](mailto:Dimitar.Gavrilov@paysafe.com) and [Aleksandra Santova](mailto:Aleksandra.Santova@paysafe.com) regarding SSL and CloudFlare issues.

We were a bit stuck on fixing and issue. Even more, we were not able to get some response on timeline about fixes.
![[ia-verry-unsatisfied-with-responce.png]]
After a few meetings, we finished with that conclusion:

> According to the collected info problem, that was caused by Cloudflare, has roots in ignoring `x-forwarded-for` header on your server side. Redirection links are working, but you log CF ip addresses, which are proxy. That's why geo ip filter was broken,
> 
> You may pass article [https://support.cloudflare.com/hc/en-us/articles/200170786-Restoring-original-visitor-IPs-Logging-visitor-IP-addresses-with-mod-cloudflare-](https://developers.cloudflare.com/support/troubleshooting/restoring-visitor-ips/restoring-original-visitor-ips/) to your dev team.
>
> It's crucial, because `x-forwarded-for` is a standard header. If I proxy access to IA using traefik or similar proxy for adding SSL on top of http provided by you we will have a similar issue. Problem is that any proxy will add the original ip in `x-forwarded-for` header. Because it's a standart, it's not invented by Cloudlflare. [https://developer.mozilla.org/en-US/docs/Web/HTTP/headers/X-Forwarded-For](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For)
>
> In common, it means that any solution that I can provide to handle SSL will break GEO ip restrictions in IA. You need to fix detecting original user IP before we can add any SSL proxy on top of yours server.

Actually after that we got response that IA will someday implement that, but no ETA was given, and we have continued our live without GEO filtering/split/report.