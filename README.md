# ZenPos 客人點餐頁（GitHub Pages）

`docs/index.html` 是可以放到 **GitHub Pages** 的客人點餐頁。它本身是純靜態網頁，
菜單與訂單都即時打你的 **ZenPos 後端 API**：

- 客人在這個網頁點餐 → `POST {api}/stores/{store}/orders` → iPad 會同步收到訂單。
- iPad 新增/修改菜單 → 推到後端 → 這個網頁**即時抓最新菜單**（每 2 秒、回到前景時也會刷新），自動同步。

## 用網址參數指定後端與店家

開啟時帶兩個參數：

```
https://<你的帳號>.github.io/<repo>/?api=https://你的後端網址&store=你的店家ID
```

- `api`：後端的公開網址（**必須是 https**）。
- `store`：店家 ID（在 App 設定 → 資料同步，或後端 `/stores` 可查到）。

## 開啟 GitHub Pages

1. 把整個 repo 推到 GitHub。
2. GitHub → **Settings → Pages** → Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾選 **/docs**，Save。
3. 幾分鐘後會得到 `https://<帳號>.github.io/<repo>/` 網址，後面接上 `?api=...&store=...` 即可。

## ⚠️ 後端必須是「公開 + HTTPS」

GitHub Pages 是 https 的公開網站，**不能**連你家區網的 `http://Jason-3.local:3000`。
要讓 GitHub 上的點餐頁和 iPad 串在一起，後端得部署到可從網際網路連到、且有 HTTPS 憑證的主機，例如：

- Render / Railway / Fly.io / Zeabur（Node 一鍵部署，附 https 網址）
- 或自架 + Cloudflare Tunnel / ngrok（給你一個 https 對外網址）

iPad 端也要把後端位址（`RESTAURANT_API_BASE_URL`，在 `project.yml`）改成同一個公開網址，
這樣「網頁點餐 → iPad 收單」「iPad 改菜單 → 網頁同步」才會走同一個後端。

> 本機開發時（電腦當後端、iPad 同一個 Wi-Fi）維持現狀即可，不需要這個 GitHub 頁；
> 這個頁是給你要用「固定網址的公開點餐網站」時使用。
