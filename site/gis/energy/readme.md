# 再生能源 Energy

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關：經濟部能源署。
- 取得方式：本地固定資料，隨專案部署；人工執行更新才重新取得，沒有排程更新或更新時間偵測。
- 畫面名稱、資料身分及檔案路徑由 [manifest.json](../manifest.json) 統一定義。

## 主題與檔案

| 畫面名稱 | 資料檔案 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 離岸風場 Offshore Wind | [offshore-wind-farms.geojson](offshore-wind-farms/offshore-wind-farms.geojson) | GeoJSON／EPSG:4326；場址面，共 22 筆 | 2026-10-09 |
| 風電潛力場址 Wind Plan | [offshore-wind-potential.geojson](offshore-wind-potential/offshore-wind-potential.geojson) | GeoJSON／EPSG:4326；潛力場址面，共 33 筆 | 2026-10-09 |

## 離岸風場 Offshore Wind

- 主題目錄：`offshore-wind-farms/`；資料 ID：`moeaea-offshore-wind-farms`。
- 原始格式：ZIP／XLSX，TWD97 TM2 角點表。
- 原始資料：[能源署下載附件](https://www.moeaea.gov.tw/ECW/populace/Law/wHandLawsList_File.ashx?kind=6&id=5476)。
- 附件入口：[能源署法規及公告附件](https://www.moeaea.gov.tw/ECW/populace/Law/LawsList.aspx?kind=6&menu_id=3302)。
- 授權依據：[國家海洋資料庫資料說明](https://nodass.namr.gov.tw/dataInfo?metadataid=9)；核對紀錄保存在 manifest。

22 筆可顯示場址代表有效設置同意／籌設許可，非商轉清單。依原始角點順序閉合；無法確認區塊連接方式者不補造邊界，排除原因保存於 manifest。海上範圍不套用預設陸地縣界。

## 風電潛力場址 Wind Plan

- 主題目錄：`offshore-wind-potential/`；資料 ID：`moeaea-offshore-wind-potential`。
- 原始格式：TSV（Tab 分隔文字表），TWD97 TM2 角點。
- 原始資料：[能源署開放資料下載](https://www.moeaea.gov.tw/ECW/populace/opendata/wHandOpenData_File.ashx?set_id=145)。
- 授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/36681)；核對紀錄保存在 manifest。

33 筆可顯示潛力場址，非核准範圍或現況風場。官方面積單位為 km²；原資料參考縣市不等於海上行政區歸屬。無法確認的區塊記錄於 manifest，不猜測邊界。

## 維護方式

原始版本與日期維持既有紀錄，來源未提供的日期不推算。人工更新沿用 `pnpm gis:refresh --themes`；`--rebuild` 從本地完整資料重建衍生資產，不重新下載。主題資料夾只管理資料檔案，說明集中維護於本文件。
