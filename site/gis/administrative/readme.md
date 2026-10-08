# 行政區 Administrative

本分類的資料來源、檔案對應與使用限制統一記錄於本文件。

- 發布機關：內政部國土測繪中心；透過 TGOS 取得原始圖資。
- 取得方式：本地固定資料，隨專案部署；人工執行更新才重新取得，沒有排程更新或更新時間偵測。
- 名稱、資料身分、檔案路徑與授權核對紀錄由 [manifest.json](../manifest.json) 統一定義。

## 主題與檔案

| 資料用途 | 資料檔案 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 縣市界 Counties | [counties.geojson](counties/counties.geojson) | GeoJSON／EPSG:4326；縣市範圍面，共 22 筆 | 2026-10-09 |
| 縣市索引 | [index.json](counties/index.json) | JSON；縣碼、名稱、EPSG:4326 範圍座標及各圖資筆數，共 22 筆 | 2026-10-09 |

## 縣市界 Counties

- 主題目錄：`counties/`；界線資料 ID：`tw-admin-counties`；索引資料 ID：`tw-admin-index`。
- 原始格式：ZIP／SHP（直轄市、縣市界線 1140318）。
- 原始來源：[TGOS 原始界線下載](https://www.tgos.tw/tgos/VirtualDir/Product/1cd4f4c9-6b01-4cf9-bf6c-23a73aa17d24/%E7%9B%B4%E8%BD%84%E5%B8%82%E3%80%81%E7%B8%A3%28%E5%B8%82%29%E7%95%8C%E7%B7%9A1140318.zip)。
- 界線授權依據：[政府資料開放平臺資料說明](https://data.gov.tw/dataset/7442)。

22 個縣市作為搜尋、區域條件與圖資裁切的共用支援資料，沒有獨立圖層開關。完整界線保留原始精度；`index.json` 由縣市界及各圖資筆數產製，相關來源與授權分別記錄於各分類文件及 manifest。索引中的筆數屬目前本地資料，不是即時統計。

## 維護方式

原始版本與日期維持既有紀錄，來源未提供的日期不推算。人工更新使用 `pnpm gis:refresh`；`--rebuild` 從本地完整資料重建索引及衍生資產，不重新下載。縣市衍生圖磚放在各自主題資料夾；本分類說明集中維護於本文件。
