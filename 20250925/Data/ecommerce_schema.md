
# 🛒 電子商務資料庫結構說明

本資料庫設計模擬一般電商平台的交易與廣告行為結構，包含會員、商品、交易、廣告與互動行為等資訊。

---

## 📄 customers — 客戶資料表

| 欄位名稱       | 型別     | 說明                     |
|----------------|----------|--------------------------|
| `customer_id`  | VARCHAR  | 客戶唯一識別碼（主鍵）   |
| `name`         | VARCHAR  | 客戶姓名                 |
| `gender`       | CHAR(1)  | 性別（M / F）            |
| `birth_date`   | DATE     | 出生日期                 |
| `register_date`| DATE     | 註冊時間                 |
| `region`       | VARCHAR  | 地區（北部／中部／南部） |
| `modify_date`  | DATETIME | 修改時間                 |

---

## 📄 products — 商品資料表

| 欄位名稱       | 型別     | 說明                     |
|----------------|----------|--------------------------|
| `product_id`   | VARCHAR  | 商品唯一識別碼（主鍵）   |
| `product_name` | VARCHAR  | 商品名稱                 |
| `category`     | VARCHAR  | 商品分類（美妝、3C 等）  |
| `brand`        | VARCHAR  | 品牌名稱                 |
| `list_price`   | DECIMAL  | 市價                     |
| `sale_price`   | DECIMAL  | 售價                     |
| `weight_dimensions` | VARCHAR  | 重量/尺寸描述            |
| `employee_id`     | VARCHAR  | 上架人員（員工編號）     |
| `create_date`     | DATETIME | 上架時間                 |
| `modify_date`     | DATETIME | 修改時間                 |

---

## 📄 transactions — 交易資料表

| 欄位名稱         | 型別     | 說明                             |
|------------------|----------|----------------------------------|
| `transaction_id` | VARCHAR  | 交易編號（主鍵）                 |
| `customer_id`    | VARCHAR  | 客戶ID（外鍵）                   |
| `product_id`     | VARCHAR  | 商品ID（外鍵）                   |
| `quantity`       | INT      | 購買數量                         |
| `amount`         | DECIMAL  | 總金額（通常為 `sale_price * quantity`） |
| `transaction_time` | DATETIME | 交易時間                       |
| `payment_method` | VARCHAR  | 支付方式（信用卡、轉帳等）       |

---

## 📄 ads — 廣告主檔資料表

| 欄位名稱        | 型別     | 說明                            |
|-----------------|----------|---------------------------------|
| `ad_id`         | VARCHAR  | 廣告ID（主鍵）                  |
| `ad_text`       | TEXT     | 廣告文案內容（例如：限時優惠！）|
| `start_date`    | DATE     | 活動開始時間                    |
| `end_date`      | DATE     | 活動結束時間                    |
| `target_category` | VARCHAR | 主打商品類別                    |
| `campaign_type` | VARCHAR  | 活動類型（如：曝光型、轉換型） |
| `employee_id`     | VARCHAR  | 上架人員（員工編號）     |
| `create_date`     | DATETIME | 上架時間                 |
| `modify_date`     | DATETIME | 修改時間                 |

---

## 📄 ad_clicks — 客戶點擊廣告紀錄

| 欄位名稱       | 型別     | 說明                              |
|----------------|----------|-----------------------------------|
| `click_id`     | VARCHAR  | 點擊紀錄ID（主鍵）                |
| `customer_id`  | VARCHAR  | 客戶ID（外鍵）                    |
| `ad_id`        | VARCHAR  | 廣告ID（外鍵）                    |
| `click_time`   | DATETIME | 點擊時間                          |
| `device_type`  | VARCHAR  | 裝置類型（mobile、desktop）       |
| `converted`    | BOOLEAN  | 是否轉換為實際交易（True / False）|

---

## 🔗 關聯關係圖（ER 簡圖）

```text
customers --< transactions >-- products
customers --< ad_clicks >-- ads
```


