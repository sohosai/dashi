# RDB (PostgreSQL)

Table 同士の親と子の関係は以下の通り

| 親          | 子         |
| ----------- | ---------- |
| Lable Table | Item Table |
| Item Table  | Rent Table |

```mermaid
erDiagram
    Item ||--o{ Rent : "Id<->ItemId"
    Item |o--|| Label : "VisibleId<->VisibleId"
    Item {
        i32 Id PK "物品ID autoincrement"
        String VisibleId FK "Label tableのIdとリレーションを張っている"
        boolean IsWaste "廃棄物かどうか"
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
    Label {
        i32 Id PK "autoincrement"
        String VisibleId FK, UK "36進数のautoincrement"
        String Record "ActiveEnum {Qr, Barcode, Nothing}"
    }
    Rent {
        i32 Id PK "autoincrement"
        i32 ItemId FK "Item TableのIdとリレーションを張っている"
        String Recipient "貸出先"
        String Description "空の文字列を許容 備考"
        datetime RentAt "貸出日時"
        datetime ReturnAt "返却日時"
    }
```

# GraphDB (Neo4j)

一つの木構造を持っている。

```mermaid
flowchart LR
    id2((Id of Item Table)) --ItemTree--> id1((Id of Item Table))
    id3((Id of Item Table)) --ItemTree--> id1((Id of Item Table))
    id4((Id of Item Table)) --ItemTree--> id1((Id of Item Table))
    id5((Id of Item Table)) --ItemTree--> id2((Id of Item Table))
    id6((Id of Item Table)) --ItemTree--> id2((Id of Item Table))
```

# Meiliserach

`Visible Id`と`Record`だけ、Label Table にある

```mermaid
erDiagram
    Item {
        i32 Id PK "物品ID autoincrement"
        String VisibleId FK "物品に貼るID (Label Table)"
        String Record "ActiveEnum {Qr, Barcode, Nothing} (Label Table)"
        boolean IsWaste "廃棄物かどうか"
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
