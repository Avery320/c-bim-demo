# GIS 資料

此目錄保存隨 C-BIM 網站部署的 GIS 靜態快照。瀏覽器只讀取 [`src/gis/snapshot.json`](../../src/gis/snapshot.json) 指定的本地檔案；更新腳本才向原資料機關取得資料。OpenFreeMap 底圖與 NLSC 線上影像圖層不屬於此快照。

## 模組架構

| 資料 | 用途 |
| --- | --- |
| `snapshots/<id>/` | 依檔案內容雜湊命名的一組可部署快照。 |
| `src/gis/snapshot.json` | 指定目前快照及每份資料的檔名、格式、來源、下載時間、雜湊與授權狀態。 |
| `administrative/index.json`／`counties.geojson` | 縣市選單、範圍、資料筆數與邊界。 |
| 原始範圍資料／`*-by-county.pmtiles` | 分別供全臺顯示與縣市篩選；點位直接使用同一份含 `countyCode` 的 GeoJSON。 |
| `energy/`／`trees/` | 能源署場址面及三個地方政府樹木點位；圖層、來源與版本分別識別。 |

流程：`scripts 製作並驗證 → snapshot.json 指向成果 → GisSnapshot 提供讀取入口`。GeoJSON 及索引範圍使用經緯度 EPSG:4326；清單中 PMTiles 的 `crs` 表示圖磚投影 EPSG:3857，其 MVT 幾何使用圖磚內的局部座標。[GDAL 的 PMTiles 格式說明](https://gdal.org/en/stable/drivers/vector/pmtiles.html)及 [Tippecanoe 的輸入投影定義](https://github.com/felt/tippecanoe/blob/main/README.md#projection-of-input)可用於區分圖磚投影與製作時輸入的經緯度。

## 目前資料與原始來源

| 資料 ID | 原機關與直接取得處 | 本地處理 |
| --- | --- | --- |
| `tw-admin-index`、`tw-admin-counties` | 內政部國土測繪中心 [直轄市、縣市界線（TWD97 經緯度）](https://data.gov.tw/dataset/7442)的 SHP 原檔 | 保留 22 個縣市的官方代碼、名稱、範圍與多邊形；索引另記錄各縣市的水資源與樹木圖徵筆數。前端選單先讀索引，選取縣市時才讀界線。原檔版本 `1140318`。 |
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
| `moeaea-offshore-wind-farms` | 經濟部能源署[風力發電附件清單](https://www.moeaea.gov.tw/ECW/populace/Law/LawsList.aspx?kind=6&menu_id=3302)的[場址座標 ZIP 原檔](https://www.moeaea.gov.tw/ECW/populace/Law/wHandLawsList_File.ashx?kind=6&id=5476) | 讀取「已取得有效之風力發電離岸系統設置同意證明文件或籌設許可之離岸風電場址座標」XLSX，按公司／電廠合併儲存格及表列角點次序重建面；TWD97 TM2 121 度轉 WGS84。不是商轉清單。 |
| `moeaea-offshore-wind-potential` | 經濟部能源署[台灣離岸風電潛力場址地理資訊](https://data.gov.tw/dataset/36681)的[原始 TSV 下載](https://www.moeaea.gov.tw/ECW/populace/opendata/wHandOpenData_File.ashx?set_id=145) | 以場址編號分組、保留表列角點次序及官方統計面積，TWD97 TM2 121 度轉 WGS84。下載檔名為 CSV，實際以 Tab 分隔；版本名稱 `11406`。潛力場址不等於核准或可開發範圍。 |
| `taipei-protected-trees` | 臺北市政府文化局[臺北市受保護樹木](https://data.gov.tw/dataset/121274)的[CSV 原檔](https://data.taipei/api/frontstage/tpeod/dataset/resource.download?rid=a7c2db0d-8b6e-42b2-bdcc-ae69a6797e1d) | 保留原始樹木編號、樹種、學名、地址、管理單位及胸徑／胸圍；原始經緯度直接使用。 |
| `new-taipei-protected-trees` | 新北市政府農業局[新北市珍貴樹木](https://data.gov.tw/dataset/125615)的[CSV 原檔](https://data.ntpc.gov.tw/api/datasets/dfd0dd0f-564b-461f-88fd-4493a87387b9/csv/file) | 僅保留「公告列管」；以個別序號為 ID，保留學名、位置、地段、原始量測與推估樹齡。依[同一官方資料集欄位說明](https://staging.data.ntpc.gov.tw/datasets/dfd0dd0f-564b-461f-88fd-4493a87387b9)轉換 TWD97 X／Y；胸徑及胸圍由 cm 換算 m。 |
| `kaohsiung-protected-trees` | 高雄市政府農業局[高雄市列管特定紀念樹木清冊](https://data.gov.tw/dataset/163742)的[JSON 原檔](https://openapi.kcg.gov.tw/Api/Service/Get/914625f5-7800-4502-9573-1e2331d16bc5) | 保留原始編號、樹名、地址、地段及經緯度；未提供的量測資料不補造。 |

更新腳本是 [`scripts/refresh-gis-assets.mjs`](../../scripts/refresh-gis-assets.mjs)。它向水利署 WFS 要求 `GetFeature`、GeoJSON 與 EPSG:4326；清理顯示欄位後才寫入快照。行政區界線由 `shpjs` 讀取官方 SHP，點位用多邊形判定縣市代碼，面資料用 GEOS 裁切成縣市片段，輪廓線則從原始多邊形邊界取縣內線段。2026-10-04 官方來源回傳河道面 13,262 筆、水庫面 80 筆、壩體 98 筆、臺南滯洪池 45 筆、桃園滯洪池 25 筆。筆數是這次取得的結果，不是未來更新時應固定不變的總數。

縣市篩選套用本地水資源與樹木。樹木依座標與官方縣界的交集判斷，來源縣市另外保留供核對，不強制改成提供機關的行政區。海上風場與潛力場址維持原範圍，不用陸地縣界截斷，也不推定最近縣市。

國土利用現況與水利用地是 NLSC 線上影像圖層。兩層可照常開啟，維持原範圍。MapLibre 的圖徵篩選不能裁切影像圖磚；若要裁切影像，需另行確認官方來源能力與使用條款。

農田水利署另有[灌排渠道系統圖](https://data.gov.tw/dataset/45224)，公開項目以 WMS 顯示。依本專案目前「由原機關資料重建本地快照」的選擇，渠道圖層暫不提供；原先取自參考專案的渠道檔已撤下。若將來重建渠道，需先確認可供本地轉換的官方資料、版本與使用條款。

## 授權與顯名聲明

2026-10-05 已核對下列官方資料及授權說明：均標示「政府資料開放授權條款－第1版」。本專案依[該條款](https://data.gov.tw/license)重製、轉換及公開傳輸資料；使用者須遵守相同條款與原資料顯名要求。原機關未對本專案的轉換成果提供精度保證。

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
| 經濟部能源署／取得設置同意或籌設許可之離岸風電場址 | [國家海洋研究院官方中繼資料 9](https://nodass.namr.gov.tw/dataInfo?metadataid=9)明列能源署提供、EPSG:3826 及政府資料開放授權條款第 1 版；原始檔從能源署直接下載 | `moeaea-offshore-wind-farms`。附件內容與中繼資料更新日期可能不同，使用實際下載版本。 |
| 經濟部能源署／台灣離岸風電潛力場址地理資訊 | [資料集 36681](https://data.gov.tw/dataset/36681)，版本名稱 `11406` | `moeaea-offshore-wind-potential`。 |
| 臺北市政府文化局／臺北市受保護樹木 | [資料集 121274](https://data.gov.tw/dataset/121274)，CSV 於 `capturedAt` 取得的版本 | `taipei-protected-trees`。 |
| 新北市政府農業局／新北市珍貴樹木 | [資料集 125615](https://data.gov.tw/dataset/125615)，CSV 於 `capturedAt` 取得的版本 | `new-taipei-protected-trees`。 |
| 高雄市政府農業局／高雄市列管特定紀念樹木清冊 | [資料集 163742](https://data.gov.tw/dataset/163742)，JSON 於 `capturedAt` 取得的版本 | `kaohsiung-protected-trees`。 |

水利署資料頁的 JSON 資源清冊列出原檔 `fname`；以上名稱對應本專案使用的同機關 WFS 圖層或 SHP 下載。清冊建置日期不代表 WFS 與原檔具有相同內容版本；實際下載時間與內容版本由快照的 `capturedAt`、`sha256` 記錄。

縣市裁切成果沿用水利署與國土測繪中心兩方的來源授權；行政區索引另彙整各水資源與樹木來源的圖徵筆數。資料不是原機關原始發布檔，欄位清理、座標轉換、裁切及格式轉換由 C-BIM 完成。網站地圖的來源註記可連到本文件，文件也隨靜態成果一起部署。

## 主題資料涵蓋與待確認項目

2026-10-05 取得的資料及處理結果如下；筆數不是未來更新時固定不變的總數。樹木三來源合計 5,474 筆，不宣稱涵蓋全臺。

| 資料 | 原始資料及目前成果 | 排除或待確認項目 |
| --- | --- | --- |
| 設置同意／籌設許可風場 | 23 筆計畫，22 筆可確認的場址面 | 「台中渢妙離岸風力發電計畫第一期」依表列次序閉合會自相交，附件未提供多區塊連接方式；暫不繪製場址面，不自行重排或使用凸包。 |
| 潛力場址 | 原始 36 處，33 處可確認的場址面 | 第 3、7、36 號依表列次序閉合會自相交，區塊連接方式待確認。第 5 號的角點標籤重複，仍依表列座標順序重建並通過幾何檢查，不依標籤排序。 |
| 臺北樹木 | 3,874 筆 | 有 1 筆座標落在新北市界內；依原始位置顯示並保留臺北來源身分。 |
| 新北樹木 | 1,118 筆原始清單；232 筆解列排除，883 筆可顯示 | 3 筆列管座標不合理：`107-金-12`（252480, 121601）、`107-土-52`（2932283, 2761640）、`110-莊-36`（250227, 1214401），不猜測修補。另 1 筆有效點位落在臺北市，`107-淡-62` 未匹配陸地縣界，保留全臺顯示但不列入縣市結果。 |
| 高雄樹木 | 717 筆 | 使用官方經緯度；沒有樹齡、胸徑或胸圍欄位，不補造。 |

排除的無效座標及待確認場址記於 `snapshot.json` 各資料集的 `omittedFeatures`，圖層詳情顯示可繪製與排除筆數。未匹配縣界的點位使用 `countyCode: null`；原始樹木清單的列管狀態與實際座標品質仍應由原機關核對。本次開發規格見 [`gis-energy-and-trees-spec.md`](../../docs/gis-energy-and-trees-spec.md)。

## 更新與核對

使用根目錄 README 指定的 Node／pnpm 版本與 Volta 執行方式，並安裝 [Tippecanoe](https://github.com/felt/tippecanoe) 2.79 或相容版本。執行 `pnpm gis:refresh` 會向原機關取得水利、行政區、風電與樹木資料、轉換、驗證，最後一次切換 `snapshot.json`；失敗不切換。`pnpm gis:refresh --themes` 只更新風電、樹木與縣市筆數，沿用既有水資源和縣界檔案的內容及下載時間，不需要 Tippecanoe。`pnpm gis:verify` 只檢查現有快照的檔案、雜湊、筆數與清單，不連外下載。

`public/gis-data/snapshots/<id>/` 的 ID 由 17 份檔案 SHA-256 組成；內容變動才產生新目錄。`capturedAt` 是本專案下載／衍生資料產製時間，不是機關測量或發布日期。提交新快照時要連同 `snapshot.json` 一起提交；確認舊版本無人使用後才移除舊目錄。正式快照不加入 `.gitignore`。

這些圖資只供視覺參考，不代替法定河川界、工程測量或即時觀測。已核對來源的 `rightsEvidence` 集中在 `scripts/gis-snapshot.mjs` 資料目錄，更新時寫入清單，不因資料內容更新而丟失。新增或變更來源時須重新核對條款及更新本文件；未提供證據的來源仍為 `unverified`。`pnpm gis:verify-public` 核對快照來源與授權依據是否符合目錄，證據不足或不一致時阻止公開部署。部署 PMTiles 後，以 `pnpm gis:range <站台根網址>` 核對 HTTP Range 讀取。
