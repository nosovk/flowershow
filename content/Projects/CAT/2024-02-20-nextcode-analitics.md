# CAT: NextCode anaytics feature request

## Feature Name: Export of signup/login attempts to Datawarehouse.

## Goal / Business Objective 

- 1. Ensuring Data Quality Control During User Registration 
- 1.1. Identifying and analyzing field validation errors during user registration on product domains. 
- 1.2. Identifying and analyzing field validation errors during user registration on pre-landing and landing pages. 
- 1.3. Optimizing the user registration process by addressing detected issues. 

- 2. Analyzing Registration Funnel Efficiency 
- 2.1. Collecting and monitoring data on successful registrations to build conversion statistics. 
- 2.2. Identifying weak points in the registration process and improving user experience. 

## Design 

Not needed, backend feature
## Requirement / Feature Description 

We expect to have a table, where we have list of all signup\login\validation\deposit requests to our API (both success and error ones).

For example, table like that: 

| Field             | Type      | Required | Description                                                        |
| ----------------- | --------- | -------- | ------------------------------------------------------------------ |
| `timestamp`       | TIMESTAMP | &check;  | Time when event occurred                                           |
| `user_agent`      | STRING    | &cross;  | Browser UserAgent header from request                              |
| `ip`              | STRING    | &cross;  | IP from request                                                    |
| `geo`             | STRING    | &cross;  | geo from Cloudflare headers (MaxMind DB)                           |
| `as_country_code` | STRING    | &cross;  | country code from IP to ASN api  (optional)                        |
| `as_description`  | STRING    | &cross;  | description of AS from IP to ASN api (optional)                    |
| `as_number`       | STRING    | &cross;  | AS number of IP from IP to ASN api (optional)                      |
| `domain`          | STRING    | &cross;  | Which domain accepted request (required for mirrors)               |
| `path`            | STRING    | &cross;  | URL path (optional, for GraphQL not very useful)                   |
| `query`           | STRING    | &cross;  | URL query params (optional, for GraphQL)                           |
| `action`          | STRING    | &cross;  | GraphQL action (login, signup, restore password, validate)         |
| `fp`              | STRING    | &cross;  | user fingerprint, if it was present                                |
| `referrer`        | STRING    | &cross;  | http referrer header                                               |
| `email`           | STRING    | &cross;  | email used for registration                                        |
| `country`         | STRING    | &cross;  | country used for registration                                      |
| `currency`        | STRING    | &cross;  | currency used for registration                                     |
| `phone`           | STRING    | &cross;  | phone used for registration                                        |
| `status`          | STRING    | &cross;  | status of registration (test/multi acc/success/error)              |
| `aff_data`        | STRING    | &cross;  | "affiliate_data"  value (aff id, promo id, click id)               |
| `additional_data` | STRING    | &cross;  | additional fata from request body (which have no separate columns) |
| `resp`            | STRING    | &cross;  | api response as text                                               |
| `duration`        | INTEGER   | &check;  | how many nanoseconds we were waiting for response                  |

> Expected Functionality: (How should it work from a business / technical perspective?) 

We should be able to analyze how many users are blocked\registered and with which kind of statuses. Like, we would like to know how many users tried to register with emails that are already in use.

Key Scenarios / User Flows: (Step-by-step description of how the feature will be used) 

We use Metabase to store reports. We build alerts in Metabase, which are triggered for example in case when there are more than 100 errors on a registration endpoint in a hour.

Furthermore, we use report to clarify which errors are most usual for our players, like gmul.com instead of gmail.com.

We use report to measure response time, like users from Japan have in common 20% slower response time, compared to EU users.

> In addition to the standard task description, please answer the following questions (not required) to help us take into account the task's details. If you have answers to them, please provide : 
> 
> Which Backoffice settings will be modified or added? 

Probably none

> How does this impact the Portal? 

> Are there dependencies on other systems or modules? 

We depend on Google Cloud Big Query product. Also be aware, that in case of BQ outage we should buffer data (we shouldn't lose any records)

We depend on MetaBase (could be shifted to another instance if needed)

> Are there any required changes in access control (authorization, roles, permissions)? 

No, it's purely backend functionality.

> Are there any specific business rules or constraints? 

We should not lose any records, even if we have temporary unavailability of BQ

We should not batch sending of data (use streaming insert). We ask for as small as possible delay in data ingress.

> Which test scenarios are critical to verify? 

We need to distinguish different error types. It would be nice if we will have error codes, not only text descriptions of errors. Texts are always localized, and time to time it's hard to find out the meaning of error in Turkish language.

> Are there specific performance requirements? 

Our api should not affect performance of signup.

> Are there any regional/legal restrictions? 

No

> What risks or potential issues exist? 

If some data is missing, its very hard to verify that on error events, that does not have any outcome in database (like no user was created, no side effects to measure)

## Is there integration with other services? 

Yes, we need to integrate with Google Big Query as with data warehouse solution (using service account). Better to use existing google cloud project (`singular-server-415210`).

# Additional notes:

There are two functions are currently missing on a new platform:
- there is not intermediate verification requests (like to verify email without submiting the whole form). Backend filed validation should be also logged, if it will be introduced in future.
- there are no clear identification of errors, during the tests we got "Unknown error", instead of Rate limit error (and actual limit seems to be a bit too big, like more then 30 registartions were done with one Fingerprint, before hitting errors)

## Deposit logging

Also we would like to have the same functionality regarding deposit attemts. The shape of the data should be discussed.