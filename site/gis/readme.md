# GIS 圖資

本目錄管理隨專案部署的固定圖資，以及線上圖層的來源說明。現有內容分為 **8 個分類、22 個主題**；來源與使用限制集中於各分類的 `readme.md`，總覽與分類合計 **9 份 README**。

## 主題分類

| 分類 | 來源文件 | 取得方式 |
| --- | --- | --- |
| Basemap layers | [basemap](basemap/readme.md) | OpenFreeMap 線上底圖 |
| 土地使用 Land Use | [land-use](land-use/readme.md) | 官方線上影像與座標查詢 |
| 水資源 Water | [water](water/readme.md) | 本地固定資料 |
| 再生能源 Energy | [energy](energy/readme.md) | 本地固定資料 |
| 環境 Environment | [environment](environment/readme.md) | 本地固定資料 |
| 廢棄物 Waste | [waste](waste/readme.md) | 本地固定資料 |
| 地質 Geology | [geology](geology/readme.md) | 斷層、液化為本地資料；地表地層為線上影像及本地圖例 |
| 行政區 Administrative | [administrative](administrative/readme.md) | 本地界線與共用索引 |

## 目錄與命名

以下列到各圖層主題層級，行尾標註畫面名稱與用途。動態圖層的主題路徑用於命名對應，服務網址集中於分類文件。

```text
public/gis/                           # GIS 固定圖資與動態服務來源說明
├── readme.md                         # 圖資總覽、命名與維護規則
├── manifest.json                     # 共用分類、圖層名稱、資產路徑與版本
├── basemap/                          # Basemap layers
│   ├── readme.md                     # 底圖來源、資料類型與更新方式
│   └── openfreemap/                  # OpenFreeMap；動態取得，無本地圖磚
├── land-use/                         # 土地使用 Land Use
│   ├── readme.md                     # 土地使用服務來源、資料類型與更新方式
│   ├── land-use-survey/              # 國土利用現況調查成果圖；動態影像與座標查詢
│   └── water-land-use/               # 水利用地；動態影像與座標查詢
├── water/                            # 水資源 Water
│   ├── readme.md                     # 水資源來源、資料類型與更新日期
│   ├── basins/                       # 流域 Basin
│   ├── rivers/                       # 河川 River；河道範圍
│   ├── reservoirs/                   # 水庫集水區／壩體
│   ├── detention-basins/             # 滯洪池點位 Detention
│   ├── river-level-stations/         # 河川水位測站位置
│   ├── groundwater-wells/            # 地下水觀測井位置
│   └── flood-potential/              # 淹水潛勢 Flood 650mm/24h；模擬範圍與圖例
├── energy/                           # 再生能源 Energy
│   ├── readme.md                     # 能源來源、資料類型與更新日期
│   ├── offshore-wind-farms/          # 離岸風場 Offshore Wind
│   └── offshore-wind-potential/      # 風電潛力場址 Wind Plan
├── environment/                      # 環境 Environment
│   ├── readme.md                     # 環境來源、資料類型與更新日期
│   ├── protected-trees/              # 受保護樹木 Protected Trees
│   ├── wildlife-habitats/            # 重要棲息環境 Wildlife Habitats
│   └── important-wetlands/           # 重要濕地 Important Wetlands
├── waste/                            # 廢棄物 Waste
│   ├── readme.md                     # 廢棄物設施來源、資料類型與更新日期
│   ├── incinerators/                 # 焚化爐 Incinerator
│   ├── landfills/                    # 衛生掩埋場 Landfill
│   └── coastal-landfills/            # 濱海掩埋場 Coastal
├── geology/                          # 地質 Geology
│   ├── readme.md                     # 地質來源、資料類型與更新日期／方式
│   ├── active-faults/                # 活動斷層 Active Faults
│   ├── soil-liquefaction/            # 土壤液化潛勢 Liquefaction；潛勢範圍與圖例
│   └── surface-geology/              # 地表地層 Geology；動態影像與本地圖例
└── administrative/                   # 行政區 Administrative
    ├── readme.md                     # 行政區來源、資料類型與更新日期
    └── counties/                     # 縣市界 Counties；共用界線與索引
```

- 實體目錄 `public/gis/`，網站 URL `/gis/`。沒有快照雜湊資料夾。
- 本地檔案依分類 → 主題 → 檔案管理；README 只放在 GIS 根目錄及分類資料夾，主題來源集中於分類文件。名稱取自資料意義，點／線／面是顯示方法。
- [manifest.json](manifest.json) 是分類名稱、畫面主題名稱、目錄與 35 份資料資產路徑的共同定義，瀏覽器及產製腳本共用。三份本地圖例 JSON 另放在對應主題。
- 線上服務的來源與取得方式記錄於分類文件；地表地層的本地圖例放在 `geology/surface-geology/`。
- 同主題用副檔名及 `-by-county` 區分用途；不同樹木來源用城市後綴，水庫用 `catchments`／`dams` 區分。
- 資料 ID 是既有程式與圖徵追溯身分，機關代碼不因改名重建；透過 manifest 對應可讀名稱與路徑。
- SHA-256 用於完整性及 `?v=<檔案雜湊>` 快取版本，整批 ID 用於結果追溯，均不作目錄名稱。舊發布內容留在 Git 歷史，不隨網站重複部署。

