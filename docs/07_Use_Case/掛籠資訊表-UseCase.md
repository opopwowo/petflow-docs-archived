# 掛籠資訊表 Use Case Cage Card Use Cases

> 掛籠資訊表（Cage Card）之使用案例規格草案。功能規格見 [13 寵物管理 › 掛籠資訊表](../13_寵物管理/掛籠資訊表.md)；對應 API 見 [11 API 設計 › 掛籠資訊表 API](../11_API設計/掛籠資訊表-API.md)。

## 參與者（Actor）

| Actor | 說明 |
| --- | --- |
| 門市人員 | 建立 / 編輯 / 列印卡片 |
| 店長 | 具發布、下架與售價顯示設定權限 |
| 消費者 | 掃描 QR Code 瀏覽公開頁(免登入) |
| 系統 | 寵物售出時自動下架、發送通知 |

## Use Case 一覽

| ID | 名稱 | 主要 Actor |
| --- | --- | --- |
| UC-CC-01 | 由寵物建立掛籠資訊表 | 門市人員 |
| UC-CC-02 | 發布掛籠資訊表 | 店長 |
| UC-CC-03 | 列印掛籠資訊表 | 門市人員 |
| UC-CC-04 | 掃描 QR Code 瀏覽公開頁 | 消費者 |
| UC-CC-05 | 寵物售出自動下架 | 系統 |

---

## UC-CC-01 由寵物建立掛籠資訊表

- **前置條件**：使用者已登入且具 `cage-card:create`;目標寵物存在且屬同租戶。
- **後置條件**：建立一張 `draft` 卡片,引用寵物 `petId`。

**主流程**

1. 使用者於寵物檔案頁點「建立掛籠資訊表」。
2. 系統帶出寵物引用欄位(品種、晶片號、來源…)供預覽。
3. 使用者填入籠位編號、選樣板、設定顯示欄位。
4. 系統驗證(schema、法定揭示欄位未被關閉)。
5. 系統建立 `draft` 卡片並產生 `qrSlug`,寫入 Audit Log。

**替代 / 例外**

- 4a. 關閉了法定揭示欄位 → 回 `422 MANDATORY_FIELD_HIDDEN`。
- 1a. 該寵物已有有效卡片 → 提示改為編輯既有卡片(部分唯一索引約束)。
- 2a. 寵物屬其他租戶 → `403 CROSS_TENANT_FORBIDDEN`。

---

## UC-CC-02 發布掛籠資訊表

- **前置條件**：卡片為 `draft`;使用者具 `cage-card:update`。
- **後置條件**：卡片轉為 `published`,QR 公開頁可存取。

**主流程**

1. 店長於卡片編輯頁點「發布」。
2. 系統執行完整性檢核(必填欄位、法定揭示欄位齊全)。
3. 通過 → 狀態 `draft → published`,記錄 `publishedAt` 與 Audit Log。

**例外**

- 2a. 檢核未過 → `409 INCOMPLETE_FOR_PUBLISH`,列出缺漏欄位。

---

## UC-CC-03 列印掛籠資訊表

- **前置條件**：卡片為 `published`;使用者具 `cage-card:print`。
- **後置條件**：產生 PDF,`printCount + 1`。

**主流程**

1. 使用者選樣板後點「列印」。
2. 系統於 Worker 產生 PDF、暫存 R2、回傳短效簽章連結。
3. `printCount` 累加,寫入 Audit Log。

**例外**

- 1a. 卡片非 `published` → `409 CAGE_CARD_NOT_PUBLISHED`。

---

## UC-CC-04 掃描 QR Code 瀏覽公開頁

- **前置條件**：無(免登入);`qrSlug` 對應之卡片為 `published`。
- **後置條件**：回傳揭示欄位;不洩漏內部識別碼 / `tenantId`。

**主流程**

1. 消費者掃描卡片 QR Code,開啟 `/p/{qrSlug}`。
2. 系統查詢對應卡片,確認為 `published`。
3. 回傳寵物揭示欄位與照片(行動版)。

**例外**

- 2a. 卡片為 `draft` / `archived` / 已軟刪除 → `404`(不透露存在與否)。

---

## UC-CC-05 寵物售出自動下架

- **前置條件**：寵物狀態變更為「已售出 / 已歿」。
- **後置條件**：其 `published` 卡片轉為 `archived`;公開頁回傳 404。

**主流程**

1. 寵物狀態於寵物管理模組變更。
2. 系統(Domain Event / Application 層)找出該寵物的有效卡片。
3. 卡片 `published → archived`,部分唯一索引釋放,寫入 Audit Log。
4. 發送通知門市(見 [26 通知中心](../26_通知中心/README.md))。

**例外**

- 2a. 無有效卡片 → 無動作(冪等)。

## 待完成

- [ ] Use Case Diagram(總覽圖)
- [ ] 與 [08 流程圖](../08_流程圖/README.md) 之流程對應
- [ ] 前置 / 後置條件彙整入模組總表

## 相關文件

- [13 寵物管理 › 掛籠資訊表](../13_寵物管理/掛籠資訊表.md)、[11 API 設計 › 掛籠資訊表 API](../11_API設計/掛籠資訊表-API.md)
- [07 Use Case](README.md)、[24 RBAC](../24_RBAC/README.md)

---

> 本文件屬於 PetFlow Enterprise 官方文件。撰寫前請先閱讀根目錄 `CLAUDE.md`。狀態：草稿（Draft）。
