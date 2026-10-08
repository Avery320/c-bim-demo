# 地質 Geology

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關／服務提供者：經濟部地質調查及礦業管理中心。
- 取得方式：活動斷層、液化面為本地固定資料；地表地層影像由官方服務按需載入。沒有即時監測、排程更新或更新時間偵測。
- 畫面名稱、資料身分、檔案路徑及完整下載參數由 [manifest.json](../manifest.json) 統一定義；授權核對紀錄與排除原因也保存在其中。

## 主題與檔案

| 畫面名稱／資料用途 | 資料檔案／服務 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 活動斷層 Active Faults | [active-faults.geojson](active-faults/active-faults.geojson) | GeoJSON／EPSG:4326；線位，共 134 段 | 2026-10-09 |
| 土壤液化潛勢 Liquefaction | [soil-liquefaction.geojson.gzip](soil-liquefaction/soil-liquefaction.geojson.gzip) | GeoJSON + gzip／EPSG:4326；100 公尺網格潛勢面，共 179 筆 | 2026-10-09 |
| 地表地層 Geology | [官方圖磚 API](https://geomap.gsmma.gov.tw/api/Tile/v1/oas/) | 線上 PNG 影像／EPSG:3857 | 動態更新（按需取得） |
| 地表地層圖例 | [surface-geology-legends.json](surface-geology/surface-geology-legends.json) | 本地 JSON 圖例；地表地層的輔助資料 | 2026-10-09 |

## 活動斷層 Active Faults

- 主題目錄：`active-faults/`；資料 ID：`gsmma-active-faults`。
- 原始格式：地質雲完整 GeoJSON。
- 原始來源：[地質雲活動斷層資料](https://www.geologycloud.tw/data/zh-tw/ActiveFault?all=true)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/6697)。
- 縣市衍生檔：[active-faults-by-county.pmtiles](active-faults/active-faults-by-county.pmtiles)。

134 段完整線位、37 個原始名稱（含分支），不等於 37 條官方活動斷層。沿用官方紅色，以及觀察線位的實線、推測或隱伏線位的虛線。官方自 2025 年 10 月取消第一／第二類分類，舊欄位僅作來源紀錄，不用作危險等級。無官方 ID 時以列號與資料版本識別；自訂距離區不是官方地質敏感區。

## 土壤液化潛勢 Liquefaction

- 主題目錄：`soil-liquefaction/`；資料 ID：`gsmma-soil-liquefaction`。
- 原始格式：官方液化平台 `Soil_cgs_2026` ArcGIS 服務的完整 GeoJSON 查詢。
- 原始來源：[官方完整面資料查詢](https://gis.liquid.net.tw/arcgis/rest/services/P04032/Soil_cgs_2026/MapServer/0/query?where=1%3D1&outFields=OBJECTID%2CTYPE%2CYr&outSR=4326&returnGeometry=true&f=geojson)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/28691)；[官方欄位與圖例](https://gis.liquid.net.tw/arcgis/rest/services/P04032/Soil_cgs_2026/MapServer/0?f=pjson)。
- 顯示檔：[soil-liquefaction.pmtiles](soil-liquefaction/soil-liquefaction.pmtiles)；縣市衍生檔：[soil-liquefaction-by-county.pmtiles](soil-liquefaction/soil-liquefaction-by-county.pmtiles)。
- 圖例設定：[liquefaction-potentials.json](soil-liquefaction/liquefaction-potentials.json)，共用官方分級、標籤與顯示顏色，沒有座標幾何。

官方回應 489 筆：高 25／中 63／低 91，共 179 筆納入；310 筆 `TYPE=NOT` 缺少三類分級與圖例，排除項目保存在 manifest，不能視為低潛勢。23 筆沿用 GEOS MakeValid 修復紀錄。服務版本為 `Soil_cgs_2026`，原欄位 `Yr=2021／2026` 不是逐筆調查日期。高／中／低原色為 `#ff0000`／`#ffff00`／`#55ff00`，執行時讀本地面資料。

## 地表地層 Geology

- 主題目錄：`surface-geology/`；主題 ID：`gsmma-surface-geology`。
- 圖磚入口：`https://geomap.gsmma.gov.tw/api/Tile/v1/getTile.cfm?layer=CGS_CGS_MAP&z={z}&x={x}&y={y}`。
- 圖例來源：[官方圖例服務](https://geomap.gsmma.gov.tw/gwh/gsb97-1/legend_new/legend_get_pg.cfm)。本地圖例沿用既有解析結果，資料新增／更新日期見上表；影像動態取得，不記本地日期。

保留官方原色與構造線，屬地表地質影像，非地下三維地層。z1–9／10／11–12／13–17 分別使用百萬／五十萬／二十五萬／五萬分之一圖幅，年份依原圖幅。官方另有 getTooltip，目前未導入。圖例保存四比例尺的分類、官方色樣網址與缺色樣紀錄；沒有色樣者不配色，也不重新下載色樣。

## 共用檔案與維護方式

- PMTiles 採 EPSG:3857，供地圖顯示；完整 GeoJSON 採 EPSG:4326，供顯示、查詢與分析。縣市衍生檔使用[國土測繪中心官方縣市界](../administrative/readme.md)產製，保留原始邏輯圖徵身分。
- 原始版本與日期維持既有紀錄，來源未提供的日期不推算；排除與幾何修復紀錄保留於資料及 manifest。
- 本地資料人工更新沿用 `pnpm gis:refresh --geology`；`--rebuild` 從本地完整資料重建衍生資產，不重新下載。
- 地表地層影像按需取得，保留官方 attribution，使用限制依原服務說明；沒有本地圖磚副本或圖磚下載日期。官方更新由服務提供者管理，服務設定與本地圖例由開發者人工維護。
- 主題資料夾只管理資料檔案，說明集中維護於本文件。
