# GIS 圖資

本目錄保存網站部署使用的 GIS 快照。檔案清單、原機關來源、下載時間、內容雜湊與授權依據記錄於 [`src/gis/snapshot.json`](../../src/gis/snapshot.json)。`snapshots/<內容雜湊>/` 為不可變快照；雜湊代表本次發布內容，不代表機關調查年份。

## 地質圖層來源

| 圖層 | 原始來源與實際取得方式 | 版本、顯示方式與限制 |
| --- | --- | --- |
| 活動斷層 Active Faults | 經濟部地質調查及礦業管理中心[地質雲](https://www.geologycloud.tw/map/)使用的[完整 GeoJSON 入口](https://www.geologycloud.tw/data/zh-tw/ActiveFault?all=true)。開放授權依據為[官方活動斷層資料集 6697](https://data.gov.tw/dataset/6697)。 | 本地 `geology/active-faults.geojson`，134 段線位、37 個原始名稱（含三義斷層分支名稱，不等於 37 條官方活動斷層）。完整線位供共用條件；縣市顯示使用 `administrative/gsmma-active-faults-by-county.pmtiles`。保留原始名稱及線位依據，以官方地質雲的紅色、實線／虛線表示觀察／推測或隱伏。原始來源沒有圖徵 ID，結果以列號與快照版本識別；下載時間不當作線位調查年份。 |
| 土壤液化潛勢 Liquefaction | 地礦中心[官方液化平台](https://liquefaction.gsmma.gov.tw/gsmma/public/)與[WMTS 發布清單](https://liquefaction.gsmma.gov.tw/wmts/)公布的 `Soil_cgs_2026` 服務，主機為 `gis.liquid.net.tw`。本專案改用同服務的[完整面 GeoJSON 查詢](https://gis.liquid.net.tw/arcgis/rest/services/P04032/Soil_cgs_2026/MapServer/0/query?where=1%3D1&outFields=OBJECTID%2CTYPE%2CYr&outSR=4326&returnGeometry=true&f=geojson)，以官方總筆數核對完整性；[圖例與欄位定義](https://gis.liquid.net.tw/arcgis/rest/services/P04032/Soil_cgs_2026/MapServer/0?f=pjson)，[開放授權依據 28691](https://data.gov.tw/dataset/28691)。 | 本地分析 EPSG:4326，顯示與縣市裁切 PMTiles 使用同一份面資料；執行時不再請求液化影像。2026-10-06 取得 489 筆，納入高 25／中 63／低 91，共 179 筆；310 筆 `TYPE=NOT` 未提供三類分級及圖例，排除紀錄保留於 manifest。23 筆面經共用 GEOS MakeValid 修復並記錄。保留官方 `OBJECTID`、`TYPE`、`Yr`（2021／2026），年別不視為逐筆調查日期。高／中／低原色 `#ff0000`／`#ffff00`／`#55ff00`；北部六縣市於 2026-09-24 更新，見[版本說明](https://liquefaction.gsmma.gov.tw/gsmma/public/QA2.html)。100 公尺網格呈現區域相對潛勢；空白不代表低潛勢。 |
| 地表地層 Geology | 地礦中心[全臺地質圖圖磚 API](https://geomap.gsmma.gov.tw/api/Tile/v1/oas/)，`getTile.cfm?layer=CGS_CGS_MAP`。[官方 OpenAPI 定義](https://geomap.gsmma.gov.tw/api/Tile/v1/oas/NEW_TWGeolTileAPI.json)。 | 執行時連線取得 EPSG:3857 PNG，保留官方原色及圖中的構造線；屬地表地質圖影像，非地下三維地層。服務在 z1–9／10／11–12／13–17 分別使用百萬／五十萬／二十五萬／五萬分之一資料。年份依原圖幅，詳圖例與圖幅資料見[官方地質資料查詢](https://geomap.gsmma.gov.tw/gsb108-1/)。 |

活動斷層及土壤液化沿用本專案同一份本地快照、來源清單、驗證與共用篩選流程。地表地層仍使用官方影像服務，需有網路連線，不出現在本地檔案 manifest；官方 OpenAPI 另提供 `getTooltip.cfm` 座標查詢，但本專案目前僅顯示圖磚，尚未導入該 API 或完整分析幾何。三個圖層均不讀取參考專案的資料或服務。

地表地層色帶另取自[官方地質圖台](https://geomap.gsmma.gov.tw/gwh/gsb97-1/sys8a/index_3.cfm)使用的 `legend_new/legend_get_pg.cfm` 圖例服務；2026-10-06 以 `SCALE=1000k／0500k／0250k／0050k`、`size=中` 及全臺範圍取得。629 個分類中，607 個提供色樣、22 個未提供，原始名稱、色樣網址與請求範圍保留於 `src/gis/surface-geology-legends.json`。色帶採色樣中央區域最常見的底色，相鄰排列並保留分類邊界；完整紋理仍以官方原圖例為準。沒有色樣的分類不自行配色。

官方自 2025 年 10 月起[取消第一類／第二類活動斷層分類](https://twgeoref.gsmma.gov.tw/GipOpenWeb/wSite/ct?ctNode=239&mp=6&xItem=327367)。原始 GeoJSON 的舊欄位只保留作來源紀錄，不以它建立危險等級或不同顏色。自訂距離區也不能稱為官方地質敏感區。

## 更新與模組架構

| 元件 | 責任 |
| --- | --- |
| `scripts/gis-snapshot.mjs` | 保存資料身分與授權，驗證檔案、筆數與雜湊，完整成功後一次發布快照。 |
| `scripts/refresh-gis-assets.mjs`／`geology-datasets.mjs` | 從原機關下載及檢查資料；`pnpm gis:refresh --geology` 更新活動斷層、液化完整面與顯示／縣市資產及筆數。無參數更新全部資料；`--themes` 更新風電、樹木及廢棄物主題，各部分更新沿用其餘資料內容與取得時間。 |
| `scripts/water-datasets.mjs`、`thematic-datasets.mjs`、`waste-and-flood-datasets.mjs` | 分別維護水利、風電／樹木、廢棄物／淹水的原機關欄位轉換；來源特殊檢查保留於主題，共用產製與發布由 refresh 調度。 |
| `scripts/gis-source.mjs`／`administrative-regions.mjs` | 各主題共用 GEOS 面修復與線／面行政區裁切，保留原線位或面邊界；點位沿用同一份官方縣界。 |
| `src/gis/MapGeologyLayers.ts`／`liquefaction-potentials.json` | 宣告三個圖層的官方來源、原色、圖例、透明度及查看欄位。液化代碼、中文分類、填色與線色共用單一設定。 |
| `MapLayerCatalog` → `MapLayerRenderer` | 共用圖層身分、來源安裝、順序、開關、透明度與底圖切換。官方影像可宣告原色及原生最高階層，放大由 MapLibre 重用最高可用圖磚。 |
| `sections/map-layers.ts` | 使用既有 BUI 元件與綠色群組標題，在「地質 Geology」呈現圖層及參數；共用既有捲動樣式。 |

GIS 快照公開部署前的權利檢查為 `pnpm gis:verify-public`。詳細鑽孔、地下土層與其他主題尚未列入這三個圖層。


## 完整分析資料

河川河道 `analysis/wra-river-polygons.geojson.gzip` 直接取自[水利署河川河道官方資料](https://data.gov.tw/dataset/25781)的 WFS `RIVERPOLY`，保留 GmlID 與完整面幾何，不是由顯示圖磚還原。它是靜態參考河道，不作法定界線判定。

淹水 `analysis/wra-flood-650mm-24h.geojson.gzip` 直接由[水利署淹水潛勢官方資料](https://data.gov.tw/dataset/25766)的原始 SHP 製作，保留原檔列號、官方分級與幾何處理紀錄。650 mm／24 h 是模擬情境，不是即時或實測雨量。

液化 `analysis/gsmma-soil-liquefaction.geojson.gzip` 由上表的官方 ArcGIS GeoJSON 查詢製作，不由 WMTS 或 PMTiles 還原；保留有官方分級的完整面。空間分析讀取完整幾何，再依共同縣市範圍裁切目標與參照。

上述分析資產與 PMTiles 使用同一份清理後資料，來源、授權、排除項目與內容雜湊均記錄於 manifest。縣市界線保留官方原始精度，避免座標四捨五入造成自相交。資料能力及規則說明見 [`GIS 模組`](../../src/gis/README.md)。

分析 GeoJSON 使用 gzip 保留完整精度；快照雜湊驗證壓縮檔，分析 worker 以瀏覽器原生 `DecompressionStream` 解壓縮。檔名使用 `.gzip`，避免 Vite 對 `.gz` 自動設定 HTTP `Content-Encoding`；以檔案位元組提供，壓縮格式由 manifest 的 `encoding` 決定。

## 身分、範圍與本地重建

產製時先固定原始圖徵 ID，再保存 GeoJSON 與圖磚的 `_featureId`。完整資料、縣市片段、填色與原始邊線共用邏輯 ID；點選／清單的版本使用整批快照 ID，實際資產與 SHA-256 另記。沒有官方 ID 的 `row:<列號>` 只保證在該快照內穩定。

縣市筆數依完整圖徵裁切後是否保留同維度幾何計算；共界點可符合相鄰縣市，只有點接觸的線或只有線接觸的面不列入。區內沒有原始邊線的流域仍可能有有效面。每筆圖徵只計一次，不計圖磚／輪廓碎片。

`pnpm gis:refresh --rebuild` 從驗證過的完整本地資料重建顯示圖磚、縣市資產及統計。此操作保留原擷取日期與來源／排除紀錄，不代表重新調查；全部驗證後才切換快照。Node 版本須依專案固定設定。

## 資料欄位整理

滯洪池 `county` 保存「臺南市／桃園市」，供名稱顯示與共同條件使用；`tainan:`／`taoyuan:` 圖徵 ID 與 `_featureId` 保持原值。2026-10-06 從既有本地快照整理名稱，70 筆點位、座標、面積、縣市歸屬、來源取得時間與其餘資料內容均未變更；新的快照雜湊表示這次欄位整理，沒有重新下載機關資料。後續下載在產製階段使用中文名稱，面積空白保存 null，官方零值仍保存 0；本地重建沿用已標準化的完整資料。
