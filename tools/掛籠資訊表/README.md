# 掛籠資訊表產生器

由外部資料檔產生「貓咪掛籠資訊表」可編輯 Word 檔。

- 版面：上方貓咪橫幅 + 9 欄可編輯表格 + 下方貓咪腳印。
- 尺寸：每張 **81.1 × 79 mm**、每頁 **6 張（2×3）**。
- 文字皆可於 Word 直接點選編輯；對位由表格保證，不會漂移。

> ⚠️ **個資不進版控**：真實晶片號、特寵業字號、貓舍名、寵物清冊都不放進 git。
> 本目錄的 `.gitignore` 已排除 `pets.json`、`licenses.json`、`*.xlsx`、`*.docx`。

## 安裝

```bash
pip install -r requirements.txt
```

## 使用

1. 複製範本並填入你自己的資料（這兩個檔不會進 git）：
   ```bash
   cp pets.sample.json pets.json
   cp licenses.sample.json licenses.json
   ```
2. 編輯 `pets.json`（每隻一筆）與 `licenses.json`（登記站 → 特寵業字號）。
3. 產生 Word：
   ```bash
   python generate_cage_cards.py --pets pets.json --licenses licenses.json --out 掛籠資訊表.docx
   ```

## 資料格式

`pets.json`（陣列，每筆欄位）：

| 欄位 | 說明 |
| --- | --- |
| `chip` | 主要晶片號碼（15 碼，會自動格式化為 5-5-5） |
| `name` | 寵物名稱（可留空） |
| `sex` | 性別（公 / 母） |
| `breed` | 品種 |
| `birth` | 生日（YYYY/MM/DD） |
| `neuter` | 絕育情形（預設「未絕育」） |
| `color` | 毛色特徵（可留空） |
| `station` | 來源＝最近辦理變更登記站；用來對應 `licenses.json` 的字號 |
| `dam_chip` | 來源母貓晶片號碼 |

`licenses.json`：`{ "登記站名稱": "特寵業…字第…號" }`。若某站不在此表，會改用該筆 `pets.json` 的 `license` 欄（若有）。

## 兩台電腦同步

- **程式與貓咪範本圖**（本目錄）透過 git 同步。
- **你的資料檔**（`pets.json`、`licenses.json`、清冊 xlsx）不進 git；請自行以雲端硬碟或隨身碟在兩台電腦間複製。

## 資產

`assets/` 內含掛籠資訊表版面圖：`template.png`（完整範本）、`banner.png`（上橫幅）、`footer.png`（下腳印）。
