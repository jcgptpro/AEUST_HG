# schema.md

## 檔案與角色
- `pet_customer_final_20250929.csv`：**會員主檔**
- `pet_order_final_20250929.csv`：**訂單主檔**
- `pet_order_detail_final_20250929.csv`：**訂單明細**

## 關聯（ER 簡圖）
```
customers (customer_no) 1 ──< orders (order_num) 1 ──< order_details (order_num, detail_no)
```

## customers（會員主檔）
| 欄位 | 型別 | 說明 |
|---|---|---|
| customer_no | STRING | 會員唯一代碼（主鍵） |
| age_category | STRING | 年齡區間（例：26-30、31-35…） |
| gender | STRING | 性別（男／女等） |
| hsien_desc | STRING | 縣市／行政區（例：台北市） |
| bs_tag_desc | STRING | 行為／興趣標籤（摘要字串） |

**主鍵**：`customer_no`

---

## orders（訂單主檔）
| 欄位 | 型別 | 說明 |
|---|---|---|
| order_num | INTEGER | 訂單編號（主鍵） |
| customer_no | STRING | 會員代碼（FK → customers.customer_no） |
| order_date | DATE/STRING | 訂單日期（字串，需轉日期） |
| order_time | TIME/STRING | 訂單時間（字串；與日期合併） |
| store_name | STRING | 通路／門市 |
| order_amt | INTEGER | 整單金額（折扣後） |

**主鍵**：`order_num`  
**外鍵**：`customer_no → customers.customer_no`

> R、F 通常以訂單主檔計；M 建議用「明細加總」較精準（亦可直接用 `order_amt`）。

---

## order_details（訂單明細）
| 欄位 | 型別 | 說明 |
|---|---|---|
| order_num | INTEGER | 訂單編號（FK → orders.order_num） |
| detail_no | INTEGER | 訂單內流水序（與 order_num 組合鍵） |
| description | STRING | 品名／項目（含折扣、其他支付媒體等） |
| quantity | INTEGER | 數量 |
| unit_price | INTEGER | 單價（折扣列可能為 0 或負） |
| amount | INTEGER | 小計金額（quantity×unit_price；折扣為負） |

**複合主鍵**：`(order_num, detail_no)`  
**外鍵**：`order_num → orders.order_num`

---

## 資料品質提示（建議當作練習）
- 明細含折扣／非商品列，`amount` 可能為 0 或負。
- `orders.order_amt` 與明細 `amount` 加總理論上應一致，可做核對。
- `order_date`、`order_time` 需合併為 `order_ts`（時間戳）以便計算 R。

