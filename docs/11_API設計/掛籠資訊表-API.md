# 掛籠資訊表 API 合約 Cage Card API

> 掛籠資訊表（Cage Card）之 OpenAPI 3.1 合約草案。依 [CLAUDE.md](../../CLAUDE.md) 第 6 節 **API First**：先定合約、後實作。功能規格見 [13 寵物管理 › 掛籠資訊表](../13_寵物管理/掛籠資訊表.md)。

## 1. 設計原則

- **版本前綴**：所有端點以 `/api/v1` 開頭。
- **資源命名**：複數名詞 `cage-cards`。
- **多租戶**：除公開頁外,皆需 `Authorization` 與租戶情境;伺服器由存取權杖推導 `tenantId`,**用戶端不得自帶** `tenantId` 以防跨租戶越權。
- **RBAC**：每個端點宣告所需權限,預設 Deny(見 [24 RBAC](../24_RBAC/README.md))。
- **軟刪除**:`DELETE` 為軟刪除;查詢預設排除 `deleted_at` 非空者。
- **稽核**:所有寫入操作記錄 Audit Log(見 [25 AuditLog](../25_AuditLog/README.md))。
- **公開頁**:`/public/cage-cards/{qrSlug}` 免登入,僅回傳已發布卡片之揭示欄位,且不得洩漏內部識別碼或 `tenantId`。

## 2. 狀態碼約定

| 狀態碼 | 使用時機 |
| --- | --- |
| `200` | 讀取 / 更新成功 |
| `201` | 建立成功 |
| `204` | 軟刪除成功(無內容) |
| `400` | 請求格式錯誤 / schema 驗證失敗 |
| `401` | 未驗證 |
| `403` | 已驗證但無權限 / 跨租戶存取 |
| `404` | 資源不存在(或公開頁指向非 `published` 卡片) |
| `409` | 狀態轉換衝突(如未通過完整性檢核即發布、重複上架) |
| `422` | 語意驗證失敗(如關閉法定揭示欄位) |
| `500` | 伺服器錯誤 |

## 3. OpenAPI 3.1 合約

