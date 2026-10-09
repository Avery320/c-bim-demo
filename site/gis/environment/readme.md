# 環境 Environment

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關：受保護樹木為臺北市政府文化局、新北市政府農業局、高雄市政府農業局；野生動物保護區、重要棲息環境、國土生態綠網與天然植群為農業部林業及自然保育署；重要濕地為內政部國家公園署。
- 取得方式：本地固定資料，隨專案部署；人工執行更新才重新取得，沒有即時監測、排程更新或更新時間偵測。
- 畫面名稱、資料身分、檔案路徑及完整下載參數由 [manifest.json](../manifest.json) 統一定義；授權核對紀錄與排除原因也保存在其中。

## 主題與檔案

| 畫面名稱 | 完整資料檔案 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 受保護樹木 Protected Trees | [protected-trees-taipei.geojson](protected-trees/protected-trees-taipei.geojson)、[protected-trees-new-taipei.geojson](protected-trees/protected-trees-new-taipei.geojson)、[protected-trees-kaohsiung.geojson](protected-trees/protected-trees-kaohsiung.geojson) | GeoJSON／EPSG:4326；位置點，臺北 3,874／新北 883／高雄 717 筆 | 2026-10-09 |
| 野生動物保護區 Wildlife Protected Areas | [wildlife-protected-areas.geojson.gzip](wildlife-protected-areas/wildlife-protected-areas.geojson.gzip) | GeoJSON + gzip／EPSG:4326；公告範圍面，32 筆含分區、16 處保護區 | 2026-10-09 |
| 重要棲息環境 Wildlife Habitats | [wildlife-habitats.geojson.gzip](wildlife-habitats/wildlife-habitats.geojson.gzip) | GeoJSON + gzip／EPSG:4326；公告範圍面，共 33 筆 | 2026-10-09 |
| 重要濕地 Important Wetlands | [important-wetlands.geojson.gzip](important-wetlands/important-wetlands.geojson.gzip) | GeoJSON + gzip／EPSG:4326；公告範圍面，共 89 筆 | 2026-10-09 |
| 國土生態綠網關注區域 Ecological Network Focus Areas | [ecological-network-focus-areas.geojson.gzip](ecological-network-focus-areas/ecological-network-focus-areas.geojson.gzip) | GeoJSON + gzip／EPSG:4326；保育規劃範圍面，共 44 筆 | 2026-10-09 |
| 國土生態綠網區域保育軸帶 Ecological Conservation Corridors | [ecological-conservation-corridors.geojson.gzip](ecological-conservation-corridors/ecological-conservation-corridors.geojson.gzip) | GeoJSON + gzip／EPSG:4326；保育規劃範圍面，共 45 筆 | 2026-10-09 |
| 天然植群圖 Natural Vegetation | [natural-vegetation.geojson.gzip](natural-vegetation/natural-vegetation.geojson.gzip) | GeoJSON + gzip／EPSG:4326；植群範圍面，原始 49,953 筆 | 2026-10-09 |

## 受保護樹木 Protected Trees

- 主題目錄：`protected-trees/`；畫面主題 ID：`tw-protected-trees`。
- 原始格式：臺北、新北為 CSV；高雄為 JSON API。

