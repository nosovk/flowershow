## **[ROP-32](https://nodeart.app/jira/browse/ROP-32): Add new transaction types**

We have a request to separate different types of bonuses in the admin panel. To do this, we want to add two additional types of transactions besides Bonus:

- No Deposit Bonus / Comp
- Cashback bonus / Discount

Reasoning: customer want's to account different types of bonuses separately.

## **[ROP-33](https://nodeart.app/jira/browse/ROP-33): update common stats report**
Modify common stats report

Filter section:
- date from {2025-05-20} / date to {2025-05-25}
- userid {eRdw2dttfH3f}

Example columns:

| uid          | bets | wins | ggr  | deposit | withdrawal | Bonuses | No Deposit Bonus / Comp | Cashback bonus / Discount |
| ------------ | ---- | ---- | ---- | ------- | ---------- | ------- | ----------------------- | ------------------------- |
| eRdw2dttfH3f | 1233 | 100  | 1133 | 200     | 100        | 10      | 20                      | 30                        |


The report should be available in both UID display mode and general mode.

## **[ROP-34](https://nodeart.app/jira/browse/ROP-34) : Add time filter**

The accounting period in the casino runs from 6 am to 6 am. Therefore, it is necessary to add the ability to select time in addition to the day in the reports. So that users can filter 1 day from hour x to hour x.

## **[ROP-43](https://nodeart.app/jira/browse/ROP-43): forbid transactions, that will cause negative balance from admin panel
Now I can manually withdraw any amount from user balance. It can cause negative balance in a player account. Negative balance used in a few reports now, they also should be updated.

We should forbid actions, that cause negative balance. Even for admins.