## 來源與時間

| 取得方式 | 記錄內容 | 更新方式 |
| --- | --- | --- |
| 本地固定資料 | 原機關、原始下載 URL、原始／部署格式、座標、授權、已知官方版本與日期、本專案資料新增／更新日期 | 人工更新；瀏覽網站時不重新下載機關來源 |
| 線上服務 | 服務提供者、URL、協定、顯示與使用限制 | 動態更新，由來源服務維護；瀏覽器依畫面按需取得，不保存本地圖磚副本 |

各分類表格最後一欄為「新增日期／更新方式」：

- 本地資料的日期表示整筆 GIS 資料在本專案新增或更新的日期，由 manifest 的 `importedAt` 保存；本次既有固定資料統一先以今天 **2026-10-09** 建立日期紀錄。這是資料日期，不是 README 編輯日期。
- 動態取得的服務標示「動態更新（按需取得）」，不填新增日期、下載日期或文件整理日期；來源更新由服務提供者管理，本專案沒有定時輪詢或更新時間偵測。
- 地表地層的線上影像與本地圖例分開列示：影像標示動態更新，本地圖例記錄資料日期。
- 官方調查、公告、統計與圖資年份依原始資料另記，缺少者不猜測。後續實際更新本地資料時，同步維護 manifest 日期與分類文件；只修改文件不改資料日期。

目前沒有接入即時監測資料。線上取得底圖或官方影像，表示按需取得服務內容；不等於即時水位、雨量、營運狀態或其他即時觀測。

## 檔案角色

| 檔案 | 用途 |
| --- | --- |
| .geojson | 小型完整向量，供顯示、查看與共用條件 |
| .geojson.gzip | 大型完整向量，瀏覽器以 DecompressionStream 解壓縮；避免 Vite 對 .gz 自動設定 Content-Encoding |
| .pmtiles | 地圖顯示，需要 HTTP Range，不能取代完整分析幾何 |
| *-by-county.pmtiles | 共用縣市裁切顯示，保留原圖徵 ID |
| 主題圖例 .json | 官方分級、標籤與顏色的共用設定 |
| administrative/counties/index.json | 共用縣碼、名稱、範圍與各資料筆數 |

## 維護流程與模組架構

本地資料發布流程：

```text
原機關 → 主題轉接器 → 共用幾何／行政區產製
                         ↓
               驗證後的 GIS 目錄與 manifest
                         ↓
            GisSnapshot → Catalog → Renderer／Identify
```

線上服務由 Catalog 宣告來源與顯示規則，瀏覽器按需讀取原服務；本地圖例與服務來源說明依所屬分類管理。

- `scripts/gis-snapshot.mjs`：保留來源／授權契約，路徑讀取 manifest；核對雜湊、身分、能力、筆數及根目錄／分類 README。
- `scripts/refresh-gis-assets.mjs`：人工更新或本地重建；沿用同一轉換／裁切／產製流程。先寫暫存目錄，全部成功後置換 `gis/`，安裝失敗還原原目錄。
- `src/gis/data/GisSnapshot.ts`：共用名稱與資料路徑，建立含檔案雜湊的載入 URL。
- MapLayerStyle／MapLayerRenderer：沿用共用點、線、面與影像的樣式、開關與生命週期，不新增顯示演算法。

| 人工指令 | 用途 |
| --- | --- |
| `pnpm gis:refresh` | 從原機關更新全部本地資料 |
| `pnpm gis:refresh --themes` | 更新風電、樹木、廢棄物及淹水潛勢 |
| `pnpm gis:refresh --geology` | 更新活動斷層與土壤液化本地資料 |
| `pnpm gis:refresh --ecology` | 更新棲息環境與濕地 |
| `pnpm gis:refresh --rebuild` | 從本地完整資料重建衍生資產，保留資料新增／更新日期 |
| `pnpm gis:verify` | 核對資料完整性與文件位置 |
| `pnpm gis:verify-public` | 公開部署前核對來源與授權紀錄 |
| `pnpm gis:range <網站網址>` | 核對實際部署主機的 PMTiles HTTP Range |

新增主題先在 manifest 登記可讀名稱、目錄與資產，再補原機關來源契約及所屬分類 README；沿用現有處理與顯示 API。資料說明集中於分類文件，不另建主題 README。
