## Web API

Gorush support the following API.

- **GET** [/api/stat/app](https://push.goldencrystalcrash.com/api/stat/app) show notification success and failure counts.
- **POST** `/api/push` push ios, android or huawei notifications.
### API Example

Simple send notification on Android and iOS devices using Firebase, the `platform` value is `2`:

```json
{
  "notifications": [
    {
      "tokens": ["token_a", "token_b"],
      "platform": 2,
      "message": "This notification will go to iOS and Android platform via Firebase!"
    }
  ]
}
```

Send notification with custom sound on iOS devices, **volume must be in the interval [0, 1]**:

```json
{
  "notifications": [
    {
      "tokens": ["token_a", "token_b"],
      "title": "Hello World iOS!",
      "message": "Hello World iOS!",
      "platform": 2,
      "apns": {
        "payload": {
          "aps": {
            "sound": {
              "name": "default",
              "critical": 1,
              "volume": 0.1
            }
          }
        }
      }
    }
  ]
}
```

Send multiple notifications as below:

```json
{
  "notifications": [
    {
      "tokens": ["token_a", "token_b"],
      "platform": 2,
      "message": "Hello World iOS!"
    },
    {
      "tokens": ["token_a", "token_b"],
      "platform": 2,
      "message": "New World iOS!"
    },
    .....
  ]
}

```


### Request body

The Request body must have a notifications array. The following is a parameter table for each notification.

| name                    | type         | description                                                                                       | required | note                                                          |
| ----------------------- | ------------ | ------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------- |
| notif_id                | string       | A unique string that identifies the notification for async feedback                               | -        |                                                               |
| tokens                  | string array | device tokens                                                                                     | o        |                                                               |
| platform                | int          | platform(iOS = 2)                                                                                 | o        | 1=iOS, 2=Android (Firebase), 3=Huawei (HMS)                   |
| message                 | string       | message for notification                                                                          | -        |                                                               |
| title                   | string       | notification title                                                                                | -        |                                                               |
| priority                | string       | Sets the priority of the message.                                                                 | -        | `normal` or `high`                                            |
| content_available       | bool         | data messages wake the app by default.                                                            | -        |                                                               |
| sound                   | interface{}  | sound type                                                                                        | -        |                                                               |
| data                    | string array | extensible partition                                                                              | -        | only Android and IOS                                          |
| huawei_data             | string       | JSON object as string to extensible partition partition                                           | -        | only Huawei. See the [detail](#huawei-notification)           |
| retry                   | int          | retry send notification if fail response from server. Value must be small than `max_retry` field. | -        |                                                               |
| topic                   | string       | send messages to topics                                                                           |          |                                                               |
| image                   | string       | image url to show in notification                                                                 | -        | only Android and Huawei                                       |
| to                      | string       | The value must be a registration token, notification key, or topic.                               | -        | only Android                                                  |
| collapse_key            | string       | a key for collapsing notifications                                                                | -        | only Android                                                  |
| delay_while_idle        | bool         | a flag for device idling                                                                          | -        | only Android                                                  |
| time_to_live            | uint         | expiration of message kept on FCM storage                                                         | -        | only Android                                                  |
| huawei_ttl              | string       | expiration of message kept on HMS storage                                                         | -        | only Huawei See the [detail](#huawei-notification)            |
| restricted_package_name | string       | the package name of the application                                                               | -        | only Android                                                  |
| dry_run                 | bool         | allows developers to test a request without actually sending a message                            | -        | only Android                                                  |
| notification            | string array | payload of a FCM message                                                                          | -        | only Android. See the [detail](#android-notification-payload) |
| huawei_notification     | string array | payload of a HMS message                                                                          | -        | only Huawei. See the [detail](#huawei-notification)           |
| app_id                  | string       | hms app id                                                                                        | -        | only Huawei. See the [detail](#huawei-notification)           |
| bi_tag                  | string       | Tag of a message in a batch delivery task                                                         | -        | only Huawei. See the [detail](#huawei-notification)           |
| fast_app_target         | int          | State of a mini program when a quick app sends a data message.                                    | -        | only Huawei. See the [detail](#huawei-notification)           |
| expiration              | int          | expiration for notification                                                                       | -        | only iOS                                                      |
| apns_id                 | string       | A canonical UUID that identifies the notification                                                 | -        | only iOS                                                      |
| collapse_id             | string       | An identifier you use to coalesce multiple notifications into a single notification for the user  | -        | only iOS                                                      |
| push_type               | string       | The type of the notification. The value of this header is alert or background.                    | -        | only iOS                                                      |
| badge                   | int          | badge count                                                                                       | -        | only iOS                                                      |
| category                | string       | the UIMutableUserNotificationCategory object                                                      | -        | only iOS                                                      |
| alert                   | string array | payload of a iOS message                                                                          | -        | only iOS. See the [detail](#ios-alert-payload)                |
| mutable_content         | bool         | enable Notification Service app extension.                                                        | -        | only iOS(10.0+).                                              |
| name                    | string       | sets the name value on the aps sound dictionary.                                                  | -        | only iOS                                                      |
| volume                  | float32      | sets the volume value on the aps sound dictionary.                                                | -        | only iOS                                                      |
| interruption_level      | string       | defines the interruption level for the push notification.                                         | -        | only iOS(15.0+)                                               |
| content-state           | string array | dynamic and custom content for live-activity notification.                                        | -        | only iOS(16.1+)                                               |
| timestamp               | int          | the UNIX time when sending the remote notification that updates or ends a Live Activity           | -        | only iOS(16.1+)                                               |
| event                   | string       | describes whether you update or end an ongoing Live Activity                                      | -        | only iOS(16.1+)                                               |
| stale-date              | int          | the date which a Live Activity becomes stale, or out of date                                      | -        | only iOS(16.1+)                                               |
| dismissal-date          | int          | the UNIX time -timestamp- which a Live Activity will end and will be removed                      | -        | only iOS(16.1+)                                               |

### iOS alert payload

| name           | type             | description                                                                                      | required | note |
| -------------- | ---------------- | ------------------------------------------------------------------------------------------------ | -------- | ---- |
| title          | string           | Apple Watch & Safari display this string as part of the notification interface.                  | -        |      |
| body           | string           | The text of the alert message.                                                                   | -        |      |
| subtitle       | string           | Apple Watch & Safari display this string as part of the notification interface.                  | -        |      |
| action         | string           | The label of the action button. This one is required for Safari Push Notifications.              | -        |      |
| action-loc-key | string           | If a string is specified, the system displays an alert that includes the Close and View buttons. | -        |      |
| launch-image   | string           | The filename of an image file in the app bundle, with or without the filename extension.         | -        |      |
| loc-args       | array of strings | Variable string values to appear in place of the format specifiers in loc-key.                   | -        |      |
| loc-key        | string           | A key to an alert-message string in a Localizable.strings file for the current localization.     | -        |      |
| title-loc-args | array of strings | Variable string values to appear in place of the format specifiers in title-loc-key.             | -        |      |
| title-loc-key  | string           | The key to a title string in the Localizable.strings file for the current localization.          | -        |      |

See more detail about [APNs Remote Notification Payload](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html).

### iOS sound payload

| name     | type    | description                                          | required | note |
| -------- | ------- | ---------------------------------------------------- | -------- | ---- |
| name     | string  | sets the name value on the aps sound dictionary.     | -        |      |
| volume   | float32 | sets the volume value on the aps sound dictionary.   | -        |      |
| critical | int     | sets the critical value on the aps sound dictionary. | -        |      |

request format:

```json
{
  "sound": {
    "critical": 1,
    "name": "default",
    "volume": 2.0
  }
}
```

```bash
curl --request POST \
  --url https://push.goldencrystalcrash.com/api/push \
  --header 'content-type: application/json' \
  --data '{
  "notifications": [
    {
      "tokens": [
        "dq6-MICVfkrYg3fgsp_7O5:APA91bE-3aMgO1QOgMJE4kchYZbvbvE3-Or-SyBh3Zu15193sExhq215yXIczQYSdjA6QK90SuNs_VkaKHD-49Qbi5RwpaCUQzIDnbq1Czb6xyqhY--W_pU",
        "fUwoMP5B40CTvR1NIPfK1w:APA91bFcpvPrM5ffx7Mp5AiIPW5PprE2yG8Qfr-QvMB1h2o75lHefqmh9vLFOSUtD9KdbuOAIZmoZLI-1ffuLHy5NHMu_NU43_ET33NIpELxBJPY38dHVnY"
      ],
      "platform": 2,
      "title": "title",
      "message": "Hello World android!",
      "apns": {
        "payload": {
          "aps": {
            "sound": {
              "name": "default",
              "critical": 1,
              "volume": 0.1
            }
          }
        }
      }
    }
  ]
}'

```