## **[ROP-44](https://nodeart.app/jira/browse/ROP-44): add top-up api
We got a payment provider api from client: [https://documenter.getpostman.com/view/17969062/2sA2xiVWvG](https://documenter.getpostman.com/view/17969062/2sA2xiVWvG)
Now we can implement online top-up for player balance. It seems that there is a 2 step flow inside
api endpoint /check gives us a list of banks
api endpoint /start accepts bank id and amount, producing redirect link to us.

## **[ROP-47](https://nodeart.app/jira/browse/ROP-47): remove landing page, directly go to login page
We have a landing at [https://app.slotpara.com/en](https://app.slotpara.com/en)  
We got a request to delete it, and directly redirect user to login page: [https://app.slotpara.com/en/login](https://app.slotpara.com/en/login)  
For authorized user we want to directly procceed to lobby:  [https://app.slotpara.com/en/play](https://app.slotpara.com/en/play)

## **[ROP-48](https://nodeart.app/jira/browse/ROP-48): Hide transaction history view for the player**

It is necessary to comment out or remove the button for showing transactions for the user. Also remove the routes so that the user cannot directly access these routes.

## **[ROP-49](https://nodeart.app/jira/browse/ROP-49): Make the top logo bigger**
I've attached two screenshots, as is, and as needed. We need the larger one.
![[ROP-49-as-is.png]]
![[ROP-49-as-needed.png]]
## **[ROP-51](https://nodeart.app/jira/browse/ROP-51): Make the header two-tier on a narrow screen**

For the mobile version, the logout button currently disappears from the screen. To be able to log out - I propose to split the header into two tiers on the mobile version. Logo and balance first. Menu buttons second.
![[ROP-51.png]]

## **[ROP-52](https://nodeart.app/jira/browse/ROP-52): Add DAU metric**
Add Active Users metrics to report. It should account: how many users have an actual money transaction in selected period. (like bet, win, deposit or withdrawal)


## **[ROP-53](https://nodeart.app/jira/browse/ROP-53): Subtract money from bonuses**

Previously, we could enter negative transactions in the Bonus, No Deposit Bonus, Cashback Bonus transaction types. This was used to deduct the total bonuses issued to the user. But administrators say that it's better to avoid — on a keyboard (reasoning unknown, probably no minus (-) letter at Turkish layout?) . 
Solution: Disable option to input negative numbers into inputs. Then add new transaction options to dropdown: Bonus (minus), No Deposit Bonus (minus), Cashback Bonus (minus). Those new options would be recorded in the balance as negative transactions within the corresponding types, but for the cashier, this will still be the input of a positive number. 

Also, we have to add separated columns to reports, to account plus and minus transactions separately.

Important: we want to keep existing transaction types, because altering them will significantly increase complexity of reports.

Important: the final balance cannot become negative in the end.

## **[ROP-54](https://nodeart.app/jira/browse/ROP-54): Add confirmation when changing balance

When admins have to create a transaction manually, they complain that there is no visual identification of the success of the operation, as a result, they press confirm several times, and it turns out that they create several transactions. Also, they require some confirmation step to double-check expected result.

Expectation: when you click create a transaction, show a popup with a confirmation request. While there is no confirmation of success from backend — disable the button `add a new transaction`.

## **[ROP-55](https://nodeart.app/jira/browse/ROP-55): Add support role**

Example user:

username: ozun

The user has selected roles: Audit, Cashier, KYC, Reset Client password.

I expect, that this user will have access to the `client/transactions (History)` and `client/transactions audit` tabs.

I observe: the presence of the Transactions Audit tab, but not the History tab.

I propose to allocate access to the transaction history in a separate role - support, with which the person would see both tabs - Transactions Audit and regular Transaction (although the audit may not be needed for them).

## **[ROP-57](https://nodeart.app/jira/browse/ROP-57): Show balance in the user list**

In the user list, it is not entirely clear whether the person has a balance or not. I would like to immediately show the column with the person's balance so that it is clear whether should I interact with player, or not.

- The column should not be reactive, refreshing the page is ok.
- Clicking on the balance (or the text `adjust`) should still open the balance change modal.

## **[ROP-59](https://nodeart.app/jira/browse/ROP-59): Add functionality for linking to junket**

Scenario: there is a casino partner, who is roughly speaking an affiliate (manager, called a junket). And this junket receives a percentage of the player's GGR per month. We do not want to create logins/passwords for junkets in the backend-system.

Therefore, the following scheme is proposed: We create a separate column inside each use — `junket`. In it, you can specify another system user with the junket role (similar to the `issuer` field in transactions). Changing this field should be logged, just like changing any field in the profile (logging occurs in BQ, the current scheme allows us to add a new attribute without changing the data schema in BQ). The field can take an empty value (meaning that the user has no junket associated with him). 

How should the Junket role be assigned? Ideally, create a separate tab, just like for admins (the users tab in admin panel, make the same for junkets). So that way we can separately create users with the Junket role in it. For access to the Junket section in admin panel, a separate role - `junket manager` is needed. For access to editing the Junket field inside the Client — the `kyc role` is sufficient. 

Inside Junket, there should be a Junket Report tab, which is a User Report, but with an additional filter by the Junket field in client profile (this way they will see the amounts of deposits, withdrawals, bonuses, etc. only for their client). We can add filter after adding the junket field in the player's profile. 

Junket cannot log in to the admin panel or as a client. But at the same time, a password can be set for the junket. The junket password is used to obtain statistics for themselves. A link like: rop-admin.com/reports/junket/[username]?password=[password] should open a report similar to the report in Junket Report. 

Important: in the end, we will need to manage junkets, but for now, just accounting for players in the context of junkets should be enough.

## **[ROP-60](https://nodeart.app/jira/browse/ROP-60): Add dropdown list for selecting junket**

When choosing a junket, there is a problem with finding the name. I would like to add a dropdown list with the names of junkets. It can be loaded only when user interacts within the input, so as not to make unnecessary requests to get the entire list every time.

## ** [ROP-61](https://nodeart.app/jira/browse/ROP-61): Report for junket on the client**

We want the junket to be able to log into their junket profile, like a casino client. To do this, we want to add the ability for users with the Junket role to log into the client part. For users with the Junket role, we want to add the ability to view a special report. In the report, there will be information on all players attached to this junket. I propose to simply display the button to show the report in the profile. Also, keep in mind that there may be more reports for junkets in the future. Important - the ROP client should make a request to the rop.src admin panel to get the report URL, no metabase data should be contained in rop.src.

## **[ROP-62](https://nodeart.app/jira/browse/ROP-62): Make username search case-insensitive**
Currently, the username search is case-sensitive, which is very inconvenient, so it is better to make the search case-insensitive. Ideally, also change umlauts to regular names. 
Examples of names:
- Yozeltin
- Kagun
- Bugdaycis
- Gunesm

It is not possible to find users by these usernames in the search (but they are there, just with lowercase letters).

## **[ROP-63](https://nodeart.app/jira/browse/ROP-63): Add functionality for balance change requests**

Scenario: there are several operators who accept requests, they enter balance replenishment requests, but in the end, they are approved by the cashier. 

We want to add a special role - `cashier assistant` and add a special table — cashier requests. A user with the `cashier assistant` role can create cash movements, but instead of applying them to the transaction table, they are placed in the balance change requests table. 

The request contains information about who created it, the amount, and the type of transaction (bonus, refund, or deposit). 

These requests do not affect the balance and do not affect transactions. When a user with the `cashier` role logs into the system, they can go to the table with the list of requests. By selecting a request, the user with the `cashier` role can execute it. Executing a request means that a transaction will be created. At the same time, it is possible to record in the request which transaction was created based on it, and change the status of the request to complete. There should be no option to delete a completed request.

## ** [ROP-64](https://nodeart.app/jira/browse/ROP-64): Add junket report inside the junket profile**

We want to add the junket report functionality directly within the junket profile in client part. We don't want to allow to non casino employees to operate within admin panel. Because of that we will extend client part.

## **[ROP-66](https://nodeart.app/jira/browse/ROP-66): additional info upon transaction in confirm dialog**:
We have added a modal screen at [ROP-54], but we should extend it with transaction name (not only amount like now).

## **[ROP-67](https://nodeart.app/jira/browse/ROP-67): add column that shows direction of transaction
It seems that admin panel users have problems with Difference in transaction history. I suggest to add additional column with +-, or just add +- to balance cell.

Important: we still put only positive values into database, it's just a visualization, no changes to data model should be applied.
![[ROP-67.png]]

## **[ROP-69](https://nodeart.app/jira/browse/ROP-69): disable non Latin letters in username
Time to time we got usernames with äöåç letters inside. Client asked to remove option to input them. Also, this will fix issues with search - because a and ä are different letters, its often causes problems when they use search. 

## [ROP-70](https://nodeart.app/jira/browse/ROP-70): add phone number to client profile
Client asked to add phone number field to profile. 

I suggest making a separate storage for county code and phone number itself. It's much easier to operate with county codes, if we will be requested to make any further actions with those data. We can use https://www.npmjs.com/package/svelte-tel-input to receive countryCallingCode, nationalNumber and store them as client attributes. This field should have uniq constraint, adding two or more customers with one phone number should not be allowed.

## **[ROP-27](https://nodeart.app/jira/browse/ROP-72): add Junket column
We got a request to modify client list page, by adding Junket name, as a separate column.
![[ROP-27.png]]
Important: with a high probability it will cause performance impact (loading time will much higher), because of data model that we have. Consider adding some cache to avoid high loading time.

## **[ROP-73](https://nodeart.app/jira/browse/ROP-73): add Junket-admin and Junket-manager roles**
Add new role `Junket-Admin`, that is required to get into Junket section of admin panel.

Add `Junket-Manager` role, that will have access to managing which Junket is attached to exact client.

For `KYC` role we should FORBID option to changed Junket attribut for clients.

## **[ROP-75](https://nodeart.app/jira/browse/ROP-75): Change Notifications place to bottom-right corner
For now notification toast appears in standard, top right position. Client says that it closes functional buttons.

Please move it to bottom right corner instead.
![[ROP-75.png]]
## **[ROP-76](https://nodeart.app/jira/browse/ROP-76): Change Action Names, Change Action Order (like Deposit, Bonus, Cashback etc)
Client complains on UX of actions dropdown menu.
They need to make lots of actions fast, and would like to sort actions by priority:
1. Deposit  
2. Bonus: Bonus In  
3. Cashback Bonus: Cashback Bonus In (discount)  
4. No Deposit Bonus: No Deposit Bonus In (compliment)  
5. Withdraw  
6. Bonus: Bonus Out  
7. Cashback Bonus (minus): Cashback Bonus Out (discount)  
8. No Deposit Bonus (minus): No Deposit Bonus Out (compliment)  
9. Refund

## **[ROP-77](https://nodeart.app/jira/browse/ROP-77): Clients filtration and order **
Add additional sorting options to clients list: 
- sort by Last Login Date
- sort by other fields if no performance impact


## **[ROP-79](https://nodeart.app/jira/browse/ROP-79): add family and name columns to list of players
In a clients list now we depend on username, we were asked to add family name and name columns to list layout