```yaml
openapi: 3.1.0
info:
  title: PetFlow Enterprise — 掛籠資訊表 API
  description: 掛籠資訊表（Cage Card）的 RESTful 合約草案。
  version: 0.1.0
servers:
  - url: https://api.petflow.example/api/v1
    description: 正式環境（示意）
security:
  - bearerAuth: []
tags:
  - name: cage-cards
    description: 掛籠資訊表管理（需驗證）
  - name: public
    description: QR Code 公開頁（免登入）
paths:
  /cage-cards:
    get:
      tags: [cage-cards]
      summary: 列出掛籠資訊表
      operationId: listCageCards
      description: 依租戶隔離，支援分頁與過濾。需權限 cage-card:read。
      parameters:
        - $ref: '#/components/parameters/Page'
        - $ref: '#/components/parameters/PageSize'
        - name: storeId
          in: query
          description: 依門市過濾
          schema: { type: string }
        - name: status
          in: query
          description: 依卡片狀態過濾
          schema:
            $ref: '#/components/schemas/CageCardStatus'
        - name: petId
          in: query
          description: 依寵物過濾
          schema: { type: string }
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CageCardListResponse'
        '401': { $ref: '#/components/responses/Unauthorized' }
        '403': { $ref: '#/components/responses/Forbidden' }
    post:
      tags: [cage-cards]
      summary: 由寵物建立掛籠資訊表
      operationId: createCageCard
      description: 以 petId 建立卡片，引用欄位不複製。需權限 cage-card:create。
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CageCardCreate'
      responses:
        '201':
          description: 建立成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CageCard'
        '400': { $ref: '#/components/responses/BadRequest' }
        '401': { $ref: '#/components/responses/Unauthorized' }
        '403': { $ref: '#/components/responses/Forbidden' }
        '422': { $ref: '#/components/responses/UnprocessableEntity' }
  /cage-cards/{id}:
    parameters:
      - $ref: '#/components/parameters/CageCardId'
    get:
      tags: [cage-cards]
      summary: 讀取單一掛籠資訊表
      operationId: getCageCard
      description: 回傳自有欄位並合併寵物引用欄位。需權限 cage-card:read。
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CageCard'
        '401': { $ref: '#/components/responses/Unauthorized' }
        '403': { $ref: '#/components/responses/Forbidden' }
        '404': { $ref: '#/components/responses/NotFound' }
    patch:
      tags: [cage-cards]
      summary: 更新掛籠資訊表 / 狀態轉換
      operationId: updateCageCard
      description: 更新自有欄位或進行狀態轉換。需權限 cage-card:update。
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CageCardUpdate'
      responses:
        '200':
          description: 更新成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CageCard'
        '400': { $ref: '#/components/responses/BadRequest' }
        '401': { $ref: '#/components/responses/Unauthorized' }
        '403': { $ref: '#/components/responses/Forbidden' }
        '404': { $ref: '#/components/responses/NotFound' }
        '409': { $ref: '#/components/responses/Conflict' }
        '422': { $ref: '#/components/responses/UnprocessableEntity' }
    delete:
      tags: [cage-cards]
      summary: 軟刪除掛籠資訊表
      operationId: deleteCageCard
      description: 標記 deleted_at，非實體刪除。需權限 cage-card:delete。
      responses:
        '204': { description: 軟刪除成功 }
        '401': { $ref: '#/components/responses/Unauthorized' }
        '403': { $ref: '#/components/responses/Forbidden' }
        '404': { $ref: '#/components/responses/NotFound' }
  /cage-cards/{id}/print:
    parameters:
      - $ref: '#/components/parameters/CageCardId'
    post:
      tags: [cage-cards]
      summary: 產生列印用 PDF
      operationId: printCageCard
      description: 產生指定樣板之 PDF，回傳 R2 短效簽章連結，printCount 累加。需權限 cage-card:print。
      requestBody:
        required: false
        content:
          application/json:
            schema:
              type: object
              properties:
                templateId:
                  type: string
                  description: 未帶則使用卡片預設樣板
      responses:
        '200':
          description: 產生成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PrintResult'
        '401': { $ref: '#/components/responses/Unauthorized' }
        '403': { $ref: '#/components/responses/Forbidden' }
        '404': { $ref: '#/components/responses/NotFound' }
        '409':
          description: 卡片非 published 狀態，不可列印
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }
  /public/cage-cards/{qrSlug}:
    get:
      tags: [public]
      summary: QR Code 公開頁
      operationId: getPublicCageCard
      description: 免登入。僅回傳「已發布」卡片之揭示欄位；draft/archived/已刪除一律回傳 404。
      security: []
      parameters:
        - name: qrSlug
          in: path
          required: true
          description: 不可猜測的公開短碼
          schema: { type: string }
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PublicCageCard'
        '404': { $ref: '#/components/responses/NotFound' }
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: 伺服器由權杖推導 tenantId，用戶端不得自帶。
  parameters:
    Page:
      name: page
      in: query
      schema: { type: integer, minimum: 1, default: 1 }
    PageSize:
      name: pageSize
      in: query
      schema: { type: integer, minimum: 1, maximum: 100, default: 20 }
    CageCardId:
      name: id
      in: path
      required: true
      description: 掛籠資訊表識別碼
      schema: { type: string }
  responses:
    BadRequest:
      description: 請求格式錯誤
      content:
        application/json:
          schema: { $ref: '#/components/schemas/Error' }
    Unauthorized:
      description: 未驗證
      content:
        application/json:
          schema: { $ref: '#/components/schemas/Error' }
    Forbidden:
      description: 無權限或跨租戶存取
      content:
        application/json:
          schema: { $ref: '#/components/schemas/Error' }
    NotFound:
      description: 資源不存在
      content:
        application/json:
          schema: { $ref: '#/components/schemas/Error' }
    Conflict:
      description: 狀態轉換衝突
      content:
        application/json:
          schema: { $ref: '#/components/schemas/Error' }
    UnprocessableEntity:
      description: 語意驗證失敗（如關閉法定揭示欄位）
      content:
        application/json:
          schema: { $ref: '#/components/schemas/Error' }
  schemas:
    CageCardStatus:
      type: string
      enum: [draft, published, archived]
    Money:
      type: object
      required: [amount, currency]
      properties:
        amount: { type: integer, description: 以最小貨幣單位計（分），避免浮點誤差 }
        currency: { type: string, description: ISO 4217，如 TWD }
    FieldVisibility:
      type: object
      description: 各欄位顯示開關；法定揭示欄位由伺服器鎖定為 true，不可關閉。
      properties:
        price: { type: boolean, default: false }
        vaccination: { type: boolean, default: true }
    PetSnapshot:
      type: object
      description: 由 Pet 即時帶出之唯讀引用欄位（非儲存於卡片）。
      properties:
        name: { type: string }
        breed: { type: string }
        sex: { type: string, enum: [male, female, unknown] }
        birthDate: { type: string, format: date }
        color: { type: string }
        microchipNo: { type: string, description: 法規揭示項目 }
        source: { type: string, description: 繁殖場名稱與特定寵物業許可證字號 }
        vaccinationSummary:
          type: array
          items:
            type: object
            properties:
              item: { type: string }
              date: { type: string, format: date }
    CageCardCreate:
      type: object
      required: [petId, storeId, cageNo, templateId]
      properties:
        petId: { type: string }
        storeId: { type: string }
        cageNo: { type: string }
        templateId: { type: string }
        price: { $ref: '#/components/schemas/Money' }
        note: { type: string }
        fieldVisibility: { $ref: '#/components/schemas/FieldVisibility' }
    CageCardUpdate:
      type: object
      description: 部分更新；status 用於狀態轉換（draft/published/archived）。
      properties:
        cageNo: { type: string }
        templateId: { type: string }
        price: { $ref: '#/components/schemas/Money' }
        note: { type: string }
        fieldVisibility: { $ref: '#/components/schemas/FieldVisibility' }
        status: { $ref: '#/components/schemas/CageCardStatus' }
    CageCard:
      type: object
      required: [id, petId, storeId, cageNo, templateId, qrSlug, status, createdAt, updatedAt]
      properties:
        id: { type: string }
        petId: { type: string }
        storeId: { type: string }
        cageNo: { type: string }
        templateId: { type: string }
        qrSlug: { type: string }
        status: { $ref: '#/components/schemas/CageCardStatus' }
        price: { $ref: '#/components/schemas/Money' }
        note: { type: string }
        fieldVisibility: { $ref: '#/components/schemas/FieldVisibility' }
        pet: { $ref: '#/components/schemas/PetSnapshot' }
        publishedAt: { type: [string, 'null'], format: date-time }
        printCount: { type: integer, default: 0 }
        createdAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }
        deletedAt: { type: [string, 'null'], format: date-time }
    PublicCageCard:
      type: object
      description: 公開頁僅回傳揭示欄位，不含 tenantId / 內部識別碼 / 售價（除非開啟）。
      properties:
        cageNo: { type: string }
        pet: { $ref: '#/components/schemas/PetSnapshot' }
        photos:
          type: array
          items: { type: string, format: uri }
    PrintResult:
      type: object
      required: [downloadUrl, expiresAt, printCount]
      properties:
        downloadUrl: { type: string, format: uri, description: R2 短效簽章連結 }
        expiresAt: { type: string, format: date-time }
        printCount: { type: integer }
    CageCardListResponse:
      type: object
      required: [data, pagination]
      properties:
        data:
          type: array
          items: { $ref: '#/components/schemas/CageCard' }
        pagination:
          type: object
          properties:
            page: { type: integer }
            pageSize: { type: integer }
            total: { type: integer }
    Error:
      type: object
      required: [code, message]
      properties:
        code: { type: string, description: 機器可讀錯誤碼，如 CAGE_CARD_NOT_PUBLISHED }
        message: { type: string, description: 人類可讀訊息（繁體中文） }
        details:
          type: array
          items: { type: object }
```

