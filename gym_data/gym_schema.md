# Gym Retail/Fitness — Data Schema (v2025.11.10)

本資料集由三張核心表構成：**customers、orders、order_details**。主要用於會員/交易/品類分析、RFM 與行銷分群。

---

## 1) customers 〈會員主檔〉
- **檔名**：`gym_customer.csv`  
- **列數**：58,115
- **主鍵建議**：`customer_no`

| 欄位 | 型別 | 說明 |
|---|---|---|
| `customer_no` | string | 會員唯一代號。與交易、訂單明細關聯鍵。 |
| `age_category` | string | 年齡類別（如 18–24、25–34、35–44…）。可用於分群、客群交叉。 |
| `gender` | string | 性別（M/F/Other/Unknown）。 |
| `hsien_desc` | string | 縣市／地區描述（地理區域）。可做區域業績與門市佈局分析。 |
| `bs_tag_desc` | string | 商務/族群標籤（如「健身新手」「重訓愛好者」「瑜伽族」等）。*實際值依資料而定*。 |

---

## 2) orders 〈訂單主檔〉
- **檔名**：`gym_order.csv`  
- **列數**：73,795
- **主鍵**：`order_num`（每張訂單一列）  
- **關聯**：`customer_no` → customers.`customer_no`; `order_num` → order_details.`order_num`

| 欄位 | 型別 | 說明 |
|---|---|---|
| `order_num` | int | 訂單編號（主鍵）。 |
| `customer_no` | string | 會員代號（外鍵）。 |
| `order_date` | string(YYYY-MM-DD) | 訂單日期。 |
| `order_time` | string(HH:MM:SS) | 訂單時間。 |
| `store_name` | string | 門市／通路名稱。可做門市績效、區域熱點分析。 |
| `order_amt` | int | 訂單金額（主檔金額）。 |

---

## 3) order_details 〈訂單明細〉
- **檔名**：`gym_orderdetail.csv`  
- **列數**：266,379
- **複合鍵建議**：`(order_num, detail_no)`（一張訂單多列）

| 欄位 | 型別 | 說明 |
|---|---|---|
| `order_num` | int | 訂單編號（外鍵）。 |
| `detail_no` | int | 訂單明細序號（同一訂單內的列號）。 |
| `prod_category` | string | 商品大類（如補劑、器材、服飾、課程等）。 |
| `quantity` | float | 數量。 |
| `unit_price` | float | 單價。 |
| `amount` | int | 該明細小計金額（quantity × unit_price）。**Monetary 建議以明細加總**。 |

---

## 關聯關係（ER 概念）
- **customers (1) — (N) orders**：一位會員可有多張訂單。  
- **orders (1) — (N) order_details**：一張訂單對多筆商品／服務明細。

---