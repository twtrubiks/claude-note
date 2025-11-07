# CLAUDE 筆記

* [Youtube Tutorial - Claude Code 技巧 (設定/指令/VSCode整合)](https://youtu.be/O9R5VwbxQdc)

* [Youtube Tutorial - Claude Opus 新限制來襲！$200 方案不再吃到飽，開發者必學 /model opusplan 聰明用法！](https://youtu.be/aJ0VtSOvkhM)

* [Youtube Tutorial - 雲端 AI 編碼大戰！Claude Code Web (Sonnet 4.5) VS Jules (Gemini 2.5 Pro)](https://youtu.be/8IZLzCM-Gho)

這篇文章主要是介紹 claude code 的一些用法以及想法, 如果有好用的我會再更新 [官方文檔](https://docs.anthropic.com/en/docs/claude-code/overview#install-and-authenticate)

* [Claude Skills](Claude-Skills.md) - [Youtube Tutorial - Claude Skills 實戰教學：用 Skill_Seekers 打造你的 Odoo 專家技能！](https://youtu.be/Ml5qTQymalY)

## 技巧

### 快速啟動

```cmd
claude "請簡單介紹一下什麼是 docker"
```

### plan mode 切換模式

切換 plan mode (shift + tab)

這個非常方便, 你可以用他來規劃.

## 初始化

第一次使用, 可以執行 `init`, 會幫你建立 CLAUDE.md

## 特殊符號代表的含意

```cmd
! for bash mode       double tap esc to clear input      ctrl + _ to undo
/ for commands        shift + tab to auto-accept edits   ctrl + z to suspend
@ for file paths      ctrl + r for verbose output
# to memorize         \⏎ for newline
```

也不一定要使用 `#` 指令, 因為直接和 CLAUDE 說請它幫你加進 CLAUDE.md 順便幫你整理我更喜歡.

### 不需要記憶系統指令

```cmd
claude -p "幫我找出 commands 底下有什麼東西"
```

### 客製化 command

建立一個路徑 command

```cmd
❯ pwd
/home/twtrubiks/.claude/commands
```

底下建立一個 `commit-fast.md` 裡面輸入你的 Prompt

重開 claude, 輸入 `/` 就會看到剛剛建立的 commit-fast,

補充說明一下, `.claude/` 這個資料夾其實紀錄很多東西.

## 加強思考關鍵字

使用強的模型 "ultrathink", 或是和它說仔細思考類似的.

## 在對話串中加入 URL 或文檔網址

可以提高精準度, 或是使用之前介紹的 Context7 MCP.

## 匯出資料

`/export` 指令, 快速匯出對話紀錄.

## 恢復對話紀錄

`/resume` 你可以恢復過去的對話紀錄

## 暫停以及恢復

在 CLAUDE 中, 如果你想要刪除目前輸入的內容, 使用 Ctrl+U,

如果你想要暫停使用 Ctrl+Z, 可以再執行 fg 來恢復狀態.

如果還沒啟動 CLAUDE 可以使用

`claude --continue`

`claude --resume`

## 強制中斷 claude

直接點選 `Esc` 可取消目前的對話

## 圖片支援

可以直接拖拉圖片進去, 有 OCR 也可以改檔名, 也可以複製貼上

## 換行

結尾打 `\` 然後 enter 就會自動換行

## MCP

大家可以自己加入自己喜歡的 MCP, 我之前也介紹過很多了,

這邊我就用 sequential-thinking 來說明,

### 增加 MCP

```cmd
claude mcp add sequential-thinking -s user npx @modelcontextprotocol/server-sequential-thinking
```

這邊加 `-s user` 代表的是 全局變量, 也就是不是針對特定專案底下會生效而已.

## 更新

例如這邊想增加成 全局變量

```cmd
claude mcp update <server-name> -s user
```

## 移除 MCP

```cmd
claude mcp remove sequential-thinking
```

## 不需要權限的模式

這個模式我很喜歡, 簡單說, 就是執行過程中, 不會再問你要不要執行之類的.

```cmd
claude --dangerously-skip-permissions
```

但也要記住, 如果它跑偏了, 記得手動按 `Esc` 去中斷.

## VSCode 整合

先安裝 Claude 套件, 然後記得打開 Claude, 如果它沒有連動你的 IDE, 請在 Claude 中輸入 `/ide`,

這功能很贊, 因為你可以從 VSCode 中去選程式碼, Claude 是可以讀的到的.

使用 `/terminal-setup` 設定, shift + enter 可換行.

## Claude 費用

我一開始以為吃到飽, 但我後面發現我錯了.

[About Claude's Max Plan Usage](https://support.anthropic.com/en/articles/11014257-about-claude-s-max-plan-usage)

| 項目 | Max 方案 ($100/月) | Max 方案 ($200/月) |
| :--- | :--- | :--- |
| **每月價格** | $100 美金 | $200 美金 |
| **總用量** | Pro 方案的 **5 倍** | Pro 方案的 **20 倍** |
| **目標用戶** | 頻繁使用者，處理多樣化任務 | 每日重度使用者，依賴 Claude 完成大部分工作 |
| **一般訊息用量** | 每 5 小時**至少 225 則**訊息 | 每 5 小時**至少 900 則**訊息 |
| **程式碼用量 (Claude Code)** | 每 5 小時約 **50 - 200 次**提示 | 每 5 小時約 **200 - 800 次**提示 |

---

### **共同功能與限制 (適用於以上兩種方案)**

* **200K 上下文窗口：**
    與 Pro 方案相同，提供 200K Token 的上下文處理能力。

* **用量重置週期：**
    所有訊息用量限制**每 5 小時重置一次**。

* **月度會話上限：**
    每月最多 50 個會話（每個會話為一個 5 小時的週期）。

## Approaching Opus weekly limit

應該是因為太多人買 200美金瘋狂用, claude code 想要降低成本,

所以多了這一個限制.

因為 Opus 費用大約就是 Sonnet 的 5倍, 所以我完全可以理解他們為什麼要加入這個新的限制.

這樣子的話, 其實就必須多用

`/model opusplan` 規劃的時候用 Opus, 執行的時候用 Sonnet.

畢竟當初沒有這個限制的時候, 我根本不管這個指令, 反正就是 Opus 吃到飽,

也不是我懶的切換, 是因為 Opus 我真心喜歡 (雖然有過度設計的狀況),

但如果你很清楚你的架構, 自己再調整一下就好.

(像最近為了避免達到限制, 我使用 Sonnet 感覺就有降智, 要多調幾次, 不像之前 Opus 可幾乎一次到位)

但現在這個限制出來, 你就沒辦法吃到飽了, 如果依照以前的用法, 很快就會被限制了.

我以前的公司也是買一個帳號, 多人使用 claude code, 但這個限制出來,

我想就沒辦法這樣搞了, 因為現在這種限制, 你如果高頻用一點, 就會達到限制了.

## 推薦幾個工具以及文章

查看 Claude 使用量 - github

[ccusage](https://github.com/ryoppippi/ccusage) - 你想要知道你用多少的 token 裝這個就對了.

最後推薦一個 [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code/tree/main), 你在這裡面可以發現很多很酷的東西.
