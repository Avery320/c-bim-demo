# GIS 圖資

GIS 圖資系統按分類管理台灣主題資料、分類圖例與線上地圖服務，提供地圖顯示及完整向量篩選所需的資料。

## 模組定位

資料定義與產製位於 `scripts/gis`，部署資產位於 `public/gis`，載入、查詢及顯示位於 `src/gis`。每個分類維護自己的來源與欄位，共用模組處理資產發布、幾何運算及點線面顯示。

## 架構原則

- 分類 `catalog.mjs` 保存來源、授權、欄位、幾何與輸出設定；根 manifest 登錄分類，更新腳本產生各分類 manifest。
- 資料集以固定 ID 連接完整分析向量、顯示資產與縣市裁切資產；GeoJSON 的 `feature.id` 與圖磚的 `_featureId` 共用圖徵身分。
- 完整向量使用 EPSG:4326 `[經度, 緯度]`；空間運算以專案設定的公尺座標系執行，預設為 TWD97。
- 2D／3D 共用篩選流程，各地圖視窗保存自己的條件與結果；3D 顯示使用 2D 計算完成的結果。
- `vectorStyle`／`rasterStyle` 建立共用圖層設定，Renderer 同步資料與外觀；點、線、面的 3D 顯示沿用同一份圖徵及互動狀態。

## 專案結構

```text
scripts/gis/
├── refresh.mjs                  # 更新與本地重建入口
├── shared/                      # 定義彙整、下載、轉換、驗證與發布
└── <分類>/
    ├── catalog.mjs              # 人工維護的來源與資料契約
    └── <轉接器>.mjs             # 官方資料到完整向量的轉換

public/gis/
├── manifest.json                # 分類索引
└── <分類>/
    ├── readme.md                # 資料意義、來源與維護入口
    ├── manifest.json            # 產生的資料集、資產、版本與統計
    └── <主題>/                  # GeoJSON、PMTiles 與圖例

src/gis/
├── data/                        # 資產定義、載入、縣市界與幾何工具
├── query/                       # 條件計畫、worker、差異與結果紀錄
├── layers/                      # 圖層宣告、共用樣式、渲染與光效
└── view/                        # 視窗狀態、搜尋、識別與地形設定
```

## 模組分層

### 應用層

- [refresh.mjs](../../scripts/gis/refresh.mjs)：呼叫分類轉接器，組裝產製與發布流程。
- [MapViewController](../../src/gis/view/MapViewController.ts)／[MapViewManager](../../src/gis/view/MapViewManager.ts)：管理各視窗的圖層、條件與互動，提供面板操作及狀態訂閱。

### Domain 與功能層

- 分類轉接器：解析官方資料，轉換欄位與座標，輸出 `{ id, features }`。
- [MapQuery](../../src/gis/query/MapQuery.ts)／[MapQueryEngine](../../src/gis/query/MapQueryEngine.ts)：編譯條件，執行欄位、縣市及空間篩選，重用未變的運算階段。

### Projection 與 UI 層

- [MapLayerCatalog](../../src/gis/layers/MapLayerCatalog.ts)／[MapLayerStyle](../../src/gis/layers/MapLayerStyle.ts)：登錄圖層，以資料集 ID 建立幾何、欄位、外觀及控制項設定。
- [MapLayerRenderer](../../src/gis/layers/MapLayerRenderer.ts)：將資料結果與互動狀態同步至 MapLibre 圖層；[MapLightBeams](../../src/gis/layers/MapLightBeams.ts) 處理點線光效，[MapTerrain](../../src/gis/view/MapTerrain.ts) 安裝地形與陰影。
- [Map Layers](../../src/ui-templates/sections/map-layers.ts)／[Map Explorer](../../src/ui-templates/sections/map-explorer.ts)：呈現作用中地圖的設定、條件與結果。

### 基礎層

- [shared/catalog](../../scripts/gis/shared/catalog.mjs)／[shared/source](../../scripts/gis/shared/source.mjs)／[shared/snapshot](../../scripts/gis/shared/snapshot.mjs)：彙整資料契約，共用下載、資產產製、驗證及發布。
- [GisSnapshot](../../src/gis/data/GisSnapshot.ts)／[GisAssetLoader](../../src/gis/data/GisAssetLoader.ts)：取得資料集與服務設定，驗證、解壓及解析本地 JSON 資產。
- [MapGeometry](../../src/gis/data/MapGeometry.ts)：共用圖徵身分、投影及 GEOS 幾何操作；[MapLayerTypes](../../src/gis/layers/MapLayerTypes.ts) 定義圖層與外觀契約。

## 主要資料

| 資料                 | 內容與維護入口                                     |
| -------------------- | -------------------------------------------------- |
| 根 `manifest.json`   | 人工維護分類入口與順序                             |
| 分類 `catalog.mjs`   | 人工維護來源、授權、幾何、欄位及輸出設定           |
| 分類 `manifest.json` | 腳本發布的資料集、資產、服務、雜湊、筆數與排除紀錄 |
| GeoJSON／PMTiles     | 完整分析向量、顯示及區域圖磚，以固定圖徵 ID 對照   |
| 圖例 JSON            | 分類值與配色，部分分類含面拉伸的 `heightLevel`     |

