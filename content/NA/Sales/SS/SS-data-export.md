# SS Data export

## Goal
Provide data lake for BI tools with relevant data about players and their actions.

## Background
We use SS functionality for [External Affiliates](https://docs.softswiss.com/display/CASINOPUB/External+Affiliates), [(download)](https://ipfs.io/ipfs/QmYZoFqwsJYdFxD6AUdW2jxDCcVcDJ1YysiyaiJhSRxZvu?filename=External%20Affiliates_a5e06fba514740ce99a053977356f4e5-260123-1738-292.pdf).

## Technical decisions
We fetch data once in an hour, this is because SS provides data in hourly chunks. We use GoLang to avoid high memory consumption - xml input on some projects is more than 4gb xml for one hour data. Due to that initial data fetch on old projects takes two\three calendar months. Making requests faster could break SS backend, that’s why initial load is artificially slowed. Because of big amount of data we use [BQ](https://cloud.google.com/bigquery) as datawarhouse.


## Data structure:
### User Table
```sql
accountId          INTEGER   REQUIRED ; SS userID
transactionDate    TIMESTAMP REQUIRED ; Registration date
email              STRING    NULLABLE		
type               STRING    NULLABLE ; account
fullName           STRING    NULLABLE		
firstName          STRING    NULLABLE		
lastName           STRING    NULLABLE		
gender             STRING    NULLABLE ; m\f
dateOfBirth        STRING    NULLABLE ; 2000-12-22
receivePromos      BOOLEAN   NULLABLE ; true\false
receiveSmsPromos   BOOLEAN   NULLABLE ; true\false
nickName           STRING    NULLABLE		
timeZone           STRING    NULLABLE		
mobilePhone        STRING    NULLABLE		
mobilePhoneStatus  STRING    NULLABLE		
country            STRING    NULLABLE ; UA\DE\US
city               STRING    NULLABLE		
address            STRING    NULLABLE		
postalCode         STRING    NULLABLE 
confirmedAt        STRING    NULLABLE ; timestamp of confirmation action
affiliateEmail     STRING    NULLABLE ; empty
affiliateLinkCode  STRING    NULLABLE ; empty
cTag               STRING    NULLABLE ; used by affilka
bTag               STRING    NULLABLE ; used by incomeaccess
qTag               STRING    NULLABLE		
sTag               STRING    NULLABLE ; custom frontend metadata
bTagNetRefer       STRING    NULLABLE ; used by NetRefer
utmSource          STRING    NULLABLE ; UTMs from registartion
utmMedium          STRING    NULLABLE		
utmCampaign        STRING    NULLABLE		
utmContent         STRING    NULLABLE		
utmTerm            STRING    NULLABLE		
groupId            STRING    NULLABLE ; 44, 51, 32 - updated on load, no use at the moment
userlimit          STRING    NULLABLE		
referralparams     STRING    NULLABLE		
gaId               STRING    NULLABLE ; google analitics id
exportDate         TIMESTAMP REQUIRED ; timestamp of export to BQ (debug info)
```

### Transactions Table
```sql
transactionId      INTEGER   REQUIRED ; SS transaction id
accountId          INTEGER   REQUIRED ; SS account id
transactionDate    TIMESTAMP REQUIRED ; Timestamp of operation
actionName         STRING    REQUIRED ; win\bet\bet_tax\bonus\cancel_bonus\cashout\deposit
amount             INTEGER   NULLABLE ; ammount in coins, 1 usd = 100
coin               STRING    NULLABLE ; EUR/BTC
source             STRING    NULLABLE ; Devcode - Skrill, pragmatic/TheDogHouse
status             STRING    NULLABLE ; Accepted for payments
ctag               STRING    NULLABLE ; unknown
exportDate         TIMESTAMP REQUIRED ; timestamp of export to BQ (debug info)
```
![example](https://i.imgur.com/1gQpBaG.png)


### View: transactions-eur
```sql=
transactions + in_eur colums
```
![example](https://i.imgur.com/yCFNH1A.png)


### exchangeRatesEUR table
```sql=
currency           STRING      REQUIRED ; Currency name	
insertDate         TIMESTAMP   REQUIRED ;	
rate               FLOAT       REQUIRED ; Exchange rate	
```
![example](https://i.imgur.com/DqhWDdG.png)


## Aditional logic
- Added custom function to select only first depostits in a range of time. Used for postbacks.
- Added postbacks for each registration/first deposit. This postback enpoint customizable. Used for custom AD integrations/notifications etc.

## BI

Different clients use a few BI systems to provide analitics dashboards. Known cases - [MicroSoft Power BI](https://powerbi.microsoft.com/), [Google Data Studio](https://datastudio.google.com/), [QLik](https://www.qlik.com/), [Tableau](https://www.tableau.com/).
We recomend opensourced solution - [MetaBase](https://www.metabase.com/).
Beacuse BigQuery is common DataWarehouse solution - any BI platfrom can use it as datasource.