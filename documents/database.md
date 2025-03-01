# RDB (PostgreSQL)

Table 同士の親と子の関係は以下の通り

| 親          | 子         |
| ----------- | ---------- |
| Lable Table | Item Table |

```mermaid
erDiagram
    Trash |o--|{ Label : "VisibleId<->VisibleId"
    Item |o--|| Label : "VisibleId<->VisibleId"
    Item {
        i32 Id PK "物品ID autoincrement"
        String VisibleId FK "Label tableのIdとリレーションを張っている"
        String Name "物品名"
        String ProductNumber "空の文字列を許容 型番"
        String Description "空の文字列を許容 物品の説明"
        Option_dayetime PurchaseYear "購入年度"
        Option_i32 PurchasePrice "購入金額"
        Option_i32 Durability "耐用年数"
        boolean IsDepreciation "減価償却対象かどうか"
        Json Connector "端子 e.g. ['USB Type-A', 'HDMI'] (可変な配列)"
        boolean IsRent "貸出中かどうか"
        String Color "空の文字列を許容 e.g. 'Red^Orange^Brown'"
        datetime CreatedAt "作成日時"
        datetime UpdatedAt "更新日時"
    }
    Trash {
        i32 Id PK "履歴ID autoincrement"
        i32 ItemId PK "物品ID"
        String VisibleId FK "Label tableのIdとリレーションを張っている"
        String Name "物品名"
        String ProductNumber "空の文字列を許容 型番"
        String Description "空の文字列を許容 物品の説明"
        Option_dayetime PurchaseYear "購入年度"
        Option_i32 PurchasePrice "購入金額"
        Option_i32 Durability "耐用年数"
        boolean IsDepreciation "減価償却対象かどうか"
        Json Connector "端子 e.g. ['USB Type-A', 'HDMI'] (可変な配列)"
        boolean IsRent "貸出中かどうか"
        String Color "空の文字列を許容 e.g. 'Red^Orange^Brown'"
        datetime CreatedAt "作成日時"
        datetime UpdatedAt "更新日時"
        String Recipient "空の文字列を許容 貸出先"
        String RentalDescription "空の文字列を許容 備考"
        Option_datetime LatestRentAt "最終貸出日時"
        Option_datetime ScheduledReplaceAt "最終返却予定日時"
        Option_datetime LatestReplaceAt "最終返却日時"
    }
    Label {
        String VisibleId PK, FK "36進数のincrement"
        boolean IsMax "最新(最大)の記録のみtrue, それ以外はfalse"
        String Record "ActiveEnum {Qr, Barcode, Nothing}"
    }
    Connector {
        String id PK "autoincrement"
        String name UK "connector名"
        String status "ActiveEnum {Active, Archive}"
    }
    Color {
        String id PK "autoincrement"
        String name UK "色名"
        String status "ActiveEnum {Active, Archive}"
    }
```

# GraphDB (Neo4j)

一つの木構造を持っている。

```mermaid
flowchart LR
    id2((Id of Item Table)) --Parent--> id1((Id of Item Table))
    id3((Id of Item Table)) --Parent--> id1((Id of Item Table))
    id4((Id of Item Table)) --Parent--> id1((Id of Item Table))
    id5((Id of Item Table)) --Parent--> id2((Id of Item Table))
    id6((Id of Item Table)) --Parent--> id2((Id of Item Table))
```

# Meiliserach

`Visible Id`と`Record`だけ、Label Table にある

```mermaid
erDiagram
    Item {
        i32 Id PK "物品ID autoincrement"
        String VisibleId FK "物品に貼るID (Label Table)"
        String Record "ActiveEnum {Qr, Barcode, Nothing} (Label Table)"
        String Name "物品名"
        String ProductNumber "空の文字列を許容 型番"
        String Description "空の文字列を許容 物品の説明"
        Option_i32 PurchaseYear "購入年度"
        Option_i32 PurchasePrice "購入金額"
        Option_i32 Durability "耐用年数"
        boolean IsDepreciation "減価償却対象かどうか"
        Json Connector "端子 e.g. ['USB Type-A', 'HDMI'] (可変な配列)"
        boolean IsRent "貸出中かどうか"
        String Color "e.g. 'Red^Orange^Brown'"
        datetime CreatedAt "作成日時"
        datetime UpdatedAt "更新日時"
    }
    Connector {
        String id PK "autoincrement"
        String name UK "connector名"
        String status "ActiveEnum {Active, Archive}"
    }
    Color {
        String id PK "autoincrement"
        String name UK "色名"
        String status "ActiveEnum {Active, Archive}"
    }
```

# FrontEndで保有しているData

## 端子の種類

```typescript
type ConnectorType = "USB Type-A" | "USB Type-C";
```

## ケーブルに巻く色のパターン

色は、Camel Caseの英語名

```typescript
type Color = Red | Green;
```
