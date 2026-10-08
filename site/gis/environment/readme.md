# 環境 Environment

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關：受保護樹木為臺北市政府文化局、新北市政府農業局、高雄市政府農業局；重要棲息環境為農業部林業及自然保育署；重要濕地為內政部國家公園署。
- 取得方式：本地固定資料，隨專案部署；人工執行更新才重新取得，沒有即時監測、排程更新或更新時間偵測。
- 畫面名稱、資料身分、檔案路徑及完整下載參數由 [manifest.json](../manifest.json) 統一定義；授權核對紀錄與排除原因也保存在其中。

## 主題與檔案

| 畫面名稱 | 完整資料檔案 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 受保護樹木 Protected Trees | [protected-trees-taipei.geojson](protected-trees/protected-trees-taipei.geojson)、[protected-trees-new-taipei.geojson](protected-trees/protected-trees-new-taipei.geojson)、[protected-trees-kaohsiung.geojson](protected-trees/protected-trees-kaohsiung.geojson) | GeoJSON／EPSG:4326；位置點，臺北 3,874／新北 883／高雄 717 筆 | 2026-10-09 |
| 重要棲息環境 Wildlife Habitats | [wildlife-habitats.geojson.gzip](wildlife-habitats/wildlife-habitats.geojson.gzip) | GeoJSON + gzip／EPSG:4326；公告範圍面，共 33 筆 | 2026-10-09 |
| 重要濕地 Important Wetlands | [important-wetlands.geojson.gzip](important-wetlands/important-wetlands.geojson.gzip) | GeoJSON + gzip／EPSG:4326；公告範圍面，共 89 筆 | 2026-10-09 |

## 受保護樹木 Protected Trees

- 主題目錄：`protected-trees/`；畫面主題 ID：`tw-protected-trees`。
- 原始格式：臺北、新北為 CSV；高雄為 JSON API。

| 發布機關／資料 ID | 原始來源 | 授權依據 |
| --- | --- | --- |
| 臺北市政府文化局／`taipei-protected-trees` | [臺北市資料下載](https://data.taipei/api/frontstage/tpeod/dataset/resource.download?rid=a7c2db0d-8b6e-42b2-bdcc-ae69a6797e1d) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/121274) |
| 新北市政府農業局／`new-taipei-protected-trees` | [新北市資料下載](https://data.ntpc.gov.tw/api/datasets/dfd0dd0f-564b-461f-88fd-4493a87387b9/csv/file) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/125615) |
| 高雄市政府農業局／`kaohsiung-protected-trees` | [高雄市資料 API](https://openapi.kcg.gov.tw/Api/Service/Get/914625f5-7800-4502-9573-1e2331d16bc5) | [政府資料開放平臺資料說明](https://data.gov.tw/dataset/163742) |

臺北 3,874 筆、新北 883 筆、高雄 717 筆共用一個圖層，各自保留來源與官方編號。新北只納入公告列管，排除解列。非全國清單，點位不是完整保護範圍；胸徑／胸圍單位為 m、推估樹齡單位為年，缺值不當作零。

## 重要棲息環境 Wildlife Habitats

- 主題目錄：`wildlife-habitats/`；資料 ID：`fanca-wildlife-habitats`。
- 發布機關：農業部林業及自然保育署。
- 原始格式：ZIP／SHP，TWD97 TM2（121 度）。
- 原始來源：[農業部原始圖資下載](https://data.moa.gov.tw/GetOpenDataFile.aspx?id=156&FileType=DataMore&RID=83443)。
- 授權依據：[農業部資料說明](https://data.moa.gov.tw/open_detail.aspx?id=156)。
- 顯示檔：[wildlife-habitats.pmtiles](wildlife-habitats/wildlife-habitats.pmtiles)；縣市衍生檔：[wildlife-habitats-by-county.pmtiles](wildlife-habitats/wildlife-habitats-by-county.pmtiles)。

陸域野生動物重要棲息環境 1151 版本，共 33 筆，不含海洋委員會主管資料。保留管理機關、設立／最新公告日期、Edition 及官方面積（ha）；非動物觀測點。綠色填面初始不透明度 25%，不表示官方分級。1 筆沿用 GEOS MakeValid 修復、0 筆排除，保留處理註記。原 PolygonZ 的 Z 全為 0，移除占位 Z 後保留完整 XY、內洞與多部件。

## 重要濕地 Important Wetlands

- 主題目錄：`important-wetlands/`；資料 ID：`nps-important-wetlands`。
- 發布機關：內政部國家公園署。
- 原始格式：官方 CSV 索引指向 TGOS ZIP／SHP；原 PRJ 為 WGS84 基準 TM2（121 度）。
- 圖資索引：[官方 CSV 索引](https://opdadm.moi.gov.tw/api/v1/no-auth/resource/api/dataset/155E015A-B3A9-4075-9E5D-3C735283724A/resource/0C1B899E-F493-4F6B-A34F-556C91B86667/download)，只採用「重要濕地範圍圖」所指向的 SHP。
- 原始來源：[TGOS 重要濕地範圍圖](https://www.tgos.tw:443/MDE/VirtualDir_TC/Product/c5894f87-68f4-4f88-ab32-12d58cc9fd35/重要濕地1150109(更新龍鑾潭).zip)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/25659)。
- 顯示檔：[important-wetlands.pmtiles](important-wetlands/important-wetlands.pmtiles)；縣市衍生檔：[important-wetlands-by-county.pmtiles](important-wetlands/important-wetlands-by-county.pmtiles)。

1150109（更新龍鑾潭）範圍圖，共 89 筆含子區域、61 個主名稱，不是 89 處獨立公告濕地。保留國際級、國家級、地方級與暫定地方級、原資料縣市及面積（ha）；未混入保育利用計畫或功能分區，原檔沒有逐筆公告日期。青綠色填面初始不透明度 25%，不表示級別。5 筆沿用 GEOS MakeValid 修復、0 筆排除；編號 19-15 北／南兩筆以編號加子區域名稱識別，均保留。

## 共用檔案與維護方式

- PMTiles 採 EPSG:3857，供地圖顯示；完整 GeoJSON 採 EPSG:4326，供顯示、查詢與分析。`.geojson.gzip` 為壓縮的完整資料，未從圖磚反推幾何。
- 縣市衍生檔使用[國土測繪中心官方縣市界](../administrative/readme.md)產製，保留原始邏輯圖徵身分；各資產來源記錄於 manifest。
- 原始版本與日期維持既有紀錄，來源未提供的日期不推算；排除與幾何修復紀錄保留於資料及 manifest。
- 受保護樹木人工更新使用 `pnpm gis:refresh --themes`；棲息環境與濕地使用 `pnpm gis:refresh --ecology`。`--rebuild` 從本地完整資料重建衍生資產，不重新下載。
- 主題資料夾只管理資料檔案，說明集中維護於本文件。
