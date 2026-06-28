# 資料庫設計 Database Design

> 定義資料模型、Schema 與遷移策略。

## 模組介紹

本模組設計關聯式資料模型（以 Cloudflare D1 為主），涵蓋 ER 模型、表結構、索引、多租戶欄位、軟刪除與稽核欄位，並規範 Migration 的版本化與可回滾。

## 目的

- 建立 ER 模型與正規化設計
- 定義各表 Schema、主鍵 / 外鍵 / 索引
- 落實多租戶（tenant_id）、軟刪除（deleted_at）、稽核欄位
- 規範 Migration 的 Up/Down 與向後相容

## 待完成文件

- [ ] ER 圖（Entity Relationship Diagram）
- [ ] 資料字典（Data Dictionary）
- [ ] 各模組 Schema 定義
- [ ] 索引與查詢效能設計
- [ ] Migration 規範與版本策略
- [ ] 資料保留與封存政策

## 後續章節

- [11 API 設計](../11_API設計/README.md)：資料對外暴露
- [22 Multi-Tenant](../22_MultiTenant/README.md)：租戶隔離
- [25 Audit Log](../25_AuditLog/README.md)：稽核資料

## 相關模組

- [09 系統架構](../09_系統架構/README.md)
- [28 安全性](../28_安全性/README.md)

---

> 本文件屬於 PetFlow Enterprise 官方文件。撰寫前請先閱讀根目錄 `CLAUDE.md`。狀態：草稿（Draft）。
