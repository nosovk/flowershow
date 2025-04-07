![[funders portal manual.pdf]]


# Goal
Clarify what product you want to build.

From the first glance I thought that you want to build merchant platform. The flow that I described during the call:
- user register
- user must pass KYC (Know You Customer)
- user gets access to cabinet and API
- users integrates payment form at his site
- customers pay to service users using payment form
- we charge customer cards and put money to user's account (and charge our commission)
- we make payouts to users bank account upon request.

But according to doc above, you need something bit different. 
The software you provided as an example is a white label solution for [ISO – independent sales organization](https://stripe.com/it/resources/more/independent-sales-organizations)
It means that you target existing ISO, they register at your platform, sign contract with a bank, and operate with bank using yours system. Am I correct?

In common, it means that yours platform is a gate to different banks (with whom ISO must sign contract separately).
But I don't understand which services you want to provide to ISO.
- CRM system for ISO to track their customer's
- payment gateway to process card payments
- payment hardware to process cards offsite?


In case you work as a proxy to bank you don't need to build tokenizer, bank provides you with everything. You still have to acquire PCI DSS, but with a smaller scope.
But you will have to build something to bring additional value to users. I need to clarify what exactly you want to provide on top of card processing, because your fee will be bigger than at a classical merchant provider (but you have no need to buy a licensee)

## What we can build
- we can build CRM
- we can build payment Widget/API for customers ([example](https://g-24.pro/wid/)) 
- we can build integration with bank API to process cards (but we need to get that API first, because we never worked within US bank API). And we will have to certify part of the system with PCI DSS (external audit and certification, at least half year on that process).
- we can build backend/frontend to cover another user stories, but at first step we have to clarify and describe them.