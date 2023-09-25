# SS Data stream

## Goal
to provide data lake for BI tools with relevant data about players and their actions.

## Background
We use SS functionality for [Event Streaming](https://docs.softswiss.com/display/SCF/Event+Streaming), [[SS-Event-Streaming-Get_started.pdf]]

## Technical decisions
Soft Swiss provides Kafka Stream with real-time feed of events from the platform. It's a big step comparing to previous ([[SS-data-export]]) implementation, where we have to work with data spited by part with a hour granularity. 

Also now we receive not snapshot of data, but feed of profile changes. This allows to get actual profile data, or even more – we can get feed of group updated. For example,we can track optin/optout to different groups for our users. With this, we can check which users were in a high roller group in exact time.
Because of big amount of data we use [BQ](https://cloud.google.com/bigquery) as Data warehouse.


## Data structure:

![[SS-Event-Streaming-Kafka_topics.pdf]]