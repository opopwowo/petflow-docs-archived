# 變更紀錄 CHANGELOG

本檔遵循 [Keep a Changelog](https://keepachangelog.com/zh-TW/1.1.0/) 格式，
版本號遵循 [語意化版本 SemVer](https://semver.org/lang/zh-TW/)。

> 本 Repository 為文件專案，版本對應「文件規格」的演進，而非程式碼釋出。

---

## [Unreleased]

### Added
- 新增「掛籠資訊表 Cage Card」規格文件（`docs/13_寵物管理/掛籠資訊表.md`）：欄位定義、狀態機、API 草案、列印與 QR Code 公開頁、法遵與非功能需求。
- 新增「掛籠資訊表 API 合約」（`docs/11_API設計/掛籠資訊表-API.md`）：OpenAPI 3.1 合約草案，含端點、schema、錯誤碼與狀態碼約定。
- 規劃中的文件項目見各模組 `README.md` 的「待完成文件」章節。

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

[Unreleased]: https://github.com/
[0.1.0]: https://github.com/
