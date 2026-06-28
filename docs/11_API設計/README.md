# API 設計 API Design

> API First：先定合約，後實作。

## 模組介紹

本模組以 OpenAPI 3.1 定義 RESTful API 合約，規範資源命名、HTTP 動詞與狀態碼、統一回應與錯誤格式、分頁排序過濾、版本控制、驗證授權，是前後端與第三方整合的契約。

## 目的

- 制定 RESTful 設計規範與命名慣例
- 以 OpenAPI 3.1 撰寫 API 合約
- 統一回應結構、錯誤碼與分頁規範
- 定義版本控制（/api/v1）與相容性政策

## 待完成文件

- [ ] API 設計規範（命名 / 動詞 / 狀態碼）
- [ ] OpenAPI 3.1 規格檔（總綱）
- [ ] 統一回應與錯誤碼字典
- [ ] 認證與授權（含 RBAC / Multi-Tenant 標頭）
- [ ] 分頁 / 排序 / 過濾規範
- [ ] Rate Limit 與冪等性（Idempotency）

## 後續章節

- [10 資料庫設計](../10_資料庫設計/README.md)：資源對應資料
- [24 RBAC](../24_RBAC/README.md)：端點權限
- [27 AI](../27_AI/README.md)：AI 端點

## 相關模組

- [09 系統架構](../09_系統架構/README.md)
- [07 Use Case](../07_Use_Case/README.md)

---

> 本文件屬於 PetFlow Enterprise 官方文件。撰寫前請先閱讀根目錄 `CLAUDE.md`。狀態：草稿（Draft）。
