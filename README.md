# YouTube 逐字稿抓取服務（給「內容特工隊」爆款拆解模組用）

這是一個只有一支API的小型後端，功能：給它一個YouTube連結，回傳這支影片的純文字逐字稿。

做法是伺服器端讀取YouTube影片頁面、解析出頁面裡內嵌的字幕檔網址，再把字幕內容轉成純文字。這是非官方做法（YouTube沒有公開的逐字稿下載API），只對「有上字幕（人工或YouTube自動產生皆可）」的影片有效。

## 部署到 Vercel（約5分鐘）

### 方法A：用 Vercel CLI（推薦，最快）

1. 如果還沒有 Node.js，先安裝：https://nodejs.org
2. 打開終端機，安裝 Vercel CLI：
   ```
   npm install -g vercel
   ```
3. 切到這個資料夾，執行部署：
   ```
   cd yt-transcript-backend
   vercel
   ```
4. 依照畫面指示操作：
   - 第一次使用會要你登入（會開瀏覽器讓你用GitHub/Google/Email登入）
   - 專案名稱可以直接按Enter用預設值
   - 其他問題都選預設值即可
5. 部署完成後，終端機會顯示一個網址，例如：
   ```
   https://yt-transcript-backend-xxxx.vercel.app
   ```
   這就是你的後端網址，等等要貼到「內容特工隊」的設定裡。

### 方法B：用 GitHub + Vercel 網站（不用裝任何東西）

1. 把這個資料夾上傳成一個新的GitHub repository
2. 到 https://vercel.com 註冊/登入（可以直接用GitHub帳號登入）
3. 點「Add New」→「Project」，選擇你剛剛上傳的repository
4. 其他設定都用預設值，直接點「Deploy」
5. 部署完成後，Vercel會給你一個網址，格式跟上面一樣

## 部署完成後，怎麼測試？

在瀏覽器網址列直接貼上（記得換成你自己的網址和一部有字幕的YouTube影片連結）：
```
https://你的網址.vercel.app/api/transcript?url=https://www.youtube.com/watch?v=某個影片ID
```
如果成功，會看到一段JSON，裡面的 `transcript` 欄位就是逐字稿文字。
如果影片沒有字幕，會看到錯誤訊息，這是正常的（不是每部影片都有字幕）。

## 接下來

把這個網址貼到「內容特工隊」爆款拆解模組裡的「後端服務網址」欄位，之後貼YouTube連結按抓取，就會自動帶出完整逐字稿。

## 重要提醒

- 這個做法**依賴YouTube目前的網頁結構**，如果YouTube改版，這支API可能會突然失效，需要更新程式碼。
- 這是**非官方**的方式（沒有使用YouTube官方逐字稿下載API，因為那個API需要OAuth授權，而且只能下載你自己上傳的影片字幕）。抓取的是**公開影片頁面上本來就看得到的字幕**，不涉及付費內容或登入才能看的內容。
- TikTok沒有類似的公開字幕來源，這支後端**只支援YouTube**，TikTok目前仍需手動貼上文字。
- Vercel的免費方案（Hobby）額度對這種小工具通常很夠用，但如果使用量變大，可以留意一下Vercel的用量計費規則。
