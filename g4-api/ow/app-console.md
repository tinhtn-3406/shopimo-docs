# Flow

## Console

### Setup user

- (1) Create user root => `ConsoleUser`
- (2) `[POST] /console/api/login`  Login with user/password of root => `ConsoleSession` => `Token`
- (3) `[GET] /console/api/session`: Check session of user => authorization

    ```js
    - MfaStatus_MFA_STATUS_NOT_READY MfaStatus = 0
    - MfaStatus_MFA_STATUS_READY     MfaStatus = 1
    ```

    - Should set `MfaStatus_MFA_STATUS_READY`
    - Use func `SetMfaType` to set `MfaStatus_MFA_STATUS_READY`
  
### Content

- Using to delivery campaign to user `DeliveryContent` => Every user will one delivery campaign

  - **stamp**
  - **coupon**
  - **notification**
  - **popup**
  - **pushNotification**
  
> Only **pushNotification**  `push notification` fcm , others only save info `dynamo`

### Delivery - Run every 30s

> `Delivery` --> create `DeliveryContent` set  `IsPublic` true ->false

#### Flow register delivery campaign

(1) [POST] `UpdateDeliveryConsole` /content/delivery: Create a new delivery

```js
persistence.Delivery{
  RetailCode:      a.conf.RetailCode,
  DeliveryId:      req.Delivery.DeliveryId,
  ContentId:       req.Delivery.ContentId,
  DeliveryType:    req.Delivery.DeliveryType,
  TargetType:      req.Delivery.TargetType,
  ContentType:     req.Delivery.ContentType,
  Title:           req.Delivery.Title,
  ImageUrl:        req.Delivery.ImageUrl,
  Duration:        req.Delivery.Duration,
  Priority:        uint8(req.Delivery.Priority),
  From:            &from,
  To:              &to,
  Data:            b,
  Status:          0,
  IsAfterDelivery: isAdterDelivery,
  CreatedBy:       userId,
  UpdatedBy:       userId,
 }
 ```

(2) [POST] `StartDelivery`  /delivery/start: Update `status = 1` of the delivery

(X) [POST] `StopDelivery`  /delivery/stop: Update `status = 19` of the delivery

#### Flow delivery content using job

> Status is `1` start delivery
> Status is `19` stop delivery
> Status is `20` end delivery

- `1 -> 2 -> 3`: **prepareDeliveryList**
  - Status is `1` -> Set is `3` after end  process
  - Create data ( not insert database) `DeliveryMessage` of every member => PutObject to s3 with key `delivery/{{deliveryId}}.json`
    - With `ContentType == "pushNotification"` always created and `MessageType = fcm`
    - With `ContentType == dynamo"` 
      - Crate `MessageType = dynamo`
      - If `AfterActionPushNotification.IsEnable` is `true` created and `MessageType = fcm`
<br/>
- `3 -> 4 -> 5`:  **publishContent**
  - Status is `3` -> Set is `5` after end  process
  - With `ContentType != "pushNotification"` to set field `Scope` of table `DeliveryContent`

    ```go
    case "allMember":
      z.Scope = "L"
    case "allMember2":
      z.Scope = "M"
    case "memberExtraction":
      z.Scope = "L"
    case "import":
      z.Scope = "L"
    case "manual":
      z.Scope = "L"
    case "allDevice":
      z.Scope = "D"
    case "event":
      z.Scope = "L"
    case "broadcast":
      z.Scope = "D"
    case "favoriteStore":
      z.Scope = "F"
    default:
      z.Scope = "L"
    }
    ```

<br/>

- `5 -> 6 -> 7`: **deliveryMessage**
  - Status is `5` -> Set is `7` after end  process
  - Create `PushNotificationControl` => **Run job push notification in path** `server/cmd/push_notification`
  
    - With `ContentType == "pushNotification"` always
    - With `ContentType == dynamo"` only create when `AfterActionPushNotification.IsEnable = true`

<br/>

- `7 or 17 -> 19`: **End the delivery when the deadline expires and stop**

<br/>

- `19 -> 20`: **stopDelivery**
  - `DeliveryContent` set `is_public = false`
  - `PushNotificationControl` set `status = 4`

### AuthToken

- **Generating auth token by console app**

---

#### Flow

##### AppHomeSetting - model `AppHomeSetting`

