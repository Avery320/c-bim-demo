# 水資源 Water

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關：經濟部水利署；滯洪池資料由臺南市政府水利局、桃園市政府水務局發布。
- 取得方式：本地固定資料，隨專案部署；人工執行更新才重新取得，沒有排程更新或更新時間偵測。
- 畫面名稱、資料身分、檔案路徑及完整下載參數由 [manifest.json](../manifest.json) 統一定義；授權核對紀錄與排除原因也保存在其中。

## 主題與檔案

| 畫面名稱 | 完整資料檔案 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 流域 Basin | [basins.geojson](basins/basins.geojson) | GeoJSON／EPSG:4326；流域面與邊界，共 143 筆 | 2026-10-09 |
| 河川 River | [rivers.geojson.gzip](rivers/rivers.geojson.gzip) | GeoJSON + gzip／EPSG:4326；河道面，共 13,261 筆 | 2026-10-09 |
| 水庫集水區 Reservoir Catchments | [reservoir-catchments.geojson](reservoirs/reservoir-catchments.geojson) | GeoJSON／EPSG:4326；集水區面 80 筆 | 2026-10-09 |
| 水庫壩體 Reservoir Dams | [reservoir-dams.geojson](reservoirs/reservoir-dams.geojson) | GeoJSON／EPSG:4326；壩體位置點 98 筆 | 2026-10-09 |
| 滯洪池點位 Detention | [detention-basins.geojson](detention-basins/detention-basins.geojson) | GeoJSON／EPSG:4326；位置點，共 70 筆 | 2026-10-09 |
| 河川水位測站位置 | [river-level-stations.geojson](river-level-stations/river-level-stations.geojson) | GeoJSON／EPSG:4326；位置點，共 349 筆 | 2026-10-09 |
| 地下水觀測井位置 | [groundwater-wells.geojson](groundwater-wells/groundwater-wells.geojson) | GeoJSON／EPSG:4326；位置點，共 804 筆 | 2026-10-09 |
| 淹水潛勢 Flood 650mm/24h | [flood-potential-650mm-24h.geojson.gzip](flood-potential/flood-potential-650mm-24h.geojson.gzip) | GeoJSON + gzip／EPSG:4326；模擬淹水範圍面，共 2,356 筆 | 2026-10-09 |

## 流域 Basin

- 主題目錄：`basins/`；資料 ID：`wra-basins`。
- 原始格式：水利署 WFS GeoJSON（`BASIN`）。
- 原始來源：[水利署流域 WFS](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer?TYPENAMES=WMS_GIC_WMS:BASIN)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/9823)。
- 衍生檔：[basins-by-county.pmtiles](basins/basins-by-county.pmtiles)，供縣市裁切顯示。

143 筆靜態流域，保留 GmlID、名稱與流域代碼。原始名稱依原資料保存，不能用作法定界線。

## 河川 River

- 主題目錄：`rivers/`；畫面主題 ID：`wra-river-channel`，原始資料 ID：`wra-river-polygons`。
- 原始格式：水利署 WFS GeoJSON（`RIVERPOLY`）。
- 原始來源：[水利署河道面 WFS](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer?TYPENAMES=WMS_GIC_WMS:RIVERPOLY)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/25781)。
- 顯示檔：[rivers.pmtiles](rivers/rivers.pmtiles)；縣市衍生檔：[rivers-by-county.pmtiles](rivers/rivers-by-county.pmtiles)。

完整資料 13,261 筆，保留 GmlID。實際是河道面，不是自行推算的中心線；不作法定河川界線判定。完整幾何直接由原機關資料製作，未從圖磚還原。

## 水庫集水區／壩體

- 主題目錄：`reservoirs/`；畫面主題 ID：集水區 `wra-reservoir-water`、壩體 `wra-reservoir-water-dam`；各自控制顯示與外觀。
- 原始格式：集水區為 WFS GeoJSON（`reservoir`）；壩體為 ZIP／SHP（`SWRESOIR`）。

