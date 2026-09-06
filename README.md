# ZenPos 客人點餐頁（GitHub Pages）

`docs/index.html` 是可以放到 **GitHub Pages** 的客人點餐頁。它本身是純靜態網頁，
菜單與訂單都即時打你的 **ZenPos 後端 API**：

- 客人在這個網頁點餐 → `POST {api}/stores/{store}/orders` → iPad 會同步收到訂單。
- iPad 新增/修改菜單 → 推到後端 → 這個網頁**即時抓最新菜單**（每 2 秒、回到前景時也會刷新），自動同步。

## 每間店的固定點餐網址

公開網址只需要帶店家的 `store`：

```
https://jason611130.github.io/zenpos-order/?store=店家ID
```

- `store` 是後端建立店家時產生的完整店家 ID；每間店都不同，也不會因改店名而改變。
- 後端網址由同一個 `config.json` 提供，所以 Cloudflare Tunnel 改變時，不需要更換店家的點餐網址或重印 QR Code。
- 沒有 `store` 的首頁不會任意顯示第一間店，避免不同店家的菜單與訂單混在一起。
- 管理員可在後台「商店」或「帳號訂閱」頁直接開啟、複製各店網址。

## 開啟 GitHub Pages

1. 把整個 repo 推到 GitHub。
2. GitHub → **Settings → Pages** → Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾選 **/(root)**，Save。
3. 幾分鐘後會得到 GitHub Pages 網址，後面接上 `?store=店家ID` 即可。

## ⚠️ 後端必須是「公開 + HTTPS」

GitHub Pages 是 https 的公開網站，**不能**連你家區網的 `http://Jason-3.local:3000`。
要讓 GitHub 上的點餐頁和 iPad 串在一起，後端得部署到可從網際網路連到、且有 HTTPS 憑證的主機，例如：

- Render / Railway / Fly.io / Zeabur（Node 一鍵部署，附 https 網址）
- 或自架 + Cloudflare Tunnel / ngrok（給你一個 https 對外網址）

iPad 與此網頁都會讀取 GitHub Pages 的 `config.json`，共用同一個 Windows 後端；
這樣「網頁點餐 → iPad 收單」「iPad 改菜單 → 網頁同步」才會走同一份資料。

> 本機開發時（電腦當後端、iPad 同一個 Wi-Fi）維持現狀即可，不需要這個 GitHub 頁；
> 這個頁是給你要用「固定網址的公開點餐網站」時使用。
