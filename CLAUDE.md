# CLAUDE.md — PetFlow Enterprise 開發規則

> 本檔案為 **Claude Code、Cursor、ChatGPT、Gemini** 等所有 AI 開發工具的最高指導原則。
> **每次開始撰寫任何程式碼之前，AI 必須先完整閱讀本檔案，並閱讀對應模組的 `docs/` 文件。**

---

## 0. 最高原則（Read First）

1. **文件先行（Docs First）**：沒有 `docs/` 文件，就不寫程式碼。任何實作都必須對應一份規格文件。
2. **唯一事實來源（SSOT）**：`docs/` 是唯一事實來源。程式碼與文件衝突時，以文件為準；若文件有誤，先修文件再寫碼。
3. **目前階段為 Documentation-Only**：在收到明確指示前，**不得撰寫任何功能程式碼**。
4. **繁體中文**：所有文件、註解、Commit 訊息、PR 說明一律使用繁體中文、UTF-8、Markdown。
5. **每次編碼前的檢查清單**（見文末 [編碼前檢查清單](#編碼前檢查清單)）必須逐項確認。

---

## 1. Clean Architecture（乾淨架構）

採用分層架構，依賴方向**由外向內**，內層不得依賴外層。

```text
┌──────────────────────────────────────────────┐
│  Presentation 層（Controller / Route / UI）     │  ← 框架、HTTP
├──────────────────────────────────────────────┤
│  Application 層（Service / Use Case）           │  ← 流程編排
├──────────────────────────────────────────────┤
│  Domain 層（Entity / Value Object / 規則）       │  ← 核心商業邏輯（無依賴）
├──────────────────────────────────────────────┤
│  Infrastructure 層（Repository 實作 / 外部服務）  │  ← DB、API、檔案
└──────────────────────────────────────────────┘
```

- **Domain 層** 不得 import 任何框架或基礎設施（純 TypeScript）。
- 依賴透過 **介面（interface）** 注入，遵守依賴反轉。
- 跨層只能透過明確定義的介面與 DTO 溝通。

---

## 2. SOLID 原則

| 原則 | 要求 |
| --- | --- |
| **S** 單一職責 | 一個類別/函式只負責一件事 |
| **O** 開放封閉 | 對擴充開放、對修改封閉，以介面擴充行為 |
| **L** 里氏替換 | 子型別可替換父型別而不破壞行為 |
| **I** 介面隔離 | 拆分小介面，不強迫實作用不到的方法 |
| **D** 依賴反轉 | 依賴抽象（interface），不依賴具體實作 |

---

## 3. DDD（領域驅動設計）

- 以 **限界上下文（Bounded Context）** 劃分模組：寵物、飼主、健康、配種、登記、訂閱、付款等。
- 核心建構元素：**Entity、Value Object、Aggregate、Aggregate Root、Domain Service、Domain Event、Repository（介面）**。
- 統一語言（Ubiquitous Language）：程式碼命名須與 `docs/` 用語一致（例：`Pet`、`Owner`、`BreedingRecord`、`Vaccination`）。
- 商業規則放在 Domain 層，不可外洩到 Controller 或 Repository。

---

## 4. Repository Pattern（儲存庫模式）

- Domain / Application 層只依賴 **Repository 介面**，不接觸具體 ORM/SQL。
- Repository 介面定義於 Domain 層，實作於 Infrastructure 層。
- 一個 Aggregate Root 對應一個 Repository。
- 範例約定：

```ts
// Domain 層：介面
export interface PetRepository {
  findById(id: PetId, tenantId: TenantId): Promise<Pet | null>;
  save(pet: Pet): Promise<void>;
  // 查詢一律帶 tenantId，禁止跨租戶查詢
}
```

---

## 5. Service Layer（服務層）

- Application 層以 **Service / Use Case** 封裝流程編排（交易、權限、事件發佈）。
- Controller **極薄**：只負責解析請求、呼叫 Service、回傳回應。
- 商業流程不可寫在 Controller 或 Repository。
- 每個 Use Case 應對應 `docs/07_Use_Case/` 的一筆紀錄。

---

## 6. API First（API 優先）

- 先設計 API 合約（OpenAPI 3.1），再實作。合約存放於 [`docs/11_API設計/`](docs/11_API設計/README.md)。
- RESTful 規範：
  - 資源命名用複數名詞：`/pets`、`/owners`、`/breeding-records`
  - 正確使用 HTTP 動詞與狀態碼（200/201/204/400/401/403/404/409/422/500）
  - 統一回應結構、統一錯誤格式、分頁/排序/過濾規範一致
  - 一律帶版本前綴：`/api/v1/...`
- 所有 API 預設需要驗證與授權，並受 **RBAC** 與 **Multi-Tenant** 限制。

---

## 7. Material Design 3（介面規範）

- UI 一律遵循 **Material Design 3（Material You）** 規範，細節見 [`docs/12_UIUX設計/`](docs/12_UIUX設計/README.md)。
- 使用 Design Token（色彩、字級、圓角、間距、Elevation）。
- 支援動態色彩、深色模式、無障礙（WCAG 2.1 AA）。
- 元件狀態完整：default / hover / focus / pressed / disabled / error。

---

## 8. Cloudflare Native（雲端原生）

- 平台預設使用 **Cloudflare** 生態：
  - **Pages**：前端託管
  - **Workers**：後端 API（Edge 運算）
  - **D1**：關聯式資料庫
  - **R2**：照片/檔案儲存（見 [`docs/18_照片管理/`](docs/18_照片管理/README.md)）
  - **KV**：快取/設定
  - **Queues**：非同步任務（通知、影像處理）
  - **Vectorize / Workers AI**：AI 功能（見 [`docs/27_AI/`](docs/27_AI/README.md)）
  - **Access / WAF**：存取控制與防護
- 設計時須考量 Edge 限制（無長駐記憶體、執行時間限制、冷啟動）。

---

## 9. TypeScript Strict（嚴格型別）

- `tsconfig` 啟用 `strict: true`（含 `noImplicitAny`、`strictNullChecks` 等）。
- **禁止 `any`**；必要時用 `unknown` 並做型別收斂。
- 對外輸入一律以 **schema 驗證**（如 Zod）後再進入 Domain。
- 公開函式須有明確的回傳型別。
- 善用 Branded Type 表示識別碼（`PetId`、`TenantId`），避免誤用。

---

## 10. Responsive（響應式）

- **Mobile First**，斷點一致（手機 / 平板 / 桌機）。
- 觸控目標 ≥ 48×48dp，版面採彈性網格。
- 在主流瀏覽器與行動裝置上驗證。

---

## 11. Soft Delete（軟刪除）

- 預設**不做實體刪除**；以 `deleted_at`（或 `is_deleted`）標記。
- 所有查詢預設排除已軟刪除資料。
- 提供還原（restore）能力；硬刪除須走特別流程並記錄 Audit Log。
- Schema 設計細節見 [`docs/10_資料庫設計/`](docs/10_資料庫設計/README.md)。

---

## 12. Audit Log（稽核日誌）

- 所有**寫入操作（建立/修改/刪除/還原）** 必須記錄稽核日誌。
- 至少包含：`who`（操作者）、`what`（動作/實體）、`when`（時間）、`where`（IP/裝置）、`before/after`（變更內容）、`tenantId`。
- 稽核日誌為**唯讀、不可竄改**。
- 細節見 [`docs/25_AuditLog/`](docs/25_AuditLog/README.md)。

---

## 13. RBAC（角色權限控管）

- 採 **Role-Based Access Control**：使用者 → 角色 → 權限。
- 每個 API/操作都須宣告所需權限，預設**最小權限原則（Deny by default）**。
- 與 Multi-Tenant 結合：使用者只能存取其租戶資料。
- 角色與權限矩陣見 [`docs/24_RBAC/`](docs/24_RBAC/README.md)。

---

## 14. Migration & Rollback（資料庫遷移與回滾）

- 所有 Schema 變更透過**版本化 Migration** 管理，禁止手動改線上資料庫。
- 每一條 Migration 必須提供 **Up（升級）** 與 **Down（回滾）**。
- Migration 須**向後相容**（先加欄位→寫入→切換→清理，採擴充再收斂策略）。
- 上線前須在測試環境驗證升級與回滾。
- 規範見 [`docs/10_資料庫設計/`](docs/10_資料庫設計/README.md) 與 [`docs/29_部署/`](docs/29_部署/README.md)。

---

## 編碼前檢查清單

開始撰寫任何程式碼前，AI 必須逐項確認：

- [ ] 已閱讀本 `CLAUDE.md`。
- [ ] 已閱讀對應模組的 `docs/` 文件與其 PRD/規格。
- [ ] 已確認需求屬於目前允許的開發範圍（目前為 **Documentation-Only**，未獲指示不寫程式碼）。
- [ ] 設計符合 Clean Architecture 分層與依賴方向。
- [ ] 遵守 SOLID、DDD、Repository Pattern、Service Layer。
- [ ] API 先有合約（API First / OpenAPI）。
- [ ] 已考量 Multi-Tenant 隔離、RBAC 權限、Audit Log、Soft Delete。
- [ ] TypeScript strict、無 `any`、輸入經 schema 驗證。
- [ ] DB 變更具備 Migration 的 Up/Down 並可回滾。
- [ ] UI 遵循 Material Design 3 且響應式。
- [ ] 部署符合 Cloudflare Native 限制。
- [ ] 文件與程式碼一致，必要時回填文件。

---

## 命名與風格約定（摘要）

| 項目 | 約定 |
| --- | --- |
| 檔案 | `kebab-case`（`pet-repository.ts`） |
| 類別 / 型別 | `PascalCase`（`PetService`） |
| 變數 / 函式 | `camelCase`（`createPet`） |
| 常數 | `UPPER_SNAKE_CASE` |
| Commit | Conventional Commits（`docs:`、`feat:`、`fix:`…） |
| 語言 | 繁體中文 / UTF-8 / Markdown |

> 任何與本檔牴觸的實作都視為錯誤，須先修正或先更新本檔。