| 內容／資料 ID | 原始來源 | 授權依據 |
| --- | --- | --- |
| 集水區／`wra-reservoirs` | [水利署集水區 WFS](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer?TYPENAMES=WMS_GIC_WMS:reservoir) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/129474) |
| 壩體／`wra-dams` | [水利署壩體 SHP](https://gic.wra.gov.tw/gis/gic/API/Google/DownLoad.aspx?fname=SWRESOIR&filetype=SHP) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/25776) |

集水區縣市衍生檔：[reservoir-catchments-by-county.pmtiles](reservoirs/reservoir-catchments-by-county.pmtiles)。

80 筆集水區及 98 筆壩體共用一個畫面控制項，保留兩份來源與身分。集水區不是水庫水面，壩體點不代表水域範圍。壩高單位為 m，沒有即時蓄水量。

## 滯洪池點位 Detention

- 主題目錄：`detention-basins/`；資料 ID：`tw-detention-basins`。
- 原始格式：臺南為 JSON API、TWD97 TM2 座標；桃園為 Big5 CSV、經緯度。

| 發布機關 | 原始來源 | 授權依據 |
| --- | --- | --- |
| 臺南市政府水利局 | [臺南市資料 API](https://soa.tainan.gov.tw/Api/Service/Get/fabe9ae6-a046-4346-9b4d-60b037fdb91b) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/108523) |
| 桃園市政府水務局 | [桃園市資料下載](https://opendata.tycg.gov.tw/api/dataset/b99ce968-1bd4-4fbb-89ea-c4a597285cb8/resource/17b5306b-33ba-4ba5-b631-e96c8153b3fb/download) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/152950) |

70 筆僅涵蓋臺南、桃園，非全國清單。臺南面積由 ha 依既有轉換保存為 m²；桃園未提供的面積維持 null。點位不是池區界線，也沒有容量保證或即時水位。

## 河川水位測站位置

- 主題目錄：`river-level-stations/`；資料 ID：`wra-river-level-stations`。
- 原始格式：水利署 WFS GeoJSON（`RIVWLSTA_e`）。
- 原始來源：[水利署水位測站 WFS](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer?TYPENAMES=WMS_GIC_WMS:RIVWLSTA_e)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/25784)。

349 筆現存測站位置，保留站碼、原始狀態與位置。只有站址，沒有即時水位；開啟圖層不會請求觀測數值。

## 地下水觀測井位置

- 主題目錄：`groundwater-wells/`；資料 ID：`wra-groundwater-wells`。
- 原始格式：水利署 WFS GeoJSON（`gwobwell_e`）。
- 原始來源：[水利署地下水觀測井 WFS](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer?TYPENAMES=WMS_GIC_WMS:gwobwell_e)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/32725)。

804 筆觀測井位置，保留井碼及原始狀態。沒有即時讀數，不等同廢棄物設施污染監測井。

## 淹水潛勢 Flood 650mm/24h

- 主題目錄：`flood-potential/`；資料 ID：`wra-flood-650mm-24h`。
- 原始格式：水利署 ZIP／SHP（`flood_650mm_24hr`）。
- 原始來源：[水利署淹水潛勢 SHP](https://gic.wra.gov.tw/gis/gic/API/Google/DownLoad.aspx?fname=flood_650mm_24hr&filetype=SHP)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/25766)。
- 顯示檔：[flood-potential-650mm-24h.pmtiles](flood-potential/flood-potential-650mm-24h.pmtiles)；縣市衍生檔：[flood-potential-650mm-24h-by-county.pmtiles](flood-potential/flood-potential-650mm-24h-by-county.pmtiles)。
- 圖例設定：[flood-depths.json](flood-potential/flood-depths.json)，共用官方深度分級、標籤與顯示顏色，沒有座標幾何。

2,356 筆第三代淹水潛勢，原資料建置日期為 2019-12-30、上架日期為 2022-08-15。情境為 24 小時定量降雨 650 mm，深度級距單位為 m；顯示全部深度，沒有深度選單。屬固定模擬情境，不是即時雨量或淹水。原檔異常深度文字、列號與幾何處理紀錄沿用原值。

## 共用檔案與維護方式

- PMTiles 採 EPSG:3857，供地圖顯示；完整 GeoJSON 採 EPSG:4326，供顯示、查詢與分析。`.geojson.gzip` 為壓縮的完整資料，未從圖磚反推幾何。
- `*-by-county.pmtiles` 使用[國土測繪中心官方縣市界](../administrative/readme.md)產製，保留原始邏輯圖徵身分；各資產來源記錄於 manifest。
- 原始版本與日期維持既有紀錄，來源未提供的日期不推算；排除與幾何修復紀錄保留於資料及 manifest。
- 人工更新沿用 `pnpm gis:refresh --water`；`--rebuild` 從本地完整資料重建衍生資產，不重新下載。
- 主題資料夾只管理資料檔案，說明集中維護於本文件。
