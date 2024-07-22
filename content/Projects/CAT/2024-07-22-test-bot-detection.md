# Goal:
We want to make a test of bad agent behavior detection on a one of current product.

### What is bad agent?
- Users that use some automation tools, like Cypress or Selenium
- Users that abuse our payment systems (starting lots of deposit requests, that are not fullfiled. Each request have to reserve card form pool, which leads to some kind of DDoS for good customers)
### Userbase
We mostly target Russian market, which causes problem with IP reputation based checks. Most of our users use VPN or TOR or some proxy to get to our products, which leads to problems in detection of suspicious traffic. 
## Methodology
We are waiting to get some score or response about validation at casino frontend. Then we attach that response to Google Analytics session and to Microsoft Clarity session.
Then we build a report in BI, that shows as two metrics:
- how much of our paying users were marked as bad agents (and what is their value, we have data about their value and can easily calculate how much of our income will be affected)
- how much of actual bad agents were marked as bad agents.

Based on those two metrics we have to make a decision, like are we going to implement it in our backend or not.

### Test integration schemas

### GTM
We obtain some script from solution provider, install it into Google Tag Manager container. We launch that Tag on page load or registrations attempt (ask solution provider), as a result of launching the tag we expect to get some result, that we can attach to google Analytics.
### GTM Fields that we can send to script
- user email
- user id

### CSV exchange
We can 