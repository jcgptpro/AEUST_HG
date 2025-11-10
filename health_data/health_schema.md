# Health Store — Data Schema (v2025.11.10)

本資料集由三張核心表構成：**customers、orders、order_details**。主要用於會員/交易/品類分析、RFM 與行銷分群。

---

## 1) customers 〈會員主檔〉
- **檔名**：`health_customer.csv`  
- **列數**：30,352
- **主鍵建議**：`customer_no`

| 欄位 | 型別 | 說明 |
|---|---|---|
| `customer_no` | string | 會員唯一代號。與交易、訂單明細關聯鍵。 |
| `age_category` | string | 年齡類別（如 18–24、25–34、35–44…）。可用於分群與交叉分析。 |
| `gender` | string | 性別（M/F/Other/Unknown）。 |
| `hsien_desc` | string | 縣市／地區描述（地理區域）。 |
| `bs_tag_desc` | string | 業務/族群標籤（實際值依資料而定）。 |

---

## 2) orders 〈訂單主檔〉
- **檔名**：`health_order.csv`  
- **列數**：74,263
- **主鍵**：`order_num`（每張訂單一列）  
- **關聯**：`customer_no` → customers.`customer_no`; `order_num` → order_details.`order_num`

| 欄位 | 型別 | 說明 |
|---|---|---|
| `order_num` | int | 訂單編號（主鍵）。 |
| `customer_no` | string | 會員代號（外鍵）。 |
| `order_date` | string(YYYY-MM-DD) | 訂單日期。 |
| `order_time` | string(HH:MM:SS) | 訂單時間。 |
| `store_name` | string | 門市／通路名稱。可做門市績效與區域分析。 |
| `order_amt` | int | 訂單金額（主檔金額）。建議以明細加總檢核。 |

---

## 3) order_details 〈訂單明細〉
- **檔名**：`health_orderdetail.csv`  
- **列數**：273,314
- **複合鍵建議**：`(order_num, detail_no)`（一張訂單多列）

| 欄位 | 型別 | 說明 |
|---|---|---|
| `order_num` | int | 訂單編號（外鍵）。 |
| `detail_no` | int | 訂單明細序號（同一訂單內的列號）。 |
| `description` | string | 商品名稱／明細說明。 |
| `quantity` | int | 數量。 |
| `unit_price` | float | 單價。 |
| `amount` | int | 該明細小計金額（quantity × unit_price）。 |

---

## 關聯關係（ER 概念）
- **customers (1) — (N) orders**：一位會員可有多張訂單。  
- **orders (1) — (N) order_details**：一張訂單對多筆商品／服務明細。

---