瀏覽器契約與操作入口見 [GIS 模組的主要資料](../../src/gis/README.md#主要資料)及[共用 API](../../src/gis/README.md#共用-api)。

## 依賴方向

箭頭表示「使用者 → 被依賴模組」。

```text
refresh.mjs
├──→ 分類轉接器
└──→ shared/catalog / source / snapshot

shared/catalog ──→ 分類 catalog
shared/source ───→ MapGeometry
shared/snapshot ─→ 分類 manifest 與資產（讀取／發布）

MapViewController
├──→ MapQueryClient → MapQuery.worker → MapQueryEngine
└──→ MapLayerRenderer → MapLightBeams / MapTerrain

查詢與渲染 ──→ MapLayerCatalog → Map*Layers → MapLayerStyle
                                               └──→ GisSnapshot → 分類 manifest
資產載入 ────→ GisAssetLoader → GisSnapshot
產製與查詢 ──→ MapGeometry → GEOS / proj4
```

查詢與渲染共用資料集登錄；圖磚串流由 MapLibre／PMTiles 處理，完整分析向量由 worker 載入並快取。

## 核心資料流

### 資料產製

```text
官方資料 + 分類 catalog
    → 分類轉接器 → { id, features }
    → 共用幾何與資產產製
    → 完整 GeoJSON / 顯示 PMTiles / 縣市統計與裁切圖磚
    → 驗證與發布 → 分類 manifest + 資產

線上服務設定 → 分類 catalog → 分類 manifest
```

分類 manifest 保存資產路徑、雜湊、筆數、縣市統計及排除紀錄。GeoJSON 保留完整向量；PMTiles 提供顯示與區域圖磚。

### 載入、篩選與顯示

```text
分類 manifest → GisSnapshot → 資料集 / 資產 / 服務設定

圖層與條件 → MapQuery → MapQueryClient → worker
             → 完整向量載入 → 欄位 / 縣市 / 空間運算
             → DatasetView
               ├──→ MapLayerRenderer → 2D / 3D 顯示
               └──→ 結果紀錄 → Results

顯示圖磚 / 線上影像 / DEM → MapLibre 按位置及縮放載入
```

範圍條件裁切幾何，距離條件保留符合距離的完整圖徵。查詢重用未變階段，結果與渲染按圖徵 ID 更新；選取、隱藏與改色由同一份視窗互動狀態管理。

### 3D 顯示

| 幾何 | 顯示方式                                      |
| ---- | --------------------------------------------- |
| 點   | 原點位加上向外擴散、向上漸淡的圓錐光效        |
| 線   | 原線加上垂直光幕，沿篩選後路徑繪製並保留斷點  |
| 面   | MapLibre 面拉伸；配色與可選高度級距共用分類表 |

光效及拉伸參數位於 `MapLayerStyle`，地表高程來自 DEM。實作說明見 [GIS 模組的 3D 顯示](../../src/gis/README.md#3d-顯示)。

## 邊界

- 各分類維護自己的來源、轉接器及圖例；資料載入、查詢與顯示由 GIS 共用模組處理。
- 完整分析向量提供全量查詢，PMTiles 提供地圖顯示；線上影像與 DEM 使用服務契約。
- 篩選在 2D 幾何上完成；點線光效與面拉伸使用同一份結果及圖徵互動狀態。
- DEM 表示地表高程，主題高度表示顯示尺度；各資料的意義與適用範圍見分類文件。

## 資料分類與來源

| 分類                               | 內容                                  |
| ---------------------------------- | ------------------------------------- |
| [底圖與地形](basemap/readme.md)    | OpenFreeMap 底圖、Mapterhorn 高程圖磚 |
| [土地使用](land-use/readme.md)     | 官方土地使用影像與位置查詢            |
| [交通](transport/readme.md)        | 車站、軌道與道路                      |
| [水資源](water/readme.md)          | 流域、河道、設施、測站與淹水潛勢      |
| [再生能源](energy/readme.md)       | 離岸風場與風電潛力場址                |
| [環境](environment/readme.md)      | 受保護樹木、生態範圍與天然植群        |
| [廢棄物](waste/readme.md)          | 焚化廠與掩埋場                        |
| [地質](geology/readme.md)          | 斷層、液化潛勢與地表地層              |
| [行政區](administrative/readme.md) | 縣市界與搜尋索引                      |

分類文件記錄資料意義與官方來源、授權依據；開源模組與授權見 [GIS 依賴說明](../../src/gis/README.md#開源依賴與來源)。

## 新增與更新資料

1. 在分類 `catalog.mjs` 定義來源、授權、欄位與輸出資產。
2. 本地資料由分類轉接器輸出 `{ id, features }`，以 `pnpm gis:refresh --<分類>` 產製；服務設定以 `pnpm gis:refresh --catalog` 發布。
3. 在 `src/gis/layers/Map*Layers.ts` 宣告圖層，使用 `vectorStyle`／`rasterStyle` 連接資料集或影像服務。

交通範例：[資料定義](../../scripts/gis/transport/catalog.mjs) → [轉接器](../../scripts/gis/transport/datasets.mjs) → [圖層宣告](../../src/gis/layers/MapTransportLayers.ts)。新增分類時，同步登錄根 manifest、更新入口及 `MapLayerCatalog`。

更新、資產核對與部署需求見 [維護腳本](../../scripts/readme.md)。