| 發布機關／資料 ID | 原始來源 | 授權依據 |
| --- | --- | --- |
| 臺北市政府文化局／`taipei-protected-trees` | [臺北市資料下載](https://data.taipei/api/frontstage/tpeod/dataset/resource.download?rid=a7c2db0d-8b6e-42b2-bdcc-ae69a6797e1d) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/121274) |
| 新北市政府農業局／`new-taipei-protected-trees` | [新北市資料下載](https://data.ntpc.gov.tw/api/datasets/dfd0dd0f-564b-461f-88fd-4493a87387b9/csv/file) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/125615) |
| 高雄市政府農業局／`kaohsiung-protected-trees` | [高雄市資料 API](https://openapi.kcg.gov.tw/Api/Service/Get/914625f5-7800-4502-9573-1e2331d16bc5) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/163742) |

臺北 3,874 筆、新北 883 筆、高雄 717 筆共用一個圖層，各自保留來源與官方編號。新北只納入公告列管，排除解列。非全國清單，點位不是完整保護範圍；胸徑／胸圍單位為 m、推估樹齡單位為年，缺值不當作零。

## 野生動物保護區 Wildlife Protected Areas

| 項目 | 說明 |
| --- | --- |
| 主題目錄／資料 ID | `wildlife-protected-areas/`／`fanca-wildlife-protected-areas` |
| 發布機關 | 農業部林業及自然保育署 |
| 原始格式 | ZIP／SHP，TWD97 TM2（121 度）、UTF-8 |
| 原始版本 | 陸域野生動物保護區 1141；來源檔案索引日期 2025-09-24 |
| 原始來源 | [農業部原始圖資下載](https://data.moa.gov.tw/OpenData/GetOpenDataFile.aspx?id=162&FileType=DataMore&RID=59756) |
| 授權依據 | [農業部資料說明](https://data.moa.gov.tw/open_detail.aspx?id=162)、[政府資料開放授權條款第 1 版](https://data.gov.tw/dataset/25540) |
| 顯示檔 | [wildlife-protected-areas.pmtiles](wildlife-protected-areas/wildlife-protected-areas.pmtiles) |
| 縣市衍生檔 | [wildlife-protected-areas-by-county.pmtiles](wildlife-protected-areas/wildlife-protected-areas-by-county.pmtiles) |
| 顯示方式 | 環境 Environment → Areas；橙色填面預設 40%，同色邊界 100%、固定線寬；支援顏色、不透明度與名稱控制 |
| 詳情欄位 | 原始編號、名稱、分區、管理機關、設立／最新公告日期、編輯資訊、官方統計面積（ha）、原資料版本 |

- 共 32 筆範圍圖徵、16 處保護區；保留核心區、緩衝區、永續利用區等原始分區，不合併幾何。身分使用官方編號加分區名稱，避免同一保護區各分區互相覆蓋。
- 本版為陸域資料，不含海洋委員會主管資料；保護區、重要棲息環境與重要濕地各自保留來源及圖層控制。
- `Area_ha` 原值可能為分區面積或保護區總面積，不加總、不以幾何估算替換；原值為 0 時記為缺值。原資料「待更新」「面積待修正公告」等編輯註記保留。
- 原 PolygonZ 的 Z 全為 0，移除占位 Z，保留 XY、內洞與多部件；0 筆幾何修復、0 筆排除。

## 重要棲息環境 Wildlife Habitats

- 主題目錄：`wildlife-habitats/`；資料 ID：`fanca-wildlife-habitats`。
- 發布機關：農業部林業及自然保育署。
- 原始格式：ZIP／SHP，TWD97 TM2（121 度）。
- 原始來源：[農業部原始圖資下載](https://data.moa.gov.tw/GetOpenDataFile.aspx?id=156&FileType=DataMore&RID=83443)。
- 授權依據：[農業部資料說明](https://data.moa.gov.tw/open_detail.aspx?id=156)。
- 顯示檔：[wildlife-habitats.pmtiles](wildlife-habitats/wildlife-habitats.pmtiles)；縣市衍生檔：[wildlife-habitats-by-county.pmtiles](wildlife-habitats/wildlife-habitats-by-county.pmtiles)。

陸域野生動物重要棲息環境 1151 版本，共 33 筆，不含海洋委員會主管資料。保留管理機關、設立／最新公告日期、Edition 及官方面積（ha）；非動物觀測點。綠色填面初始不透明度 40%，不表示官方分級。1 筆沿用 GEOS MakeValid 修復、0 筆排除，保留處理註記。原 PolygonZ 的 Z 全為 0，移除占位 Z 後保留完整 XY、內洞與多部件。

## 重要濕地 Important Wetlands

- 主題目錄：`important-wetlands/`；資料 ID：`nps-important-wetlands`。
- 發布機關：內政部國家公園署。
- 原始格式：官方 CSV 索引指向 TGOS ZIP／SHP；原 PRJ 為 WGS84 基準 TM2（121 度）。
- 圖資索引：[官方 CSV 索引](https://opdadm.moi.gov.tw/api/v1/no-auth/resource/api/dataset/155E015A-B3A9-4075-9E5D-3C735283724A/resource/0C1B899E-F493-4F6B-A34F-556C91B86667/download)，只採用「重要濕地範圍圖」所指向的 SHP。
- 原始來源：[TGOS 重要濕地範圍圖](https://www.tgos.tw:443/MDE/VirtualDir_TC/Product/c5894f87-68f4-4f88-ab32-12d58cc9fd35/重要濕地1150109(更新龍鑾潭).zip)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/25659)。
- 顯示檔：[important-wetlands.pmtiles](important-wetlands/important-wetlands.pmtiles)；縣市衍生檔：[important-wetlands-by-county.pmtiles](important-wetlands/important-wetlands-by-county.pmtiles)。

1150109（更新龍鑾潭）範圍圖，共 89 筆含子區域、61 個主名稱，不是 89 處獨立公告濕地。保留國際級、國家級、地方級與暫定地方級、原資料縣市及面積（ha）；未混入保育利用計畫或功能分區，原檔沒有逐筆公告日期。青綠色填面初始不透明度 25%，不表示級別。5 筆沿用 GEOS MakeValid 修復、0 筆排除；編號 19-15 北／南兩筆以編號加子區域名稱識別，均保留。

## 國土生態綠網關注區域 Ecological Network Focus Areas

| 項目 | 說明 |
| --- | --- |
| 主題目錄／資料 ID | `ecological-network-focus-areas/`／`fanca-ecological-focus-areas` |
| 發布機關 | 農業部林業及自然保育署 |
| 原始格式 | ZIP／SHP，TWD97 TM2（121 度）、UTF-8 |
| 原始版本 | 國土綠網關注區域 1092；版本取自官方 SHP 檔名，非本專案新增日期 |
| 原始來源 | [林業保育署關注區域圖資](https://www.forest.gov.tw/file/79024) |
| 授權依據 | [官方圖資下載與使用說明](https://www.forest.gov.tw/0004767)，政府資料開放授權條款第 1 版 |
| 顯示檔 | [ecological-network-focus-areas.pmtiles](ecological-network-focus-areas/ecological-network-focus-areas.pmtiles) |
| 縣市衍生檔 | [ecological-network-focus-areas-by-county.pmtiles](ecological-network-focus-areas/ecological-network-focus-areas-by-county.pmtiles) |
| 顯示方式 | 環境 Environment → Areas；紫色填面預設 40%，同色邊界 100%、固定線寬；支援顏色、不透明度與名稱控制 |
| 詳情欄位 | 原始編號、名稱、綠網分區、涵蓋範圍、關注棲地、關注植物、關注動物、指認目的、官方圖資面積（ha）、原資料版本 |

- 共 44 筆，涵蓋本島 39 處與離島 5 處；每筆沿用官方 `Id`，不合併範圍。
- 屬生態保育規劃圖資；關注動植物為原始規劃清單，不表示逐筆觀測、完整物種分布或法定保護區。9 筆關注植物欄位未提供內容，保留缺值，不推算或補造。
- 原 PolygonZ 的 Z 全為 0，移除占位 Z；保留 XY、內洞與多部件。10 筆沿用 GEOS MakeValid 修復並記錄處理註記，0 筆排除。
- 原資料未提供逐筆調查或公告日期，不以版本或本專案新增日期代替。

## 國土生態綠網區域保育軸帶 Ecological Conservation Corridors

| 項目 | 說明 |
| --- | --- |
| 主題目錄／資料 ID | `ecological-conservation-corridors/`／`fanca-ecological-corridors` |
| 發布機關 | 農業部林業及自然保育署 |
| 原始格式 | ZIP／SHP，TWD97 TM2（121 度）、UTF-8 |
| 原始版本 | 國土生態綠網區域保育軸帶 1141，官方下載名稱為 11401 公開版；2025 年 2 月公告更新 |
| 原始來源 | [林業保育署區域保育軸帶圖資](https://www.forest.gov.tw/file/89332) |
| 授權依據 | [官方圖資下載與使用說明](https://www.forest.gov.tw/0004767)，政府資料開放授權條款第 1 版 |
| 顯示檔 | [ecological-conservation-corridors.pmtiles](ecological-conservation-corridors/ecological-conservation-corridors.pmtiles) |
| 縣市衍生檔 | [ecological-conservation-corridors-by-county.pmtiles](ecological-conservation-corridors/ecological-conservation-corridors-by-county.pmtiles) |
| 顯示方式 | 環境 Environment → Areas；黃色填面預設 40%，同色邊界 100%、固定線寬；支援顏色、不透明度與名稱控制 |
| 詳情欄位 | 原始編號、名稱、軸帶類別、綠網分區、原資料分署、涉及行政區、關注棲地、推動策略、編輯資訊、官方圖資面積（ha）、原資料版本 |

- 共 45 筆，以官方 `no` 識別；原始幾何為 Polygon／MultiPolygon，按面圖層顯示，不轉換為中線。
- 保留丘陵、溪流、平原、海岸與離島五種類別；顏色為可調整的顯示樣式，不代表官方分級。屬保育規劃範圍，不表示法定保護區。
- 各筆 `Edition` 保留原值，可能早於公開版年份；不將整份圖資版本當作逐筆編輯或調查日期。詳情欄位取自 SHP，不推算未提供的物種清單。
- 保留完整 XY、內洞與多部件；4 筆沿用 GEOS MakeValid 修復並記錄處理註記，0 筆排除。

## 天然植群圖 Natural Vegetation

| 項目 | 說明 |
| --- | --- |
| 主題目錄／資料 ID | `natural-vegetation/`／`fanca-natural-vegetation` |
| 發布機關 | 農業部林業及自然保育署；下載入口為農業部資料開放平臺 |
| 原始格式 | 7z／SHP，TWD97 TM2（121 度、EPSG:3826）、Big5；原檔 `ACO0301000011021` |
| 資料年代 | 2003–2009 年調查成果，2013 年修正部分崩塌地；不是目前植被現況，也不以本專案新增日期代替調查年代 |
| 原始來源 | [農業部天然植群原始圖資](https://data.moa.gov.tw/OpenData/GetOpenDataFile.aspx?FileType=SHP&RID=1908&id=153) |
| 資料與授權說明 | [政府資料開放平臺](https://data.gov.tw/dataset/9930)，政府資料開放授權條款第 1 版 |
| 顯示檔／縣市衍生檔 | [natural-vegetation.pmtiles](natural-vegetation/natural-vegetation.pmtiles)／[natural-vegetation-by-county.pmtiles](natural-vegetation/natural-vegetation-by-county.pmtiles) |
| 分類配色 | [vegetation-classes.json](natural-vegetation/vegetation-classes.json)；依[林業保育署官方圖層的群系綱配色](https://gis.forest.gov.tw/arcgis/rest/services/CO/%E5%8F%B0%E7%81%A3%E7%8F%BE%E7%94%9F%E5%A4%A9%E7%84%B6%E6%A4%8D%E7%BE%A4%E5%9C%96/MapServer/0?f=pjson)保存六類顏色，未自行推算或按潛勢分級 |
| 顯示方式 | 環境 Environment → Areas；分類色固定、填色預設 40%，同分類邊界 100%、固定線寬；支援填色不透明度與植群名稱 |
| 座標精度 | 完整資料的 WGS84 座標取九位小數（約 0.1 毫米），再執行共用幾何驗證與修復；精度不代表原調查的定位準確度 |
| 詳情欄位 | 原始植群名稱、植群編碼、群系綱、原始群系綱／亞綱、原始海拔帶及數值代碼、原圖資面積（ha）、原檔列號、資料年代 |
| 人工更新 | `pnpm gis:refresh --vegetation`；`--environment` 同時更新本分類全部資料 |

- 六類：森林、灌叢、草本植群、特殊棲地植生、人工植生、其他。保留來源中的人工植生、水域、建地、裸露地等範圍，空白處不推算植群。
- 名稱沿用 `FORMATION`，植群編碼沿用「編碼」；這些分類欄位不是唯一圖徵 ID。圖徵以固定原檔列號識別，不合併同類範圍。
- `CLASS`、`SUBCLASS` 中部分名稱受原 DBF 欄寬截短，保留原文字；群系綱完整名稱由官方六類對應，細分類不自行補造。
- `AREA_HA` 保留官方值，6 筆零值記為未提供；部分原值與單筆幾何面積不一致，不加總為面積或以本專案估算替換。
- 完整保留 49,953 筆；163 筆沿用共用 GEOS MakeValid 修復並記錄處理註記，0 筆排除。圖磚按縮放層級簡化，完整 GeoJSON 保留全部圖徵與修復後幾何。

## 共用檔案與維護方式

- PMTiles 採 EPSG:3857，供地圖顯示；完整 GeoJSON 採 EPSG:4326，供顯示、查詢與分析。`.geojson.gzip` 為壓縮的完整資料，未從圖磚反推幾何。
- 縣市衍生檔使用[國土測繪中心官方縣市界](../administrative/readme.md)產製，保留原始邏輯圖徵身分；各資產來源記錄於 manifest。
- 原始版本與日期維持既有紀錄，來源未提供的日期不推算；排除與幾何修復紀錄保留於資料及 manifest。
- 本分類人工更新使用 `pnpm gis:refresh --environment`；只更新保護區、棲息環境、濕地與國土生態綠網使用 `pnpm gis:refresh --ecology`，只更新天然植群使用 `pnpm gis:refresh --vegetation`。`--rebuild` 從本地完整資料重建衍生資產，不重新下載。
- 主題資料夾只管理資料檔案，說明集中維護於本文件。
