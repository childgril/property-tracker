# 權狀管理系統 Property Tracker

土地與建物權狀的個人/家庭管理工具,純前端單檔應用,可放 GitHub Pages 分享給家人。

## 功能

- ✅ **土地、建物權狀分開管理**,可雙向關聯配對顯示
- ✅ **所有權人變更歷程**(買賣、贈與、繼承皆可記錄)
- ✅ **持分管理**(分子/分母,支援持分合併與拆分)
- ✅ **完整買賣金額**(成交價、代書、過戶、手續費、銀行貸款、其他)
- ✅ **稅金紀錄**(房屋稅、地價稅、契稅、土地增值稅、所得稅、其他)
- ✅ **租賃管理**(承租人、租期、月租、押金、收租明細)
- ✅ **報表分析**(每筆權狀損益、年度稅金小計)
- ✅ **GitHub Gist 雲端同步**,擁有者編輯、家人唯讀查看
- ✅ **JSON 匯入/匯出**備份

## 部署到 GitHub Pages(讓家人在線上看)

### 步驟 1:建立 repository
1. 登入 GitHub,點右上 ＋ → New repository
2. 名稱填例如 `property-tracker`
3. 設為 **Public**(GitHub Pages 免費版需要 public)
4. 建立後,把 `index.html` 上傳進去

### 步驟 2:開啟 Pages
1. 進入 repository → Settings → Pages
2. Source 選 `Deploy from a branch`,Branch 選 `main` → `/(root)`
3. 等 1-2 分鐘後會給你網址,類似:`https://你的帳號.github.io/property-tracker/`

### 步驟 3:擁有者首次設定
1. 開啟網站 → 點左下「⚙ 設定」
2. 模式選 **擁有者**
3. 到 [github.com/settings/tokens/new?scopes=gist](https://github.com/settings/tokens/new?scopes=gist&description=Property%20Tracker) 建立 Token(**勾 gist 權限**),複製貼到設定中
4. 儲存設定
5. 開始新增權狀資料,完成後點「☁ 同步雲端」
6. 第一次同步會建立一個 Gist,系統會自動填入 Gist ID
7. **重要**:到 GitHub Gists 找到這個 Gist,點右上 → Make public(這樣家人不需要 Token 也能讀)
8. 複製 Gist ID,傳給家人

### 步驟 4:家人查看
家人有兩種方式:

**方法 A:用網址參數(最簡單)**

擁有者把這個網址傳給家人:
```
https://你的帳號.github.io/property-tracker/?gist=GIST_ID&mode=viewer
```
家人點開就自動載入,無需任何設定。

**方法 B:手動設定**

1. 家人開啟網站 → 設定
2. 模式選 **觀看者**
3. Gist ID 欄位貼上擁有者給的 ID
4. 儲存設定 → 點「⬇ 雲端載入」

## 日常使用

- **新增/修改資料後**,記得點「☁ 同步雲端」上傳
- **觀看者**重新開啟網站會自動載入最新資料,也可手動點「⬇ 雲端載入」
- 不想用雲端的話,也可以用「匯出 JSON」/「匯入 JSON」做檔案備份

## 安全提醒

- GitHub Token 只存在你瀏覽器的 localStorage,不會上傳到任何地方
- **不要分享你的 Token**,只分享 Gist ID
- Gist 設為 public 後,任何知道 ID 的人都能讀取(但 ID 是長串隨機字串,不易被猜中)
- 若想更嚴格的隱私,讓家人也建 GitHub 帳號,Gist 保持 secret,觀看者也填 Token(會自動帶 Authorization)

## 資料結構

每個權狀(property)包含:
```
{
  type: 'land' | 'building',
  name, address, parcelId, zoning, area, areaUnit,
  acquireDate, owner, notes,
  linkedTo: [其他權狀id],      // 配對的土地/建物
  ownerHistory: [{date, name, share, method, notes}],
  transactions: [{type, date, counterparty, share, price, fees, loanInfo, notes}],
  taxes: [{date, kind, period, amount, notes}],
  rentals: [{tenant, startDate, endDate, monthlyRent, deposit, ..., payments:[]}]
}
```
