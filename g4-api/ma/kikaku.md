# Kikaku (Product launch plan)

## Database

- `ma_kikakus`: Info about kikaku

    | Status | Description |
    |--------|-------------|
    | 0      | Private     |
    | 1      | Public      |
    | 2      | Completed   |

  - `ma_kikaku_categories` -> `ma_kikaku_sub_categories` -> `ma_kikaku_items`

- `ma_order`: Save order of kikakus by `kikaku_id`, `kikaku_category_id`
  
    | BackendStatus | Description           |
    |---------------|-----------------------|
    | 0             | Reservation completed |
    | 1             | Not Payment completed |
    | 2             | Payment completed     |
    | 3             | Receipt completed     |
    | 4             | Receipt date exceeded |
    | 9             | Cancelled             |

    | Status | Description           |
    |--------|-----------------------|
    | 0      | Reservation completed |
    | 1      | Payment completed     |
    | 2      | Receipt completed     |
    | 5      | Cancelled             |
    | 6      | Cancelled             |

-`ma_kikaku_stocks`:  Number quantity of `ma_kikaku_items` in evert store and all store


---

## API

- `[POST] /api/cart/item` : **AddCartItem** -> add item in `CartItem` temporary before save in `Order`
- `[POST] /api/cart/order` : **AddOrder** -> Add order from `CartItem`
- `[GET] /api/order/store/list` : **GetOrderListWithStore**
- `[GET] /api/order/list` : **GetOrderList**
- `[POST] /api/qr/set`:  **SetQr** -> Check payment have sucess
- `[GET] /api/qr/scan`:  ****
- `[GET] /api/kikaku/list` : **GetKikakuList**  -> Get list kikaku

  ```json
  {
    "kikakuList": [
      {
        "kikakuId": "46751852",
        "title": "s",
        "from": "2024-10-13",
        "to": "2024-10-31",
        "receivingFrom": "2024-10-24",
        "receivingTo": "2024-10-26",
        "bannerImageUrl": "https://d30mih2ad582x7.cloudfront.net/images/7d65804ec681f2d47af3ae65e88eca79fe8f546a.jpg",
        "headerImageUrl": "https://d30mih2ad582x7.cloudfront.net/images/7d65804ec681f2d47af3ae65e88eca79fe8f546a.jpg",
        "body": "",
        "status": 0,
        "paymentDeadline": "2024-10-19",
        "reservationFrom": "2024-10-14",
        "reservationTo": "2024-10-17",
        "tagList": [
          "締め切り間近",
          "おすすめ"
        ],
        "remark": "",
        "storeCode": "",
        "timeRangeList": [],
        "dateRangeList": [],
        "bannerText": "",
        "orderTotal": 0,
        "scope": 3,
        "commitOrderTotal": 0,
        "cancelOrderTotal": 0,
        "noPayOrderTotal": 0,
        "receivedOrderTotal": 0
      },
    ]
  }

  ```

