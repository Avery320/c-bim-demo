# 行政區

國土測繪中心縣市界供搜尋、區域篩選及圖資裁切使用。來源與授權見 [政府資料開放平臺](https://data.gov.tw/dataset/7442)。

## 模組定位

行政區提供 GIS 共用的縣市範圍與搜尋索引。資料集的縣市筆數及裁切圖磚由產製流程寫入各自分類；瀏覽器載入時將這些筆數與縣市索引合併。

## 主要功能

- 產製縣碼、名稱、座標範圍與縣市面幾何。
- 提供縣市搜尋、範圍條件與資料集裁切基準。
- 以共用 GEOS 裁切建立區域資產及縣市筆數。

## 模組架構

### 目錄結構

```text
public/gis/administrative/
├── manifest.json                # 產生的分類資料與服務契約
├── counties/                    # 縣市界與搜尋索引
└── readme.md                    # 分類說明與維護入口
```

### 資料流程

```text
官方縣市 SHP → administrative/datasets.mjs
    → counties.geojson + index.json
    → AdministrativeRegions → 搜尋 / 縣市篩選
縣市界 + 主題完整向量 → 共用裁切 → 各分類區域資產與筆數
```

## 主要資料

| 檔案                                          | 內容                                                            |
| --------------------------------------------- | --------------------------------------------------------------- |
| [counties.geojson](counties/counties.geojson) | 縣市面幾何                                                      |
| [index.json](counties/index.json)             | 縣碼、名稱與座標範圍；各資料集的縣市筆數保存於所屬分類 manifest |

縣市幾何與索引以縣碼連接；[MapGeometry](../../../src/gis/data/MapGeometry.ts) 同時供發布前裁切及瀏覽器空間查詢使用。

## 邊界

- 縣市界使用於查詢範圍與地圖輪廓；主題圖層的外觀及 3D 顯示由各圖層與共用 Renderer 管理。
- 各資料集的縣市統計位於所屬分類 manifest，透過縣市界雜湊連接裁切版本。

## 依賴方向

箭頭表示「使用者 → 被依賴模組」。

```text
refresh → administrative/datasets → shared/source + MapGeometry
AdministrativeRegions → GisAssetLoader / GisSnapshot → 本分類資產與 manifest
```

共用資料契約、查詢與渲染入口見 [GIS 模組](../../../src/gis/README.md)。

## 更新入口

[資料定義](../../../scripts/gis/administrative/catalog.mjs) · [轉接與裁切](../../../scripts/gis/administrative/datasets.mjs) · [資產紀錄](manifest.json)

更新：`pnpm gis:refresh`。