- [POST] `SetAppHomeSettingPublicState` /appHomeSetting/public:
  - Set field `IsPublic` is true
- [POST] `SetAppHomeSettingDefaultState` /appHomeSetting/default:
  - Set field `IsPublic` and `is_default` is true
- [POST] `UpdateAppHomeSetting` /appHomeSetting
  - Add/Update `appHomeSetting` and save in field `data` in json
- [GET] `GetAppHomeSetting` /appHomeSetting: Get app home setting
- [GET] `GetAppHomeSettingList` /appHomeSetting/list: Get list of app home settings

---

##### Coupon  - model `Content`

> Note: Have using `delivery` with `contentType = "coupon"`

- [POST] `UpdateCoupon` /content/coupon:
  - Add/Update coupon in table `Content` with ContentType is `coupon`
- [DELETE] `DeleteCoupon` /content/coupon
- [GET] `GetCouponListConsole` /content/coupon/list
- [GET] `GetCouponConsole` /content/coupon

---

#### consoleNotification - model `ConsoleNotification`

> Note: Have using `delivery` with `contentType = "notification"`

- [POST] `UpdateConsoleNotification` /consoleNotification:
- [DELETE] `DeleteConsoleNotification` /consoleNotification
- [GET] `GetConsoleNotificationList` /consoleNotification/list
- [GET] `GetConsoleNotification` /consoleNotification

---

#### stampCard - model `Content`

> Note: Have using `delivery` with `contentType = "stamp"`

- [POST] `UpdateStampCard` /content/stampCard
  - Add/Update stamp card in table `Content` with ContentType is `stamp`
- [GET] `GetStampCardList` /content/stampCard/list
- [GET] `GetStampCard` /content/stampCard

```json
{
  "content_id": "be07a0d6cd55b778",
  "title": "10月度スタンプラリー開催中！",
  "stamp_card_image_url": "https://d21deq89do6ri1.cloudfront.net/images/6f5167334a0fff740aaf42e4e5174c9cf43a5109.png",
  "stamp_list": [
    {
      "index": 1,
      "stamp_image_url": "https://d21deq89do6ri1.cloudfront.net/images/1a760828bbb17a2ec15f1bcde4e2c3751eed18d6.png",
      "point": 1
    },
    ...
    {
      "index": 10,
      "stamp_image_url": "https://d21deq89do6ri1.cloudfront.net/images/acbe17709213cbf40d3d7ca7eb8eb4bed56c0dcf.png",
      "point": 3
    }
  ],
  "image_url": "https://d21deq89do6ri1.cloudfront.net/images/6f5167334a0fff740aaf42e4e5174c9cf43a5109.png"
}

```

---

#### pushNotification - model `Content`

> Note: Have using `delivery` with `contentType = "pushNotification"`

- [POST] `UpdatePushNotification` /pushNotification:
  - Add/Update push notification in table `Content` with ContentType is `pushNotification`
- [DELETE] `DeletePushNotification` /pushNotification
- [GET] `GetPushNotificationList` /pushNotification/list
- [GET] `GetPushNotification` /pushNotification
  
```json
{
  "content_id": "5abcd950397ee4cb",
  "title": "本日最終日！",
  "is_default_icon": true,
  "body": {
    "index": 1,
    "body_type": "body",
    "body": "画面ご提示でブランドランファンレジにて20％OFF"
  },
  "link": {
    "type": "screen",
    "url": "/coupon/424c4e33c16e9c7b"
  }
}
```

---

#### popup - model `Content`

> Note: Have using `delivery` with `contentType = "popup"`

- [POST] `UpdatePopupConsole` /popup:
  - Add/Update push notification in table `Content` with ContentType is `popup`
- [GET] `GetPopupConsole` /popup
  
```json
{
  "content_id": "bd340183ce5f996f",
  "title": "河内長野店よりご案内♪",
  "image_url": "https://d21deq89do6ri1.cloudfront.net/images/b08ce325885c8cff78f3379bd234bb3e076f0e0a.jpg",
  "link": {
    "type": "internal",
    "url": "https://www.okuwa.net/netsuper/store/ns_kawachinagano_sp.html",
    "title": "詳しくはこちら♪",
    "style": "yellow"
  }
}
```

---

##### Condition - Model `Condition`

> Setting condition term

- [POST] `UpdateCondition` /condition:
- [GET] `GetCondition` /condition

##### Force Update - model `ForcedUpdate`
