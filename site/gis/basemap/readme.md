# Basemap layers

本分類的底圖來源、資料類型與使用限制統一記錄於本文件。

- 服務提供者：OpenFreeMap；底圖標示保留 OpenMapTiles 與 OpenStreetMap 資料來源。
- 取得方式：依地圖位置與縮放向服務按需載入；內容更新由服務提供者管理，不表示即時監測。
- 分類及主題名稱由 [manifest.json](../manifest.json) 統一定義。

## 主題與服務

| 畫面名稱／資料用途 | 服務來源 | 資料類型 | 新增日期／更新方式 |
| --- | --- | --- | --- |
| OpenFreeMap | [OpenFreeMap](https://openfreemap.org/) | MapLibre 樣式 JSON、向量圖磚、字形與圖示；線上服務 | 動態更新（按需取得） |

## OpenFreeMap

- 主題 ID：`openfreemap`。
- 樣式入口：`https://tiles.openfreemap.org/styles/{style}`。
- 目前使用：`dark`、`liberty`、`bright`、`positron`、`fiord`；樣式設定由 `src/gis/layers/MapLayerCatalog.ts` 管理。

Water、Parks & land cover、Buildings、Transport、Boundaries、Place labels 是同一份底圖樣式中的顯示控制項，共用 OpenFreeMap 來源。

## 維護與使用方式

保留 OpenFreeMap、OpenMapTiles 與 OpenStreetMap attribution，資料授權依服務提供者原始說明。本分類沒有本地圖磚副本、圖磚下載日期、更新時間自動偵測或定時輪詢。服務設定需調整時，由開發者人工核對並提交變更；說明集中維護於本文件。
