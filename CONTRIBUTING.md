# 貢獻指南 CONTRIBUTING

感謝你為 **PetFlow Enterprise 官方文件** 貢獻心力。本指南說明如何撰寫、提交與審查文件。

> 本 Repository 為 **Documentation-Only**。請勿在此提交任何功能程式碼。

---

## 一、貢獻前須知

1. 先閱讀 [README.md](README.md) 了解專案定位。
2. 閱讀 [CLAUDE.md](CLAUDE.md)（AI 與工程師通用的開發規則）。
3. 確認你的變更對應到 `docs/` 內的某一模組。

---

## 二、文件規範

| 項目 | 要求 |
| --- | --- |
| 格式 | Markdown（`.md`） |
| 編碼 | UTF-8（無 BOM） |
| 語言 | 繁體中文 |
| 換行 | LF |
| 標題 | 每份文件一個 `#` H1，層級不可跳級 |
| 圖表 | 優先使用 Mermaid / 純文字 ASCII 圖 |
| 連結 | 使用相對路徑連結其他文件 |
| 命名 | 資料夾 `NN_主題`；檔案 `kebab-case.md` |

每個模組資料夾的 `README.md` 至少包含：**模組介紹、目的、待完成文件、後續章節**。

---

## 三、Git Flow

| 分支 | 用途 |
| --- | --- |
| `main` | 穩定文件，受保護，僅能透過 PR 合併 |
| `develop` | 開發整合分支 |
| `feature/<主題>` | 新增 / 修改文件 |
| `fix/<主題>` | 修正文件錯誤 |
| `release/<版本>` | 發版整理 |

### 流程

```bash
git checkout develop
git pull
git checkout -b feature/pet-management-prd
# 編輯 docs/13_寵物管理/...
git add .
git commit -m "docs(pet): 新增寵物管理 PRD"
git push -u origin feature/pet-management-prd
# 在 GitHub 開 Pull Request → 目標分支 develop
```

---

## 四、Commit 訊息規範（Conventional Commits）

```text
<type>(<scope>): <簡述>
```

| type | 用途 |
| --- | --- |
| `docs` | 文件新增或修改（最常用） |
| `fix` | 修正文件錯誤 |
| `chore` | 設定、雜項（mkdocs、CI、範本） |
| `refactor` | 文件結構重整 |
| `style` | 排版、格式（不影響內容） |

範例：

```text
docs(api): 補充寵物建立 API 規格與錯誤碼
fix(db): 修正 ER 圖 owner 與 pet 的關聯方向
chore: 調整 mkdocs 導覽結構
```

---

## 五、Pull Request 規範

- 一個 PR 聚焦一個主題，避免巨大變更。
- 填寫 [PR 範本](.github/PULL_REQUEST_TEMPLATE.md) 各欄位。
- 連結相關 Issue（`Closes #123`）。
- 通過至少一位審查者核准後方可合併。
- 合併採 **Squash and merge**，保持歷史乾淨。

### 文件審查重點

- [ ] 內容正確、無歧義、與其他文件一致。
- [ ] 用語符合統一語言（與 `docs/` 既有命名一致）。
- [ ] 連結有效、圖表可正常渲染。
- [ ] 符合本文件之文件規範與 `CLAUDE.md` 原則。

---

## 六、本地預覽

```bash
pip install mkdocs-material
mkdocs serve
# http://127.0.0.1:8000
```

---

## 七、回報問題

- 文件錯誤 / 缺漏：開 **Documentation Issue**。
- 功能建議：開 **Feature Request**。
- 安全性問題：請勿公開，依 [SECURITY.md](.github/SECURITY.md) 流程處理。

感謝你的貢獻！🐾
