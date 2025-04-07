## Userflow
For end user flow looks like that:
```mermaid
sequenceDiagram
autonumber

actor user
participant tracker
participant landing
participant api
participant rotator
participant casino

    user ->>+ tracker: open affilka link
    tracker ->>- user: send landing link + stag
    user ->>+ landing: load lending passing stag
    landing ->>- user: ask user to visit api for regisration
    user ->>+ api: pass data about user
    api ->>- user: send "registration success" and JWT
    user ->>+ landing: passes JWT from API to langing
    landing ->>- user: redirect user to rotator + jwt link
    user ->>+ rotator: opens rotator passing jwt link to it
    rotator ->>- user: sends casino link with jwt
    user ->>+ casino: finaly opens casino :)
    casino ->>- user: if JWT correct, shows that authorization success
```
Where:
- tracker: is an affilka tracker. You can attach different domains to it, also you have to register all landings as destinations in it.
- landing: this is marketing with marketing form
- api: this is API that landing should use for acquiring information about currencies, promo codes etc. And the main: this API used for user registration. This is crucial – if this API blocked, none of landings will work. Please be aware of that and keep an eye to use some domain that is not blocked for hosting API. If you can't – we can proxy requests to that API using landing itself, but it should be supported by API (to be discussed)
- rotator: some web app, that determines with of casino URL should be used by customer. I actually don't know why you need that, but it was requested to use it.
- casino: actually product


Caveats:
- api used for registration could have another domain rather final casino domain (which is obtained at rotator). Because of that, cookie authorization does not work (because cookies stick to issuing domain – API domain). That's why we have to request some additional JWT functionality. If we can use real casino domain as API domain – we can avoid passing JWT and cut of rotator part.
- we should implement integration with ReCAPTCHA to avoid API DDoS.
- we should implement affilka integration (it's a bit unclear how for now, because `/api/client/partner/track_stag` seems like a method for casino, not landing)

## Landing deploy schema

[![](https://mermaid.ink/img/pako:eNqNk01PwzAMhv-KlfNWloylHwcktAFCAgkJTqwc3CYbEW0ytSkwtv130oaNCtDEJXKcx6-dV8mG5EZIkpBFYd7yZ6wsPMxSndorZWE4PIPt9BqmMxByVZg1GA25KUtlt3CHS1nT-fTSR0Cf2jKf9ZWF0i9SgNLgIBSlC1aoZQGmgvO76y3MTIlKU5wLHwROu1PZH3gdr1-oV-m0aiUkZE3-It0MN6iF0ssWoF8ircYJIv7W6cGsD2dZdhQe9-E8z5_69nSzsfmqG9GPBXQ0Al9Sf3vCOtz3YPsbMzxc-Q-objI6b5fgnzRrafYfOtsPkP3w3Ov0LfAZ79ov-R_mHa0e_1He2kkGpJSVSwr3DDepBkiJfZalTEniQiEX2BQ2JaneORQba-7XOieJrRo5IM1KoJUzhcsKS5IssKhd1r0zkmzIO0noKJhEnEd8HJ1G8YQyPiBrl2ZhEHO3iyLO4lNOw92AfBjjJGgwCuMw5JSHEY8nPIz3XS6EsqY6NJHd9tb_n-4bdY0fO5V2ut0nWVYfrw?type=png)](https://mermaid.live/edit#pako:eNqNk01PwzAMhv-KlfNWloylHwcktAFCAgkJTqwc3CYbEW0ytSkwtv130oaNCtDEJXKcx6-dV8mG5EZIkpBFYd7yZ6wsPMxSndorZWE4PIPt9BqmMxByVZg1GA25KUtlt3CHS1nT-fTSR0Cf2jKf9ZWF0i9SgNLgIBSlC1aoZQGmgvO76y3MTIlKU5wLHwROu1PZH3gdr1-oV-m0aiUkZE3-It0MN6iF0ssWoF8ircYJIv7W6cGsD2dZdhQe9-E8z5_69nSzsfmqG9GPBXQ0Al9Sf3vCOtz3YPsbMzxc-Q-objI6b5fgnzRrafYfOtsPkP3w3Ov0LfAZ79ov-R_mHa0e_1He2kkGpJSVSwr3DDepBkiJfZalTEniQiEX2BQ2JaneORQba-7XOieJrRo5IM1KoJUzhcsKS5IssKhd1r0zkmzIO0noKJhEnEd8HJ1G8YQyPiBrl2ZhEHO3iyLO4lNOw92AfBjjJGgwCuMw5JSHEY8nPIz3XS6EsqY6NJHd9tb_n-4bdY0fO5V2ut0nWVYfrw)