- `[GET] /api/kikaku/store?kikakuId=4a8f85cd&storeCode=undefined`: Get detail kikaku

  ```json
  {
    "kikaku": {
      "kikakuId": "4a8f85cd",
      "title": "テスト（9.10～）",
      "from": "2024-09-09",
      "to": "2024-09-21",
      "receivingFrom": "2024-09-19",
      "receivingTo": "2024-09-21",
      "bannerImageUrl": "https://d30mih2ad582x7.cloudfront.net/images/c799b12e2ee80e399abe6fc35e0b5512fc37d953.jpg",
      "headerImageUrl": "https://d30mih2ad582x7.cloudfront.net/images/c799b12e2ee80e399abe6fc35e0b5512fc37d953.jpg",
      "body": "",
      "status": 1,
      "paymentDeadline": "2024-09-16",
      "reservationFrom": "2024-09-10",
      "reservationTo": "2024-09-13",
      "tagList": [],
      "remark": "",
      "storeCode": "undefined",
      "timeRangeList": [],
      "dateRangeList": [
        "2024-09-19,0",
        "2024-09-20,0",
        "2024-09-21,0"
      ],
      "bannerText": "",
      "orderTotal": 0,
      "scope": 2,
      "commitOrderTotal": 0,
      "cancelOrderTotal": 0,
      "noPayOrderTotal": 0,
      "receivedOrderTotal": 0
    },
    "kikakuCategoryList": [
      {
        "kikakuId": "4a8f85cd",
        "kikakuCategoryId": "d897a6f6",
        "title": "テスト",
        "remark": ""
      }
    ],
    "kikakuSubCategoryList": [
      {
        "kikakuId": "4a8f85cd",
        "kikakuCategoryId": "d897a6f6",
        "kikakuSubCategoryId": "3ce9e652",
        "title": "テスト",
        "body": "テストです。",
        "shortCode": "",
        "imageUrl": "",
        "from": "",
        "to": "",
        "receivingFrom": "",
        "receivingTo": "",
        "reservationFrom": "",
        "reservationTo": "",
        "paymentDeadline": "",
        "subTitle": "",
        "tagList": [],
        "remark": "",
        "isAvailable": true,
        "isUseSubCategoryCondition": true,
        "subImageUrlList": [
          "",
          "",
          "",
          "",
          ""
        ],
        "subBody": ""
      }
    ],
    "kikakuItemList": [
      {
        "kikakuId": "4a8f85cd",
        "kikakuCategoryId": "d897a6f6",
        "kikakuSubCategoryId": "3ce9e652",
        "kikakuItemId": "2eaaeeb7",
        "itemCode": "4908011614246",
        "title": "雪印メグミルク　ナチュレ恵　ブルーベリー＋いちご　７０ｇ×４",
        "body": "",
        "price": 138,
        "priceWithTax": 149.04,
        "taxRate": 8,
        "subTitle": "",
        "paymentDeadline": "2024-09-16",
        "stock": 0,
        "order": 0,
        "listPrice": 0,
        "employeePrice": 0,
        "discountTagList": [],
        "shortCode": "14246",
        "imageUrl": "https://d30mih2ad582x7.cloudfront.net/images/6d425a1a6ed0fba3335cfc85d172bb9359c7ac73.png",
        "tagList": [
          "予約期間外:2024-09-10-2024-09-13"
        ],
        "category1Code": "05",
        "category1Name": "日配",
        "category2Code": "550",
        "category2Name": "乳飲料",
        "category3Code": "5521",
        "category3Name": "ヨーグルト",
        "category4Code": "",
        "category4Name": "",
        "itemName": "雪印メグミルク　ナチュレ恵　ブルーベリー＋いちご　７０ｇ×４",
        "costPrice": 117,
        "lot": 6,
        "deliveryType": "1",
        "tempType": "2",
        "supplierCode": "0023",
        "logisticsType": "05",
        "centerCode": "801",
        "officeCode": "00",
        "maxOrderPerOrder": 90,
        "maxOrderPerUser": 90,
        "discountFlag": "3",
        "barcode": "4908011614246",
        "taxType": "1",
        "maxReservationQuantity": 0,
        "plannedQuantity": 0,
        "orderHistoryList": [
          {
            "orderId": "317202409122146",
            "orderdAt": "2024-09-12 09:33"
          },
          {
            "orderId": "317202409122476",
            "orderdAt": "2024-09-12 09:37"
          },
          {
            "orderId": "317202409126350",
            "orderdAt": "2024-09-12 09:23"
          }
        ],
        "orderItemCode": "4908011614246",
        "discountPrice": 0,
        "discountRate": 0,
        "changedPrice": 0,
        "changedPriceWithTax": 0,
        "newPrice": 0,
        "commitOrder": 0,
        "commitPrice": 138,
        "commitPriceWithTax": 149.04
      },
      {
        "kikakuId": "4a8f85cd",
        "kikakuCategoryId": "d897a6f6",
        "kikakuSubCategoryId": "3ce9e652",
        "kikakuItemId": "af977c7d",
        "itemCode": "4903015500113",
        "title": "ヤマザキビスケット　チップスターＳ　うすしお味　４５ｇ",
        "body": "",
        "price": 95,
        "priceWithTax": 102.6,
        "taxRate": 8,
        "subTitle": "",
        "paymentDeadline": "2024-09-16",
        "stock": 0,
        "order": 0,
        "listPrice": 0,
        "employeePrice": 0,
        "discountTagList": [],
        "shortCode": "00113",
        "imageUrl": "https://d30mih2ad582x7.cloudfront.net/images/88ba6872780bb62f1ecd8226a6ca184e0a8af5ad.jpg",
        "tagList": [
          "予約期間外:2024-09-10-2024-09-13"
        ],
        "category1Code": "07",
        "category1Name": "菓子",
        "category2Code": "700",
        "category2Name": "菓子",
        "category3Code": "7152",
        "category3Name": "カップ・箱スナック",
        "category4Code": "",
        "category4Name": "",
        "itemName": "ヤマザキビスケット　チップスターＳ　うすしお味　４５ｇ",
        "costPrice": 88,
        "lot": 8,
        "deliveryType": "1",
        "tempType": "0",
        "supplierCode": "0017",
        "logisticsType": "03",
        "centerCode": "802",
        "officeCode": "00",
        "maxOrderPerOrder": 90,
        "maxOrderPerUser": 90,
        "discountFlag": "3",
        "barcode": "4903015500113",
        "taxType": "1",
        "maxReservationQuantity": 0,
        "plannedQuantity": 0,
        "orderHistoryList": [
          {
            "orderId": "317202409138873",
            "orderdAt": "2024-09-13 15:20"
          },
          {
            "orderId": "518202409123067",
            "orderdAt": "2024-09-12 23:54"
          },
          {
            "orderId": "317202409121424",
            "orderdAt": "2024-09-12 09:37"
          },
          {
            "orderId": "317202409129022",
            "orderdAt": "2024-09-12 09:23"
          }
        ],
        "orderItemCode": "4903015500113",
        "discountPrice": 0,
        "discountRate": 0,
        "changedPrice": 0,
        "changedPriceWithTax": 0,
        "newPrice": 0,
        "commitOrder": 0,
        "commitPrice": 95,
        "commitPriceWithTax": 102.6
      },
      {
        "kikakuId": "4a8f85cd",
        "kikakuCategoryId": "d897a6f6",
        "kikakuSubCategoryId": "3ce9e652",
        "kikakuItemId": "85a431f7",
        "itemCode": "4901408532253",
        "title": "ＹＫＢＣ　チョココロネ　１個",
        "body": "",
        "price": 98,
        "priceWithTax": 105.84,
        "taxRate": 8,
        "subTitle": "",
        "paymentDeadline": "2024-09-16",
        "stock": 0,
        "order": 0,
        "listPrice": 0,
        "employeePrice": 0,
        "discountTagList": [],
        "shortCode": "2253",
        "imageUrl": "https://d30mih2ad582x7.cloudfront.net/images/cef553d4214a1a574a236e2e03c006b814a65776.jpg",
        "tagList": [
          "予約期間外:2024-09-10-2024-09-13"
        ],
        "category1Code": "58",
        "category1Name": "パン",
        "category2Code": "580",
        "category2Name": "パン",
        "category3Code": "5812",
        "category3Name": "菓子パン",
        "category4Code": "",
        "category4Name": "",
        "itemName": "ＹＫＢＣ　チョココロネ　１個",
        "costPrice": 72,
        "lot": 1,
        "deliveryType": "1",
        "tempType": "0",
        "supplierCode": "0028",
        "logisticsType": "04",
        "centerCode": "802",
        "officeCode": "01",
        "maxOrderPerOrder": 90,
        "maxOrderPerUser": 90,
        "discountFlag": "3",
        "barcode": "4901408532253",
        "taxType": "1",
        "maxReservationQuantity": 0,
        "plannedQuantity": 0,
        "orderHistoryList": [
          {
            "orderId": "317202409133168",
            "orderdAt": "2024-09-13 15:20"
          },
          {
            "orderId": "518202409125846",
            "orderdAt": "2024-09-12 23:54"
          },
          {
            "orderId": "317202409129905",
            "orderdAt": "2024-09-12 09:37"
          },
          {
            "orderId": "317202409125384",
            "orderdAt": "2024-09-12 09:23"
          }
        ],
        "orderItemCode": "4901408532253",
        "discountPrice": 0,
        "discountRate": 0,
        "changedPrice": 0,
        "changedPriceWithTax": 0,
        "newPrice": 0,
        "commitOrder": 0,
        "commitPrice": 98,
        "commitPriceWithTax": 105.84
      }
    ]
  }
  ```

---

## Flow

### Create Order

- Model: `ma_persistence.MaCartItem{}` => `ma_persistence.MaOder{}`
- API : `AddCartItem` -> `PrecheckOrder` -> `AddOrder`
