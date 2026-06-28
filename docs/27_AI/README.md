# AI 設計 AI Design

> 定義產品的 AI 功能與技術設計。

## 模組介紹

本模組規劃 PetFlow 的 AI 能力：寵物影像辨識（品種 / 個體）、健康風險提示、配種建議、智慧客服與資料摘要，採 Cloudflare Workers AI / Vectorize，並遵守安全與隱私原則。

## 目的

- 盤點 AI 使用案例與價值
- 設計模型 / 服務選型（Workers AI / Vectorize / 外部 LLM）
- 規範資料使用、隱私與防濫用
- 定義評估指標與回饋迴路

## 待完成文件

- [ ] AI 功能清單與優先序
- [ ] 影像辨識設計（品種 / 個體）
- [ ] 健康風險與配種建議設計
- [ ] RAG / 向量檢索設計（Vectorize）
- [ ] LLM 整合與 Prompt 規範
- [ ] AI 安全、隱私與評估

## 後續章節

- [18 照片管理](../18_照片管理/README.md)：影像來源
- [15 健康管理](../15_健康管理/README.md)：健康資料
- [11 API 設計](../11_API設計/README.md)：AI 端點

## 相關模組

- [28 安全性](../28_安全性/README.md)
- [16 配種管理](../16_配種管理/README.md)

---

> 本文件屬於 PetFlow Enterprise 官方文件。撰寫前請先閱讀根目錄 `CLAUDE.md`。狀態：草稿（Draft）。
