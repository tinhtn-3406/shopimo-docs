# Kikaku (Product launch plan)

## Database

- `ma_kikakus`: Info about kikaku
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

-`ma_kikaku_stocks`:  Save info number stock of `ma_kikaku_items` in store to check for change order

## Flow

### Create Order

- Model: `ma_persistence.MaCartItem{}` => `ma_persistence.MaOder{}`
- API : `AddCartItem` -> `PrecheckOrder` -> `AddOrder`
