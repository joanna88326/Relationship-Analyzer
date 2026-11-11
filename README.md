# 好友關係分析器（Relationship Analyzer）

這是一個用來練習**迭代式物件導向分析與設計**的專案，依 `srs.md` 的需求，完成 OOA 與 OOD 的設計產出。

本專案**不實作程式碼**，只產出設計文件與圖。

## 練習目標

練習迭代式物件導向設計的完整流程：

```text
SRS
 ↓
OOA Domain Model
 ↓
Forces & Problem
 ↓
Pattern 辨識
 ↓
初步精煉類別圖
 ↓
套用 Pattern
 ↓
Sequence Diagram 梳理類別互動
 ↓
最終設計
```

## 需求範圍

| 範圍 | 內容 | 是否處理 |
| --- | --- | --- |
| 基本需求 | 解析格式一腳本，查詢兩人的共同好友；整合課程提供的 `SuperRelationshipAnalyzer`（格式二） | 處理 |
| 進階需求 | 連結查詢（`hasConnection`）與第三方 Graph 套件整合 | **不處理** |

進階需求不處理，是因為本專案的重點在練習設計流程，基本需求已足以涵蓋 Forces 辨識、Pattern 選擇與套用。SRS 中與進階需求相關的內容（FR-007 到 FR-010、第 5.3 與 5.4 節）不納入本專案的分析。

## 設計產出

| 產出 | 說明 |
| --- | --- |
| OOA 類別圖 | 領域 Class、Operation 與 Note，Note 說明問題與類別職責 |
| `design-analysis.md` | 設計分析：Forces、Problem、Design Pattern 與選擇理由 |
| OOD 類別圖 | 初步精煉後，套用 Pattern 的最終類別圖 |
| `sequence-diagram.puml` | 循序圖，驗證設計能完成 Client 的完整流程 |

圖皆以 PlantUML 撰寫，`.puml` 為編輯來源。

## 設計結果摘要

- **Problem：** 如何利用 `SuperRelationshipAnalyzer` 的能力去實作 `Client` 所定義的 `RelationshipAnalyzer` 介面，且不修改 `Client`、`RelationshipAnalyzer` 與 `SuperRelationshipAnalyzer`。
- **Design Pattern：** Adapter Pattern。
  - Target：`RelationshipAnalyzer`
  - Adaptee：`SuperRelationshipAnalyzer`
  - Adapter：`SuperRelationshipAnalyzerAdapter`，實作 `RelationshipAnalyzer`，並以 Composition 持有 `SuperRelationshipAnalyzer`
- **Adapter 補足的差異：** 操作名稱對應、格式一轉格式二、單一對象判斷轉成共同好友列表。

## 文件

- `srs.md`：軟體需求規格書，為本專案的唯讀輸入。
- `design-analysis.md`：設計分析，詳細說明 Forces 與 Pattern 選擇理由。

## 不在範圍內

- 程式碼實作與單元測試。
- 進階需求（連結查詢、第三方 Graph 套件整合）。
- 格式轉換重複行的處理，以及查無此人時的行為。SRS 沒有規定，留待未來實作時確認。
