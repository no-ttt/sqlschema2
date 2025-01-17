📌 資料庫資安健檢與分析系統

📌 實作筆記：https://hackmd.io/@nooo/Hy8oHbgjc

# 系統使用說明
## 安裝環境
下載 [.Net Core 3.1](https://dotnet.microsoft.com/en-us/download/dotnet/3.1)

![](https://i.imgur.com/bQuDYvL.png)


## 如何執行 ?
1. 下載專案
2. 打開 `appsettings.json`，輸入資料庫資訊

![](https://i.imgur.com/Taoh5v2.png)

3. 執行 `WebAPI.exe`，連線 http://localhost:5000


# 自動化擷取註解
> execute: regex.py

## 注意事項
### 1. Table / View / Function / Procedure 的註解請寫在 `create` 那一行

example. 
```sql=
CREATE TABLE Restaurant ( -- 全台所有餐廳
    ...
);
```
### 2. 欄位註解請跟要註解的欄位名稱寫在同一行

example. 
```sql=
CREATE TABLE Restaurant (
    RID int, -- ID
    PictureUrl varchar(400), -- 照片連結
    PictureDes nvarchar(8000), -- 照片介紹
    OpenTime nvarchar(1000), -- 營業時間
    ParkingInfo nvarchar(1000) -- 停車資訊
);
```

### 3. Function / Procedure 的參數命名不要加 `AS`
example.

```sql=
CREATE PROCEDURE get_city  -- 取得縣市
	@city VARCHAR(20) -- 縣市資訊
    -- @city AS VARCHAR(20)  (X)
AS
BEGIN
...
```

## View Table 使用例外
view table 寫法較為多元，所以有可能會抓不到註解 ... QQ
目前只有這款寫法會過：

```sql=
CREATE VIEW vd_Spot AS  -- spot view table
SELECT 
    o.OID as OID,  -- ID
    o.CName as Name, -- 景點名稱
    o.Type as Type,  -- 景點種類
    c2.CName as city, -- 所在縣市
    c.CName as town,   -- 所在城鎮
    s.PictureUrl as PictureUrl  -- 照片連結
FROM Object o, Class c, Class c2, CO, ScenicSpot s, Inheritance i
...
```

### 解決方法
1. 多寫一點 Regex expression
2. 直接開前端寫註解 (推 👍🏻)

## 如何執行
1. 打開 `regex.py` 填上輸入資料夾及輸出檔案的路徑

![](https://i.imgur.com/umahVc4.png)

:::warning
📌 請把所有要進行截取註解的 `.sql` 檔放到同一個資料夾內 !!
:::

2. 執行 `python3 regex.py`