## 4. 錯誤碼(節錄)

| `code` | HTTP | 說明 |
| --- | --- | --- |
| `CAGE_CARD_NOT_FOUND` | 404 | 卡片不存在或已軟刪除 |
| `CAGE_CARD_NOT_PUBLISHED` | 409 | 對非 `published` 卡片執行列印 |
| `MANDATORY_FIELD_HIDDEN` | 422 | 嘗試關閉法定揭示欄位 |
| `INCOMPLETE_FOR_PUBLISH` | 409 | 未通過完整性檢核即發布 |
| `CROSS_TENANT_FORBIDDEN` | 403 | 跨租戶存取 |

## 5. 待完成

- [ ] 併入 API 總綱 OpenAPI 檔(統一 `components` 與錯誤字典)
- [ ] 與 [10 資料庫設計](../10_資料庫設計/README.md) 之 D1 Schema 對齊欄位型別
- [ ] 補充 Rate Limit 與冪等性(`Idempotency-Key`)規範
- [ ] 樣板清單端點(`GET /cage-card-templates`)

## 相關文件

- [13 寵物管理 › 掛籠資訊表](../13_寵物管理/掛籠資訊表.md)(功能規格)
- [11 API 設計](README.md)、[24 RBAC](../24_RBAC/README.md)、[25 AuditLog](../25_AuditLog/README.md)

---

> 本文件屬於 PetFlow Enterprise 官方文件。撰寫前請先閱讀根目錄 `CLAUDE.md`。狀態：草稿（Draft）。
