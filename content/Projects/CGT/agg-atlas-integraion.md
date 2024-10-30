Goal: we build new service to serve games, which will handle both game provider api and api to launch 3rd party games (casino api). We want to build new Atlas integration within that serveci.

## Glossary
- game provider — company that provides games. In our case CasinoGate is a game provider for Atlas. 
- game provider api - api that used to launch game from aggregation service.
- aggregation service - in our case its Atlas
- casino - company that uses api to launch games from aggregation service
- casino api - api to launch game from aggregation service

### Flow
Test flow that we want to cover:
- Casino Gate as a casino makes a request to Atlas Aggregation service to launch a game via casino api
- Atlas as aggregation service makes request to Casino Gate game provider api for launching a game.
- We should be able to play the game, and bet-wins log should be equal in Atlas and CasinoGame logs

## Docs
[Atlas](https://game-services-test.k8s-hz.atlas-iac.com/casino-integration-api/index.html)

### stage env
key: xxx

atlassian callback url: https://integration.pl-test.play-platform.com/integration-callback/gs/hive/v2/{provider_id}

casino gate callbackurl: https://dev.casinogate.dev/api/agg/atlas