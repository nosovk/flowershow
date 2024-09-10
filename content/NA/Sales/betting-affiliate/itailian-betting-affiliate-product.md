Goal: describe what we want to get.
Reference: https://bromber.it/schedina-social/99a8c5ddc76c36b224ee8fa7a048ecf7?mod=generate

There should be an affiliate site, where users can compare which betting provider givers better lines for each match.

To get thing working we have to somehow obtain data from different betting providers.
[example feed](./feed.xml). We got example feed, which is part of BetRadar export, but in that feed there were no odds from different providers (only one provider).

We need to clarify:
- how we can get access to feed?
- how often data updated in a feed?
- how frequently we can query feed update?
- how big that feed? (in Mbs)

If there is some XSDs or other description of data in a feed it would be great to obtain it.
This is crucial to plan which instruments and how we use them depending on input data that we have.

## Functional blocks
### Main page and widget
Actually on that screen we are showing upcoming events, also we show current line on events.
### Filter/search
filter builder, for example I want to see matches that will take place this weekend, where coefficient is bigger than 1,5. Also search by League or Team Name.
### Comparison page
This is page attached to event, where we can see table with comparison of different betting providers and affiliate links to them.
### Registration/Login
Add users option to register/login (also to recover password, send confirmation email)
### Subscribe on event
Add option to subscribe on high coefficients (to clarify)

## Additional requirements
- Site should support i18n, like it and en languages.
- site should be loading fast
- there should be an admin panel to control which betting providers we show
## Next steps 
### discovery phase
- get info about feed
- draw mocks
- draw design
### development
- coding
- launch stage
- find bugs
- launch prod
