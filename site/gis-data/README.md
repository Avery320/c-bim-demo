# GIS 資料

此目錄保存隨 C-BIM 網站部署的 GIS 靜態快照。瀏覽器只讀取 [`src/gis/snapshot.json`](../../src/gis/snapshot.json) 指定的本地檔案；更新腳本才向原資料機關取得資料。OpenFreeMap 底圖與 NLSC 線上影像圖層不屬於此快照。

## 模組架構

| 資料 | 用途 |
| --- | --- |
| `snapshots/<id>/` | 依檔案內容雜湊命名的一組可部署快照。 |
| `src/gis/snapshot.json` | 指定目前快照及每份資料的檔名、格式、來源、下載時間、雜湊與授權狀態。 |
| `administrative/index.json`／`counties.geojson` | 縣市選單、範圍、資料筆數與邊界。 |
| 原始範圍資料／`*-by-county.pmtiles` | 分別供全臺顯示與縣市篩選；點位直接使用同一份含 `countyCode` 的 GeoJSON。 |

流程：`scripts 製作並驗證 → snapshot.json 指向成果 → GisSnapshot 提供讀取入口`。GeoJSON 及索引範圍使用經緯度 EPSG:4326；清單中 PMTiles 的 `crs` 表示圖磚投影 EPSG:3857，其 MVT 幾何使用圖磚內的局部座標。[GDAL 的 PMTiles 格式說明](https://gdal.org/en/stable/drivers/vector/pmtiles.html)及 [Tippecanoe 的輸入投影定義](https://github.com/felt/tippecanoe/blob/main/README.md#projection-of-input)可用於區分圖磚投影與製作時輸入的經緯度。

## 目前資料與原始來源

| 資料 ID | 原機關與直接取得處 | 本地處理 |
| --- | --- | --- |
| `tw-admin-index`、`tw-admin-counties` | 內政部國土測繪中心 [直轄市、縣市界線（TWD97 經緯度）](https://data.gov.tw/dataset/7442)的 SHP 原檔 | 保留 22 個縣市的官方代碼、名稱、範圍與多邊形；索引另記錄各縣市的本地水資源圖徵筆數。前端選單先讀索引，選取縣市時才讀界線。原檔版本 `1140318`。 |
| `wra-basins` | 經濟部水利署 [WFS `BASIN`](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer) | 保留流域界與顯示欄位，GeoJSON。 |
| `wra-basins-by-county` | 同上 WFS；加上內政部國土測繪中心縣市界線 | 取原始流域輪廓與縣界內的交集，寫入縣市代碼並製成線段 PMTiles；不把縣界誤畫成流域界。 |
| `wra-river-level-stations` | 經濟部水利署 [WFS `RIVWLSTA_e`](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer) | 保留測站位置與顯示欄位，GeoJSON；無即時水位。 |
| `wra-groundwater-wells` | 經濟部水利署 [WFS `gwobwell_e`](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer) | 保留觀測井位置與顯示欄位，GeoJSON；無即時讀數。 |
| `wra-river-polygons` | 經濟部水利署 [WFS `RIVERPOLY`](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer)；[河川河道資料集](https://data.gov.tw/dataset/25781) | 保留河道面及 `RIVER_NAME`，用 Tippecanoe 製成 PMTiles。河道面外框直接作為線條，不另複製一份河川線。 |
| `wra-river-polygons-by-county` | 同上 WFS；加上內政部國土測繪中心縣市界線 | 離線裁切跨縣市河道面，並另外保存原始輪廓在縣內的線段；寫入縣市代碼並製成 PMTiles。 |
| `wra-reservoirs` | 經濟部水利署 [WFS `reservoir`](https://maps.wra.gov.tw/arcgis/services/WMS/GIC_WMS/MapServer/WFSServer)；[水庫集水區](https://data.gov.tw/dataset/129474) | 保留集水區面、名稱與圖徵 ID，GeoJSON；不是水庫蓄水面，也無即時蓄水資訊。 |
| `wra-reservoirs-by-county` | 同上 WFS；加上內政部國土測繪中心縣市界線 | 離線裁切跨縣市集水區面，並另外保存原始輪廓在縣內的線段；寫入縣市代碼並製成 PMTiles。 |
| `wra-dams` | 經濟部水利署 [水庫堰壩位置圖](https://data.gov.tw/dataset/25776)的 [SHP 原檔](https://gic.wra.gov.tw/gis/gic/API/Google/DownLoad.aspx?fname=SWRESOIR&filetype=SHP) | 用 `shpjs` 讀取壓縮檔內的座標系與屬性，轉成 WGS84 GeoJSON，僅留名稱、英文名稱、壩高。原檔標示建置日期 `20200512`。 |
| `tw-detention-basins` | [臺南市政府水利局資料集](https://data.gov.tw/dataset/108523)的 [JSON](https://soa.tainan.gov.tw/Api/Service/Get/fabe9ae6-a046-4346-9b4d-60b037fdb91b)；[桃園市政府水務局資料集](https://data.gov.tw/dataset/152950)的 [Big5 CSV](https://opendata.tycg.gov.tw/api/dataset/b99ce968-1bd4-4fbb-89ea-c4a597285cb8/resource/17b5306b-33ba-4ba5-b631-e96c8153b3fb/download) | 臺南 TM97 坐標轉 WGS84、面積 ha 轉 m²；桃園經緯度直接使用。合併成點位 GeoJSON，非池區範圍。 |

更新腳本是 [`scripts/refresh-gis-assets.mjs`](../../scripts/refresh-gis-assets.mjs)。它向水利署 WFS 要求 `GetFeature`、GeoJSON 與 EPSG:4326；清理顯示欄位後才寫入快照。行政區界線由 `shpjs` 讀取官方 SHP，點位用多邊形判定縣市代碼，面資料用 GEOS 裁切成縣市片段，輪廓線則從原始多邊形邊界取縣內線段。2026-10-04 官方來源回傳河道面 13,262 筆、水庫面 80 筆、壩體 98 筆、臺南滯洪池 45 筆、桃園滯洪池 25 筆。筆數是這次取得的結果，不是未來更新時應固定不變的總數。

國土利用現況與水利用地是 NLSC 線上影像圖層。兩層可照常開啟；縣市篩選只套用於本地水資源向量圖資，影像仍顯示原範圍。MapLibre 的圖徵篩選不能裁切影像圖磚；若要裁切影像，需另行確認官方來源能力與使用條款。

農田水利署另有[灌排渠道系統圖](https://data.gov.tw/dataset/45224)，公開項目以 WMS 顯示。依本專案目前「由原機關資料重建本地快照」的選擇，渠道圖層暫不提供；原先取自參考專案的渠道檔已撤下。若將來重建渠道，需先確認可供本地轉換的官方資料、版本與使用條款。

## 授權與顯名聲明

2026-10-05 已核對下列原機關資料頁：均標示「政府資料開放授權條款－第1版」。本專案依[該條款](https://data.gov.tw/license)重製、轉換及公開傳輸資料；使用者須遵守相同條款與原資料顯名要求。原機關未對本專案的轉換成果提供精度保證。

| 提供機關／原資料名稱 | 官方授權依據與版本 | 本地資料對應 |
| --- | --- | --- |
| 內政部國土測繪中心／直轄市、縣市界線（TWD97 經緯度） | [資料集 7442](https://data.gov.tw/dataset/7442)，原檔版本 `1140318` | 縣界、行政區索引及三份縣市裁切成果的縣界來源。 |
| 經濟部水利署／河川流域範圍圖 | [資料集 9823](https://data.gov.tw/dataset/9823)，官方清冊 `BASIN`，建置日期 `20201007` | `wra-basins` 及其縣市輪廓。 |
| 經濟部水利署／河川水位測站位置圖現存站 | [資料集 25784](https://data.gov.tw/dataset/25784)，官方清冊 `RIVWLSTA_e`，建置日期 `20260331` | `wra-river-level-stations`。 |
| 經濟部水利署／地下水觀測井位置圖現存站 | [資料集 32725](https://data.gov.tw/dataset/32725)，官方清冊 `gwobwell_e`，建置日期 `20231206` | `wra-groundwater-wells`。 |
| 經濟部水利署／河川河道 | [資料集 25781](https://data.gov.tw/dataset/25781)，官方清冊 `RIVERPOLY`，統計年度 `2000` | `wra-river-polygons` 及其縣市片段。 |
| 經濟部水利署／水庫集水區 | [資料集 129474](https://data.gov.tw/dataset/129474)，官方清冊 `RESERVOIR`，建置日期 `20230505` | `wra-reservoirs` 及其縣市片段。不是[水庫蓄水範圍 `ressub`](https://data.gov.tw/dataset/13795)。 |
| 經濟部水利署／水庫堰壩位置圖 | [資料集 25776](https://data.gov.tw/dataset/25776)，官方清冊 `SWRESOIR`，建置日期 `20200512` | `wra-dams`。 |
| 臺南市政府水利局／臺南市滯洪池位置及相關資訊 | [資料集 108523](https://data.gov.tw/dataset/108523)，API 於 `capturedAt` 取得的版本 | `tw-detention-basins` 的臺南點位。 |
| 桃園市政府水務局／桃園市滯洪池 | [資料集 152950](https://data.gov.tw/dataset/152950)，CSV 於 `capturedAt` 取得的版本 | `tw-detention-basins` 的桃園點位。 |

水利署資料頁的 JSON 資源清冊列出原檔 `fname`；以上名稱對應本專案使用的同機關 WFS 圖層或 SHP 下載。清冊建置日期不代表 WFS 與原檔具有相同內容版本；實際下載時間與內容版本由快照的 `capturedAt`、`sha256` 記錄。

縣市裁切成果沿用水利署與國土測繪中心兩方的來源授權；行政區索引另彙整各水資源來源的圖徵筆數。資料不是原機關原始發布檔，欄位清理、座標轉換、裁切及格式轉換由 C-BIM 完成。網站地圖的來源註記可連到本文件，文件也隨靜態成果一起部署。

## 更新與核對

使用根目錄 README 指定的 Node／pnpm 版本與 Volta 執行方式，並安裝 [Tippecanoe](https://github.com/felt/tippecanoe) 2.79 或相容版本。執行 `pnpm gis:refresh` 會向原機關取得水利與行政區資料、轉換、驗證，最後一次切換 `snapshot.json`；失敗不切換。`pnpm gis:verify` 只檢查現有快照的檔案、雜湊、筆數與清單，不連外下載。

`public/gis-data/snapshots/<id>/` 的 ID 由 12 份檔案 SHA-256 組成；內容變動才產生新目錄。`capturedAt` 是本專案下載時間，不是機關測量或發布日期。提交新快照時要連同 `snapshot.json` 一起提交；確認舊版本無人使用後才移除舊目錄。正式快照不加入 `.gitignore`。

這些圖資只供視覺參考，不代替法定河川界、工程測量或即時觀測。已核對來源的 `rightsEvidence` 集中在 `scripts/gis-snapshot.mjs` 資料目錄，更新時寫入清單，不因資料內容更新而丟失。新增或變更來源時須重新核對條款及更新本文件；未提供證據的來源仍為 `unverified`。`pnpm gis:verify-public` 核對快照來源與授權依據是否符合目錄，證據不足或不一致時阻止公開部署。部署 PMTiles 後，以 `pnpm gis:range <站台根網址>` 核對 HTTP Range 讀取。
