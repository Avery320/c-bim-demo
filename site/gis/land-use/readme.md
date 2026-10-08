# 土地使用 Land Use

本分類的服務來源、資料類型與使用限制統一記錄於本文件。

- 服務提供者：內政部國土測繪中心。
- 取得方式：依地圖位置與縮放向官方服務按需載入；內容更新由服務提供者管理，不表示即時監測。
- 畫面名稱與主題目錄由 [manifest.json](../manifest.json) 統一定義。

## 主題與服務

| 畫面名稱 | 服務來源 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| 國土利用現況調查成果圖 | [國土測繪圖資服務](https://maps.nlsc.gov.tw/)；WMTS 圖層 `LUIMAP` | PNG 圖磚／EPSG:3857；線上影像 | 動態更新（按需取得） |
| 水利用地 | [國土測繪 WMS](https://wms.nlsc.gov.tw/wms)；圖層 `LUIMAP04` | PNG 影像／EPSG:3857；線上影像 | 動態更新（按需取得） |

## 國土利用現況調查成果圖

- 主題目錄：`land-use-survey/`；主題 ID：`nlsc-land-use`。
- WMTS 圖磚：`https://wmts.nlsc.gov.tw/wmts/LUIMAP/default/GoogleMapsCompatible/{z}/{y}/{x}`。
- 點選查詢：`https://api.nlsc.gov.tw/other/LandUsePointQuery/{longitude}/{latitude}/4326`。

保留官方影像原色；服務在 z7 以下回傳空白影像。點選另使用官方座標查詢，圖磚與查詢版本可能不同。此資料不是地籍或法定使用分區，也沒有本地完整分析向量。

## 水利用地

- 主題目錄：`water-land-use/`；主題 ID：`nlsc-water-land-use`。
- WMS 入口：`https://wms.nlsc.gov.tw/wms`。
- GetMap 參數：`VERSION=1.1.1`、`LAYERS=LUIMAP04`、`SRS=EPSG:3857`、`FORMAT=image/png`、`TRANSPARENT=TRUE`；BBOX 依畫面圖磚範圍。

顯示國土利用現況中的水利利用分類，保留官方影像原色；服務在 z7 以下回傳空白影像。點選沿用官方 LandUsePointQuery 座標查詢與水利分類，不把影像當成可分析的完整面幾何。

## 維護與使用方式

保留官方 attribution，使用限制依原服務說明。本分類沒有本地圖磚副本、圖磚下載日期、更新時間自動偵測或定時輪詢。官方更新以服務提供者說明為準；服務設定需調整時，由開發者人工核對並提交變更。說明集中維護於本文件，不另建主題 README。
