# 變更紀錄 CHANGELOG

本檔遵循 [Keep a Changelog](https://keepachangelog.com/zh-TW/1.1.0/) 格式，
版本號遵循 [語意化版本 SemVer](https://semver.org/lang/zh-TW/)。

> 本 Repository 為文件專案，版本對應「文件規格」的演進，而非程式碼釋出。

---

## [Unreleased]

### Added
- 規劃中的文件項目見各模組 `README.md` 的「待完成文件」章節（09 以後之模組）。

---

## [0.2.0] - 2026-07-02

### Added
- 完成商業層與需求層（01–08）全部模組之初稿文件,共 37 份：
  - **01 產品願景**：願景宣言、使命與核心價值、價值主張畫布、北極星指標與 KPI 定義、產品三年願景藍圖。
  - **02 市場分析**：市場規模 TAM/SAM/SOM 試算、競品分析表、SWOT 分析、產業趨勢與法規環境調查、目標市場區隔、GTM 進入市場策略。
  - **03 商業模式**：商業模式畫布、訂閱方案與定價策略、單位經濟模型、收入來源與加值服務清單、成本結構分析。
  - **04 需求分析**：BRD、PRD、功能需求清單（FR 編號）、非功能性需求（NFR 編號）、需求追溯矩陣。
  - **05 使用者角色**：Persona 卡片、系統角色定義表、角色功能權限矩陣、使用者旅程地圖。
  - **06 User Story**：撰寫規範（INVEST）、各模組使用者故事清單（US 編號）、驗收條件範本、Story Map。
  - **07 Use Case**：Use Case 圖總覽、各模組 Use Case 規格書（UC 編號）、前置後置條件清單、例外與錯誤情境彙整（EXC 目錄）。
  - **08 流程圖**：核心業務流程圖、狀態機圖、時序圖、流程圖繪製與命名規範。

### Changed
- 01–08 各模組 `README.md`：「待完成文件」改為「文件清單」,狀態更新為「初稿完成（v0.2.0）」。
- `mkdocs.yml` 與 `SUMMARY.md`：導覽加入 01–08 全部子文件。

### Notes
- 建立全 repo 共同基準：訂閱方案定價（Free/Starter $599/Pro $1,499/Enterprise $3,999 起）、六位 Persona、系統角色（SUPER_ADMIN/OWNER/ADMIN/MANAGER/STAFF/VET/VIEWER）、需求編號體系（BG/FR/NFR/US/UC/EXC）、寵物狀態機（ACTIVE/FOR_SALE/SOLD/DECEASED）。
- 本階段仍為 **Documentation-Only**，未撰寫任何功能程式碼。

---

## [0.1.0] - 2026-06-28

### Added
- 建立 PetFlow-Docs 文件 Repository 初始骨架。
- 新增根目錄文件：`README.md`、`CLAUDE.md`、`CHANGELOG.md`、`CONTRIBUTING.md`、`SUMMARY.md`、`LICENSE`、`mkdocs.yml`。
- 建立 `docs/` 下 32 個模組資料夾與各自的 `README.md`（含模組介紹、目的、待完成文件、後續章節）。
- 新增 Git 設定：`.gitignore`、`.editorconfig`、`.gitattributes`。
- 新增 GitHub 範本：Issue Template、Pull Request Template、Discussion Template、`SECURITY.md`、`CODE_OF_CONDUCT.md`。
- 建立 MkDocs（Material）文件網站設定，可 `mkdocs serve` 瀏覽。

### Notes
- 本階段為 **Documentation-Only**，尚未撰寫任何功能程式碼。

---

[Unreleased]: https://github.com/opopwowo/PetFlow-Docs
[0.2.0]: https://github.com/opopwowo/PetFlow-Docs
[0.1.0]: https://github.com/opopwowo/PetFlow-Docs
