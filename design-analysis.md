# 好友關係分析器 - 設計分析

## Problems

### 整合不相容的好友關係分析器

**Force-Client 依賴自身介面：**

`Client` 依賴根據自身意圖所定義的 `RelationshipAnalyzer` 介面，不希望被強迫依賴 `SuperRelationshipAnalyzer` 的 `init`、`isMutualFriend` 等具體操作。

**來源：** `Client` 與 `RelationshipAnalyzer` 之間的 Association「analyzer」

**Force-相容性：**

實現 Client 意圖的 `SuperRelationshipAnalyzer` 與 Client 定義的既有 `RelationshipAnalyzer` 介面不相容。且 `SuperRelationshipAnalyzer` 不允許修改。

**來源：** `SuperRelationshipAnalyzer` 類別

**Force-重用與不侵入 — 既有分析能力重用：**

必須重用 `SuperRelationshipAnalyzer` 的分析能力來完成共同好友查詢，同時不修改 `Client` 與 `RelationshipAnalyzer` 的既有定義與程式碼。

**來源：** `SuperRelationshipAnalyzer` 的 Note「設計需求：整合 SuperRelationshipAnalyzer」

**Problem：**

如何利用 `SuperRelationshipAnalyzer` 的能力去實作 `Client` 所定義的 `RelationshipAnalyzer` 介面，且不影響 `Client`、`RelationshipAnalyzer` 與 `SuperRelationshipAnalyzer` 的既有程式碼？

**Design Pattern：** Adapter Pattern（轉接器模式）

**選擇理由：**

引入 Target、Adaptee 與 Adapter 三種角色：

- **Target：** `RelationshipAnalyzer`，也就是 `Client` 已依賴的介面。
- **Adaptee：** `SuperRelationshipAnalyzer`。
- **Adapter：** 新增一個實作 `RelationshipAnalyzer` 的類別，內部持有 `SuperRelationshipAnalyzer`。

各 Force 的解決者與機制如下：

- **Force-Client 依賴自身介面：** 由 `RelationshipAnalyzer`（Target）與 Adapter 共同解決。`Client` 仍只依賴 `RelationshipAnalyzer` 的 `parse` 與 `getMutualFriends`，`init`、`isMutualFriend` 與格式二被隔離在 Adapter 之後，`Client` 不需要知道 `SuperRelationshipAnalyzer` 的存在。
- **Force-相容性：** 由 Adapter 解決，它負責補足兩個介面的三項差異：
  - 操作對應：`parse` 對應 `init`。
  - 格式轉換：`parse` 將格式一腳本轉成格式二，再交給 `init`。
  - 查詢形式：`getMutualFriends` 取得 `name1` 或 `name2` 其中一人的好友清單（由 `friendsMap` 提供），逐一以 `isMutualFriend` 判斷，收集成立者作為共同好友列表。
- **Force-重用與不侵入：** 由 Adapter 與 `SuperRelationshipAnalyzer` 之間的 Composition 與委派解決。實際判斷仍由 `SuperRelationshipAnalyzer` 完成，轉換邏輯都在新增的 Adapter 內，因此 `Client`、`RelationshipAnalyzer` 與 `SuperRelationshipAnalyzer` 都不需修改。

