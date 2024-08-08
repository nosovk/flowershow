# Problem:
We have wrong geo reports and wrong ip stats in IncomeAccess

### Reason
We use CloudFlare to manage SSL on top of IncomeAccess.

### History of Question
May - September of 2020 we have a list of emails and calls with Dimitar Gavrilov <[Dimitar.Gavrilov@paysafe.com](mailto:Dimitar.Gavrilov@paysafe.com)> and Aleksandra Santova <[Aleksandra.Santova@paysafe.com](mailto:Aleksandra.Santova@paysafe.com)> regarding that issue.
We were a bit stuck on fixing and issue. Even more, we were not able to get some response on timeline about fixes.
![[ia-verry-unsatisfied-with-responce.png]]

We finished with that conclusion:

> According to the collected info problem that was caused by Cloudflare has roots in ignoring `x-forwarded-for` header on your server side. Redirection links are working, but you log CF ip addresses, which are proxy. That's why geo ip filter was broken,
> 
> You may pass article [https://support.cloudflare.com/hc/en-us/articles/200170786-Restoring-original-visitor-IPs-Logging-visitor-IP-addresses-with-mod-cloudflare-](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fsgndr.online%2Fclick%3Fredirect%3Dhttps%253A%252F%252Fsupport.cloudflare.com%252Fhc%252Fen-us%252Farticles%252F200170786-Restoring-original-visitor-IPs-Logging-visitor-IP-addresses-with-mod-cloudflare-%26dID%3D1597188361844%26linkName%3Dhttps%3A%2F%2Fsupport.cloudflare.com%2Fhc%2Fen-us%2Farticles%2F200170786-Restoring-original-visitor-IPs-Logging-visitor-IP-addresses-with-mod-cloudflare-&data=01%7C01%7CAleksandra.Santova%40paysafe.com%7Cd5272126cf324e03f4da08d83e4df717%7C07018c2ae7d84bf19456092ad6aecf0f%7C0&sdata=JK%2F8iTQLCPwrIX0H1neix0SJ1YxfhUYCqm98pzrhxDU%3D&reserved=0) to your dev team.
>
> It's crucial, because `x-forwarded-for` is a standard header. If I proxy access to IA using traefik or similar proxy for adding SSL on top of http provided by you we will have a similar issue. Problem is that any proxy will add the original ip in `x-forwarded-for` header. Because it's a standart, it's not invented by Cloudlflare. [https://developer.mozilla.org/en-US/docs/Web/HTTP/headers/X-Forwarded-For](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fsgndr.online%2Fclick%3Fredirect%3Dhttps%253A%252F%252Fdeveloper.mozilla.org%252Fen-US%252Fdocs%252FWeb%252FHTTP%252Fheaders%252FX-Forwarded-For%26dID%3D1597188361844%26linkName%3Dhttps%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FHTTP%2Fheaders%2FX-Forwarded-For&data=01%7C01%7CAleksandra.Santova%40paysafe.com%7Cd5272126cf324e03f4da08d83e4df717%7C07018c2ae7d84bf19456092ad6aecf0f%7C0&sdata=ApPLAvHki%2B6iSckgeOdh7%2BTX72s9YpwupQF34JpZpLk%3D&reserved=0)
>
> In common it means that any solution that I can provide to handle SSL will break GEO ip restrictions in IA. You need to fix detecting original user IP before we can add any SSL proxy on top of yours server.

Actually after that we got response that IA will someday implement that, but no ETA was given, and we have continued our live without GEO filtering/split/report.