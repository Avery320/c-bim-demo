# 土地使用

國土測繪中心土地利用現況影像，保留官方配色；點選位置查詢官方分類。

## 模組定位

土地使用分類以官方影像顯示現況，並以位置查詢取得分類說明。服務契約由分類維護，外觀與識別由 GIS 共用模組處理。

## 主要功能

- 使用 WMTS 顯示國土利用現況調查成果圖。
- 使用 WMS 顯示水利用地。
- 透過 MapIdentify 查詢點選位置的土地利用分類。

## 模組架構

### 目錄結構

```text
public/gis/land-use/
├── manifest.json                # 產生的分類資料與服務契約
├── land-use-survey/             # 土地利用影像服務的主題目錄
├── water-land-use/              # 水利用地服務的主題目錄
└── readme.md                    # 分類說明與維護入口
```

### 資料流程

```text
land-use/catalog.mjs → manifest → GisSnapshot
    → MapLandUseLayers / rasterStyle → 官方影像 → MapLibre
地圖點選 → MapIdentify → 官方位置查詢 → Selection Data
```

## 主要資料

| 圖層                   | 服務                                                     |
| ---------------------- | -------------------------------------------------------- |
| 國土利用現況調查成果圖 | [NLSC 圖資服務](https://maps.nlsc.gov.tw/) `LUIMAP` WMTS |
| 水利用地               | [NLSC WMS](https://wms.nlsc.gov.tw/wms) `LUIMAP04`       |

本分類在 `services` 保存 `nlsc-land-use`／`nlsc-water-land-use`；分類 manifest 的 `datasets`／`assets` 為空。

## 邊界

資料為現況調查，非地籍或法定使用分區。影像與位置查詢版本可能不同；本分類提供影像與位置查詢，無本地完整向量。條款依 NLSC 服務，地圖保留官方 attribution。

## 依賴方向

箭頭表示「使用者 → 被依賴模組」。

```text
MapLandUseLayers → rasterStyle → GisSnapshot → land-use manifest
MapViewController → MapIdentify → NLSC 位置查詢
```

共用資料契約、查詢與渲染入口見 [GIS 模組](../../../src/gis/README.md)。

## 更新入口

[服務定義](../../../scripts/gis/land-use/catalog.mjs) · [服務紀錄](manifest.json) · [圖層顯示](../../../src/gis/layers/MapLandUseLayers.ts) · [位置查詢](../../../src/gis/view/MapIdentify.ts)

發布設定：`pnpm gis:refresh --catalog`。
