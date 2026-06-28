# 安全政策 Security Policy

## 支援範圍

本 Repository 為 **PetFlow Enterprise 官方文件**（Documentation-Only），不含可執行程式碼。
本安全政策涵蓋：

- 文件中可能洩漏的機密資訊（金鑰、憑證、個資、內部端點）。
- 文件描述之設計若存在明顯安全瑕疵（架構、權限、資料保護建議）。
- 與本 Repository 相關的供應鏈風險（CI、範本、相依套件）。

## 回報安全性問題

**請勿** 透過公開 Issue 或 Discussion 回報安全性弱點。

請以私下管道回報：

- Email：`security@petflow.example`（請替換為正式信箱）
- 或使用 GitHub 的 **Security Advisories**（Repository → Security → Report a vulnerability）。

回報時請盡量提供：

1. 問題描述與影響範圍。
2. 重現步驟或受影響的文件位置。
3. 你建議的修正方式（若有）。

## 處理時程（目標）

| 階段 | 目標時間 |
| --- | --- |
| 收到回報後確認 | 3 個工作天內 |
| 初步評估與分級 | 7 個工作天內 |
| 修正與發佈 | 視嚴重程度而定 |

## 揭露原則

我們採 **負責任揭露（Responsible Disclosure）**。在修正釋出前，請勿公開細節。
我們會在修正後於 [CHANGELOG.md](../CHANGELOG.md) 致謝（除非你希望匿名）。

## 文件安全鐵則

- 文件中**嚴禁**出現真實金鑰、密碼、Token、個資。
- 範例一律使用假資料（如 `sk_test_xxx`、`user@example.com`）。
- 涉及合規（個資、寵物登記）之內容須符合 [`docs/28_安全性/`](../docs/28_安全性/README.md) 規範。
