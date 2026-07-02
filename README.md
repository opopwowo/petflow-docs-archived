# PetFlow Enterprise（晴寵）— 官方產品文件

> AI智慧寵物管理平台 — 陪伴每一隻毛孩的一生
> 寵物事業一站式 SaaS 管理平台 · 官方文件中樞（Single Source of Truth）

[![Docs](https://img.shields.io/badge/docs-MkDocs-blue.svg)](#mkdocs-文件網站)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-Documentation--Only-orange.svg)](#)
[![Lang](https://img.shields.io/badge/lang-繁體中文-red.svg)](#)

---

## 一、專案介紹

**PetFlow-Docs** 是 **PetFlow Enterprise** 的官方產品文件 Repository。

本 Repository **不存放任何商業程式碼**，僅作為產品的「唯一事實來源（Single Source of Truth）」，集中管理所有規格、設計與決策文件。

未來所有 AI 開發工具（**Claude Code、Cursor、ChatGPT、Gemini** 等）與人類工程師，**都必須依照 `docs/` 內的文件進行開發**。文件先行（Docs First），程式碼後行。

### 涵蓋文件類型

| 類型 | 說明 |
| --- | --- |
| PRD | Product Requirement Document（產品需求文件） |
| BRD | Business Requirement Document（商業需求文件） |
| 系統架構 | System Architecture |
| Database Design | 資料庫設計 |
| API Design | API 設計 |
| UI/UX Design | 介面與體驗設計 |
| SaaS Design | 多租戶 SaaS 架構設計 |
| AI Design | AI 功能設計 |
| Official Registration Design | 官方登記助手設計 |
| Roadmap | 產品路線圖 |
| Development Rules | 開發規範 |

---

## 二、系統定位

PetFlow Enterprise（中文品牌名：**晴寵**）是一套面向 **寵物店、繁殖場、寵物醫療與飼主** 的 **多租戶 SaaS 平台**，目標是把寵物的生命週期管理（飼養、健康、配種、官方登記、照片紀錄）數位化、自動化。

- **品牌識別**：正式名稱 PetFlow Enterprise；使用者介面與 App 顯示名稱為「晴寵」；標語「AI智慧寵物管理平台 — 陪伴每一隻毛孩的一生」。詳見 [品牌識別](docs/01_產品願景/01_願景宣言.md#54-品牌識別brand-identity)。

- **目標客戶**：寵物店 / 連鎖門市 / 專業繁殖者 / 寵物服務業者
- **商業模式**：B2B SaaS 訂閱制 + 加值服務
- **技術定位**：Cloudflare Native、API First、Mobile First、AI Enhanced
- **核心價值**：合規（官方登記）、效率（自動化）、信任（完整稽核）

---

## 三、架構圖

```text
┌─────────────────────────────────────────────────────────────┐
│                        使用者裝置層                            │
│        Web App  ·  PWA  ·  行動裝置  ·  後台管理                 │
└───────────────────────────┬─────────────────────────────────┘
                            │ HTTPS / REST API (API First)
┌───────────────────────────▼─────────────────────────────────┐
│                    Edge 層 (Cloudflare)                       │
│     CDN · WAF · Cloudflare Pages · Workers · Access           │
└───────────────────────────┬─────────────────────────────────┘
                            │
┌───────────────────────────▼─────────────────────────────────┐
│                   應用層 (Clean Architecture)                  │
│   Presentation → Application(Service) → Domain(DDD) → Infra    │
│   RBAC · Audit Log · Multi-Tenant · Soft Delete               │
└───────────────────────────┬─────────────────────────────────┘
                            │ Repository Pattern
┌───────────────────────────▼─────────────────────────────────┐
│                        資料層                                 │
│     D1 / Database · R2(照片) · KV · Queues · Vectorize(AI)     │
└─────────────────────────────────────────────────────────────┘
```

> 完整架構說明請見 [`docs/09_系統架構/`](docs/09_系統架構/README.md)。

---

## 四、文件導覽

文件依照「商業 → 需求 → 設計 → 模組 → 平台 → 維運」的順序編號，建議依序閱讀。

| # | 模組 | 內容重點 |
| --- | --- | --- |
| 01 | [產品願景](docs/01_產品願景/README.md) | Vision、定位、北極星指標 |
| 02 | [市場分析](docs/02_市場分析/README.md) | TAM/SAM/SOM、競品分析 |
| 03 | [商業模式](docs/03_商業模式/README.md) | 收費模式、訂閱方案、單位經濟 |
| 04 | [需求分析](docs/04_需求分析/README.md) | PRD / BRD、功能需求清單 |
| 05 | [使用者角色](docs/05_使用者角色/README.md) | Persona、角色定義 |
| 06 | [User Story](docs/06_User_Story/README.md) | 使用者故事與驗收條件 |
| 07 | [Use Case](docs/07_Use_Case/README.md) | 使用案例 |
| 08 | [流程圖](docs/08_流程圖/README.md) | 業務流程、狀態機 |
| 09 | [系統架構](docs/09_系統架構/README.md) | Clean Architecture、技術選型 |
| 10 | [資料庫設計](docs/10_資料庫設計/README.md) | ER 模型、Schema、Migration |
| 11 | [API 設計](docs/11_API設計/README.md) | REST 規範、OpenAPI |
| 12 | [UI/UX 設計](docs/12_UIUX設計/README.md) | Material Design 3、設計系統 |
| 13 | [寵物管理](docs/13_寵物管理/README.md) | 寵物檔案模組 |
| 14 | [飼主管理](docs/14_飼主管理/README.md) | 飼主/客戶模組 |
| 15 | [健康管理](docs/15_健康管理/README.md) | 疫苗、病歷、健檢 |
| 16 | [配種管理](docs/16_配種管理/README.md) | 配種、血統、繁殖紀錄 |
| 17 | [官方登記助手](docs/17_官方登記助手/README.md) | 寵物登記合規流程 |
| 18 | [照片管理](docs/18_照片管理/README.md) | 影像儲存與處理 |
| 19 | [會員訂閱](docs/19_會員訂閱/README.md) | 訂閱方案與生命週期 |
| 20 | [付款系統](docs/20_付款系統/README.md) | 金流、發票、對帳 |
| 21 | [SaaS](docs/21_SaaS/README.md) | SaaS 平台設計 |
| 22 | [Multi-Tenant](docs/22_MultiTenant/README.md) | 多租戶隔離策略 |
| 23 | [多店管理](docs/23_多店管理/README.md) | 連鎖/多門市 |
| 24 | [RBAC](docs/24_RBAC/README.md) | 角色權限控管 |
| 25 | [Audit Log](docs/25_AuditLog/README.md) | 稽核日誌 |
| 26 | [通知中心](docs/26_通知中心/README.md) | 站內/Email/推播 |
| 27 | [AI](docs/27_AI/README.md) | AI 功能與設計 |
| 28 | [安全性](docs/28_安全性/README.md) | 資安、隱私、合規 |
| 29 | [部署](docs/29_部署/README.md) | CI/CD、環境、Cloudflare |
| 30 | [測試](docs/30_測試/README.md) | 測試策略與品質 |
| 31 | [Roadmap](docs/31_Roadmap/README.md) | 產品路線圖 |
| 32 | [版本紀錄](docs/32_版本紀錄/README.md) | Release Notes |

---

## 五、開發流程（Docs First）

```text
1. 撰寫 / 更新 docs/      ← 任何功能都先有文件
2. 文件 Review 與核准      ← 透過 Pull Request
3. AI / 工程師依文件開發    ← Claude Code 須先讀 CLAUDE.md
4. 實作回填文件（如有差異）  ← 文件與程式碼一致
5. 發版並更新 32_版本紀錄
```

> **鐵則**：沒有文件，就沒有功能。任何程式碼變更都應能對應到 `docs/` 內的一份規格。

---

## 六、Git Flow

本專案採用簡化版 Git Flow：

| 分支 | 用途 |
| --- | --- |
| `main` | 穩定版文件，受保護，僅能經 PR 合併 |
| `develop` | 開發整合分支 |
| `feature/<主題>` | 新增 / 修改文件 |
| `fix/<主題>` | 修正文件錯誤 |
| `release/<版本>` | 發版前整理 |

**提交訊息規範（Conventional Commits）**

```text
docs: 新增寵物管理模組 PRD
docs(api): 補充寵物建立 API 規格
fix(db): 修正 ER 圖外鍵關係
chore: 更新 mkdocs 設定
```

詳見 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 七、如何閱讀文件

### 方法一：直接在 GitHub 閱讀
從 [文件導覽](#四文件導覽) 點擊任一模組的 `README.md`。

### 方法二：MkDocs 文件網站

本地啟動互動式文件網站：

```bash
# 安裝（需 Python 3.9+）
pip install mkdocs-material

# 啟動本機伺服器
mkdocs serve

# 瀏覽器開啟
# http://127.0.0.1:8000
```

建置靜態網站：

```bash
mkdocs build      # 產出至 site/
mkdocs gh-deploy  # 部署至 GitHub Pages
```

### 方法三：閱讀順序建議
- **商業決策者**：01 → 02 → 03 → 31
- **產品經理**：04 → 05 → 06 → 07 → 08
- **工程師 / AI**：先讀 [CLAUDE.md](CLAUDE.md) → 09 → 10 → 11 → 對應模組
- **設計師**：12 → 對應功能模組

---

## 八、給 AI 開發者的提醒

> ⚠️ **Claude Code、Cursor、ChatGPT、Gemini 等任何 AI 工具，在開始撰寫程式碼前，必須先閱讀 [CLAUDE.md](CLAUDE.md) 與相關模組文件。**

本 Repository 目前為 **Documentation-Only** 階段，**請勿在此撰寫任何功能程式碼**。

---

## 授權

本文件採 [MIT License](LICENSE) 授權。

## 相關文件

- [CLAUDE.md](CLAUDE.md) — AI 開發規則
- [CONTRIBUTING.md](CONTRIBUTING.md) — 貢獻指南
- [CHANGELOG.md](CHANGELOG.md) — 變更紀錄
- [SUMMARY.md](SUMMARY.md) — 文件總目錄
- [SECURITY.md](.github/SECURITY.md) — 安全政策
- [CODE_OF_CONDUCT.md](.github/CODE_OF_CONDUCT.md) — 行為準則
