https://karabas.com/ - @NosovK 

https://www.ticketmaster.com/ - @NosovK 

https://concert.ua/ - @innce 

https://kontramarka.ua/en - @innce

## Karabas.com
```"UA-19898265-1", {allowLinker: true, cookieDomain: "auto", name: "gtm5"})```

2083744259.1752952378

```ga("create", "UA-19898265-22", {allowLinker: true, cookieDomain: "auto", name: "gtm16"})```

```ga("gtm16.linker:autoLink", ["frontmanager.com.ua"], false, false)```

client id: 2083744259.1752952378

```gtm.init_consent```

```function: "__ogt_cross_domain", priority: 27, vtp_rules: ["list", "frontmanager\.com\.ua", "karabas\.com", "kerabas\.pp\.ua"], tag_id: 15}```

```__ogt_referral_exclusion", priority: 17, vtp_includeConditions: ["list", "karabas\.com", "paymaster\.ua", "liqpay\.com", "wayforpay\.com"], tag_id: 17}```

tracked ids:
G-FVEWSBCBRZ
G-72XCDE4CDX
AW-764160861
AW-17104773925
AW-16892781720

```
gtag('set', 'linker', {
  'accept_incoming': true
});
```



### inbiza
```javascript
    window.session_ids = JSON.parse(CookieHelper.get('_ga_session_ids') || '{}');

    function updateGa4Session(gaId, gaSessionId) {
        window.session_ids[gaId] = gaSessionId;
        document.cookie = "_ga_session_ids=" + encodeURIComponent(JSON.stringify(window.session_ids)) + "; path=/";
        console.log(gaId, gaSessionId);
    }

    function getGa4SessionData() {
        let ga4SessionsMatches = [];
        try {
            ga4SessionsMatches = [...document.cookie.matchAll(/_ga_([\w\d\-]*?)=GS\d\.\d\.(.+?)(?:;|$)/g)];
        } catch (e) {
            console.log(e);
        }

        if(ga4SessionsMatches?.length <= 0) {
            window.setTimeout(() => getGa4SessionData(), 200);
            console.log('ga4SessionsMatches not found, setTimeout getGa4SessionData');
            return;
        }

        for (const ga4SessionMatch of ga4SessionsMatches) {
            let gaId = 'G-' + ga4SessionMatch?.[1];
            let gaSessionId = ga4SessionMatch?.[2].split(".")?.[0];

            if (gaId && gaSessionId) {
                updateGa4Session(gaId, gaSessionId);
            }
        }
    }

    if (typeof dataLayer === 'object') {
        let sessionIdClosure = function(gaId) {
            return function(gaSessionId) {
                updateGa4Session(gaId, gaSessionId);
            }
        };

        for (dataLayerOption of dataLayer) {
            if (dataLayerOption[0] === 'config') {
                gtag('get', dataLayerOption[1], 'session_id', sessionIdClosure(dataLayerOption[1]));
            }
        }
    } else {
        console.log('dataLayer not found');
    }

    getGa4SessionData();
```
Похоже этот скрипт просто сохраняет в отдельную куку все gaid, которые сможет найти на сайте. То есть по факту не решает нашу проблему

## Novosad
https://www.novosad.company/events/
нет ифрейма, просто переход на сайт концерта целиком

### drama-comedy
https://drama-comedy.kiev.ua/afisha/syn/
нет ифремйма тоже, просто переход на сайт целиком

## Marco
![[./attachments/Pasted image 20250719234551.png]]
	![[./attachments/Pasted image 20250719234430.png]]
	Валюта теряется по пути
причину пока не нашел.

Так же мне кажется что фильтр неверно работает, фикс: https://gitlab.ticketcrm.com/ticketcrm/frontend/crm/-/commit/7a5eda3cb6809a427b33df770bb68a777bed9af6
слил ее в прод