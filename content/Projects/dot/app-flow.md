```mermaid
sequenceDiagram

actor user
participant app
participant bff
participant tracker

    user->>appstore: open an app page
    appstore->> user: install app
    appstore->> appsflyer: send attribution
    note left of appsflyer: organic/ad source
    user ->>+ app: installs app
    app ->>+ appsflyer: ask for atribution data
    appsflyer ->>- app: send atribution data
    app ->>+ bff: send device data and atribution data
    bff ->>+ tracker: send ip data, atribution data etc
    tracker ->>- bff: ansver with decicion and ad url
    bff ->>- app: send information about decicion and action to perform
    app ->>- user: show iframe with AD
```

