# PetFlow Enterprise（晴寵）官方文件

> AI智慧寵物管理平台 — 陪伴每一隻毛孩的一生 · 唯一事實來源（Single Source of Truth）

歡迎來到 **PetFlow Enterprise** 的官方產品文件網站。本網站集中所有產品規格與設計文件，未來所有 AI 工具（Claude Code、Cursor、ChatGPT、Gemini）與工程師都必須依照這些文件開發。

!!! warning "目前階段：Documentation-Only"
    本專案目前**僅撰寫文件，尚未開始撰寫任何功能程式碼**。任何開發前，請先閱讀 `CLAUDE.md`（位於 Repository 根目錄）。

## 系統定位

PetFlow Enterprise（中文品牌名：**晴寵**）是面向 **寵物店、繁殖場、寵物服務業者與飼主** 的 **多租戶 SaaS 平台**，將寵物生命週期管理（飼養、健康、配種、官方登記、照片紀錄）數位化、自動化、合規化。

- **商業模式**：B2B SaaS 訂閱制 + 加值服務
- **技術定位**：Cloudflare Native · API First · Mobile First · AI Enhanced
- **核心價值**：合規、效率、信任

## 架構總覽

```text
使用者裝置層 (Web / PWA / 行動)
        │  REST API (API First)
Edge 層 (Cloudflare Pages / Workers / WAF)
        │
應用層 (Clean Architecture + DDD)
   Presentation → Service → Domain → Infrastructure
   RBAC · Audit Log · Multi-Tenant · Soft Delete
        │  Repository Pattern
資料層 (D1 · R2 照片 · KV · Queues · Vectorize)
```

## 文件導覽

使用左側 / 上方導覽列瀏覽全部 32 個模組，建議閱讀順序：

| 對象 | 建議順序 |
| --- | --- |
| 商業決策者 | 產品願景 → 市場分析 → 商業模式 → Roadmap |
| 產品經理 | 需求分析 → 使用者角色 → User Story → Use Case → 流程圖 |
| 工程師 / AI | 先讀 `CLAUDE.md` → 系統架構 → 資料庫設計 → API 設計 → 對應模組 |
| 設計師 | UIUX 設計 → 對應功能模組 |

## 模組總覽

- **商業**：[產品願景](01_產品願景/README.md)、[市場分析](02_市場分析/README.md)、[商業模式](03_商業模式/README.md)
- **需求**：[需求分析](04_需求分析/README.md)、[使用者角色](05_使用者角色/README.md)、[User Story](06_User_Story/README.md)、[Use Case](07_Use_Case/README.md)、[流程圖](08_流程圖/README.md)
- **設計**：[系統架構](09_系統架構/README.md)、[資料庫設計](10_資料庫設計/README.md)、[API 設計](11_API設計/README.md)、[UIUX 設計](12_UIUX設計/README.md)
- **功能模組**：[寵物管理](13_寵物管理/README.md)、[飼主管理](14_飼主管理/README.md)、[健康管理](15_健康管理/README.md)、[配種管理](16_配種管理/README.md)、[官方登記助手](17_官方登記助手/README.md)、[照片管理](18_照片管理/README.md)
- **商業化**：[會員訂閱](19_會員訂閱/README.md)、[付款系統](20_付款系統/README.md)
- **平台**：[SaaS](21_SaaS/README.md)、[Multi-Tenant](22_MultiTenant/README.md)、[多店管理](23_多店管理/README.md)、[RBAC](24_RBAC/README.md)、[Audit Log](25_AuditLog/README.md)、[通知中心](26_通知中心/README.md)、[AI](27_AI/README.md)
- **維運**：[安全性](28_安全性/README.md)、[部署](29_部署/README.md)、[測試](30_測試/README.md)
- **規劃**：[Roadmap](31_Roadmap/README.md)、[版本紀錄](32_版本紀錄/README.md)

---

> 📌 開發規則 `CLAUDE.md`、貢獻指南 `CONTRIBUTING.md`、變更紀錄 `CHANGELOG.md` 位於 Repository 根目錄，請於 GitHub 上閱讀。
