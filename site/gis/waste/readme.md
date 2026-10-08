# 廢棄物 Waste

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關：環境部環境管理署。
- 取得方式：本地固定資料，隨專案部署；人工執行更新才重新取得，沒有排程更新或更新時間偵測。
- 畫面名稱、資料身分、檔案路徑及完整下載參數由 [manifest.json](../manifest.json) 統一定義。

## 主題與檔案

| 畫面名稱 | 資料檔案 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 焚化爐 Incinerator | [incinerators.geojson](incinerators/incinerators.geojson) | GeoJSON／EPSG:4326；煙囪位置點，共 24 筆 | 2026-10-09 |
| 衛生掩埋場 Landfill | [landfills.geojson](landfills/landfills.geojson) | GeoJSON／EPSG:4326；設施位置點，共 107 筆 | 2026-10-09 |
| 濱海掩埋場 Coastal | [coastal-landfills.geojson](coastal-landfills/coastal-landfills.geojson) | GeoJSON／EPSG:4326；設施位置點，共 29 筆 | 2026-10-09 |

## 焚化爐 Incinerator

- 主題目錄：`incinerators/`；資料 ID：`moenv-incinerators`。
- 原始格式：JSON API（`GISEPA_P_11`）。
- 原始來源：[環境部資料 API](https://data.moenv.gov.tw/api/v2/gisepa_p_11)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/8341)；核對紀錄保存在 manifest。

24 筆垃圾焚化廠煙囪位置，保留測站編號、廠名、地址及電話。非廠區界線、排放或即時營運資料；沿用紅色共用點樣式。原欄位 `coor_lat`／`coor_lon` 經緯順序依既有轉換核對。

## 衛生掩埋場 Landfill

- 主題目錄：`landfills/`；資料 ID：`moenv-landfills`。
- 原始格式：JSON API（`FAC_P_02`），TWD97 經緯度。
- 原始來源：[環境部資料 API](https://data.moenv.gov.tw/api/v2/fac_p_02)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/9370)；核對紀錄保存在 manifest。

107 筆營運中公有掩埋場容量統計，含簡易垃圾場；非完整歷史或民營名冊。保留原統計日期，設計／剩餘容量單位為 m³，以原統計日期為準。沿用橙色共用點樣式。

## 濱海掩埋場 Coastal

- 主題目錄：`coastal-landfills/`；資料 ID：`moenv-coastal-landfills`。
- 原始格式：JSON API（`STAT_P_113`）。
- 原始來源：[環境部資料 API](https://data.moenv.gov.tw/api/v2/stat_p_113)。
- 授權依據：[環境部資料說明](https://data.moenv.gov.tw/dataset/detail/STAT_P_113)；核對紀錄保存在 manifest。

29 筆濱海公有掩埋場，含停用與復育場址；保留現況、濱臨海洋與官方距海距離（m），不自行重算。沿用青色共用點樣式，點位不是場區範圍。

## 維護方式

原始版本與日期維持既有紀錄，來源未提供的日期不推算。人工更新沿用 `pnpm gis:refresh --themes`；`--rebuild` 從本地完整資料重建衍生資產，不重新下載。主題資料夾只管理資料檔案，說明集中維護於本文件。
