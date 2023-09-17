We can't add testers to TestFlight or use it for testing.
We use [Firebase app distribution](https://firebase.google.com/docs/app-distribution) to deliver the app to testers. It works for IOS, but phone should be switched into developer mode. Apple does not aware of those installs, we can use it without risk.
We have one shot to upload a working app to App Store and pass review.

Flow:
- we set up GIT
- setup domain and stub site with BFF
- setup App Store account and claim app Id (proxy)
- we set up app rails and commit them to GIT
- setup binary deps that could require special treatment on IOS (OneSignal for example)
- setup CI/CD env for app (generate new serials etc)
- setup build for each branch and publication to App Distribution
- pass app distribution builds to QA
- QA passes bugs to dev, repeat till all works as expected
- build IPA
- submit IPA to AppStore, send to review (proxy)

Danger: parts marked as (proxy) should be done with extra security measures to prevent problems with publication.
