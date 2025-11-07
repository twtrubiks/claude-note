# Claude Skills

* [Youtube Tutorial - Claude Skills 實戰教學：用 Skill_Seekers 打造你的 Odoo 專家技能！](https://youtu.be/Ml5qTQymalY)

## 什麼是 Claude Skills？

Claude Skills 可以想像成一種**技能包、SOP (標準作業程序)、特定領域的工作流程**。

它讓 Claude 能夠以模組化的方式獲得專業知識，只在需要時才載入相關技能，大幅提升效率。

### 核心優勢：結構化索引

**Skills 最大的優勢在於採用「結構化索引」的設計理念：**

就像圖書館的卡片目錄系統，Claude 不需要一次翻閱所有書籍，而是：
1. **先掃描索引卡**：快速瀏覽所有 Skills 的 name 和 description（frontmatter）
2. **找到相關的書**：識別出需要的 Skill 並載入完整內容
3. **按需查閱**：只在必要時才讀取更深層的資源和腳本

這種方式讓 Claude 能夠管理**數十甚至上百個技能**，同時保持極高的效率，因為大部分技能只會停留在「索引」階段，不會佔用實際的 token 空間。

**官方資源：**

- [官方 Skills Repository](https://github.com/anthropics/skills)
- [Awesome Claude Skills](https://github.com/travisvn/awesome-claude-skills)

---

## 關鍵特性

Skills 的強大之處在於以下四大特性：

### 1. 可組合 (Composable)

技能會自動堆疊。Claude 會識別需要哪些技能，並協調它們的使用，無需手動干預。

### 2. 可移植 (Portable)

技能在任何地方都使用相同的格式。建構一次，即可在 Claude.ai、Claude Code CLI、Claude API 和自訂代理 (agents) 中使用。

### 3. 高效 (Efficient)

Claude 只在需要時載入所需的內容。透過漸進式載入機制，每個技能在未被使用時僅掃描 frontmatter（name 和 description）。

### 4. 強大 (Powerful)

技能可以包含可執行程式碼，用於處理傳統程式設計比 token 生成更可靠的任務。

---

## Skills 與其他方法的比較

### Skills vs MCP (模型情境協議)

| 功能特性 | 技能 (Skills) | MCP |
|---------|--------------|-----|
| 目的 | 特定任務的專業知識與工作流程 | 整合外部資料/API |
| 可攜性 | 在所有地方 (Claude.ai, Code, API) 格式都相同 | 需要伺服器設定 |
| 程式碼執行 | 可包含可執行的腳本 | 提供工具/資源 |
| Token 效率 | 漸進式載入，未使用時僅掃描 frontmatter | 依實作方式而異 |
| 最適用於 | 可重複執行的任務、文件工作流程 | 資料庫存取、API 整合 |

**簡單理解：**

- **MCP (通用連接器)**：像是一個通用的「連接埠」或「橋樑」，讓 Claude 知道「有一個外部工具或 API 可以用」，主要處理外部資料和 API 的整合。
- **Skill (特定專家)**：像是一套「專家操作手冊」或「SOP」，教 Claude「如何去執行一個具體的、多步驟的任務」，主要處理特定領域的專業知識和工作流程。

**重要：** Skills 不是為了取代 MCP，它是為了更特定的情境下使用。

### Skills vs 系統提示詞 (System Prompts)

| 功能特性 | 技能 (Skills) | 系統提示詞 (System Prompts) |
|---------|--------------|---------------------------|
| 結構 | 包含 YAML 檔頭、指令、腳本的資料夾 | 純文字指令 |
| 可重複使用性 | 可版本控制、可分享、可組合 | 複製貼上、特定於單次對話 |
| 載入方式 | 依需求載入 (僅在相關時) | 始終佔用情境 (Context) |
| 維護 | 集中式更新 | 每次對話都需手動更新 |
| 可組合性 | 多個技能可自動堆疊 | 需要手動組合 |

**關鍵優勢：** 技能 (Skills) 是依需求載入的，在未被使用時僅掃描 frontmatter（name 和 description），不載入完整內容。

---

## 為什麼 Skills 如此高效？Token 優勢詳解

### Skill 架構 = 智能模組化 + 延遲載入

Skill 透過漸進式設計，讓 Claude 可以：

1. **掃描所有技能** (frontmatter: name + description)
2. **載入相關技能的完整說明** (SKILL.md body)
3. **按需讀取支援文件** (references)
4. **執行預寫好的腳本** (scripts)

這就是為什麼 Skill 能大幅節省 token，同時保持功能完整性！

### 傳統方式 vs Skill 方式

**傳統方式 (System Prompt)：**

```text
開始對話時就把所有說明書都攤在桌上 📚📚📚
      ↓
佔滿整張桌子，很擠！
```

**Skill 方式 (漸進式)：**

```text
第 1 層: 只放「目錄卡片」在桌上 📋
         ↓
      使用者說：「我要處理 PDF」
         ↓
第 2 層: 拿出 PDF 那本說明書的「摘要」📖
         ↓
      使用者說：「要填表單」
         ↓
第 3 層: 翻到「表單填寫」那一章的詳細內容 📄
```

### 漸進式載入架構

Skills 採用漸進式載入機制，根據需求分階段載入內容：

```text
階段 1: Frontmatter (永遠掃描)
├─ name: "pdf-editor"
└─ description: "PDF 處理技能..." (最多 1024 字元)

階段 2: SKILL.md Body (當 Skill 被觸發時才載入)
└─ 核心工作流程說明、使用範例、最佳實踐

階段 3: 支援資源 (按需載入)
├─ scripts/rotate_pdf.py (可直接執行，不佔 context)
├─ references/api_docs.md (Claude 需要時才讀取)
└─ assets/template.pdf (用於輸出，不載入 context)
```

**實際運作流程：**

```text
# 系統掃描所有 Skills 的 frontmatter
pdf-skill: "PDF 處理" (name + description)
excel-skill: "Excel 處理" (name + description)
word-skill: "Word 處理" (name + description)
...更多 Skills

↓ 使用者說：「處理 PDF」
↓ Claude 識別並觸發 pdf-skill
↓ 載入 pdf-skill 的 SKILL.md 完整內容
↓ 需要時再載入相關的 scripts 和 references
```

**效率優勢：**

相較於傳統的 System Prompt 會一次載入所有內容，Skills 採用漸進式載入，只在需要時才載入相關內容，大幅減少 token 消耗。

### 實際對比

#### 情境：假如有 10 個不同領域的指南

**方案 A：傳統主文件方式**

```text
MAIN_GUIDE.md:
├─ PDF 說明段落
├─ Excel 說明段落
├─ Word 說明段落
├─ PPT 說明段落
├─ Image 說明段落
├─ ... (共 10 個功能的說明)
└─ 每個段落都有詳細說明

問題: 所有說明文字永遠佔用 context ❌
```

即使你用引用，**主文件裡的說明文字還是會永遠佔用空間**！

**方案 B：Skill 架構**

```text
Skills frontmatter:
├─ pdf: "處理 PDF" (name + description)
├─ excel: "處理 Excel" (name + description)
├─ word: "處理 Word" (name + description)
├─ ... (共 10 個)

優勢: 永遠只掃描 frontmatter ✅

只有被觸發的 Skill 才載入完整內容！
```

### 關鍵差異總結

| 面向 | 傳統主文件方式 | Skill 架構 |
|------|---------------|-----------|
| **主文件大小** | 必須包含所有功能的說明 | 只有極簡的 name + description |
| **永遠佔用** | 整個主文件 | 只掃描 frontmatter |
| **觸發機制** | 手動 (Claude 需要你說才讀) | 自動 (Claude 看 description 自動選) |
| **擴展性** | 加功能 = 主文件變更大 | 加功能 = 只多一筆 frontmatter |

### Token 效率概念說明

**核心優勢：**

傳統 System Prompt 方式需要一次載入所有功能的說明文字，即使你只使用其中一個功能。

Skills 方式只需要：

1. 永遠掃描：所有 Skills 的 frontmatter（name + description）
2. 按需載入：被觸發的 Skill 的完整內容（SKILL.md body）
3. 按需載入：需要時才載入 scripts 和 references

**規模越大，優勢越明顯：**

當你有越多功能時，Skills 的效率優勢越顯著：
- 10 個功能：差異明顯
- 50 個功能：差異非常顯著
- 100+ 個功能：傳統方式幾乎不可行，Skills 仍能高效運作

### ⚠️ 重要前提

Skill 省 token 的前提是：

```text
✅ description 寫得好 → Claude 能正確觸發
❌ description 寫不好 → Claude 不觸發 → 等於沒用
```

---

## 建立自己的 Claude Skills

### 使用 Skill Creator

可以直接在網頁上使用 [skill-creator](https://github.com/anthropics/skills/tree/main/skill-creator) 建立。

### 目錄結構

```text
my-skill/
├── SKILL.md         # 包含 frontmatter 的主要技能檔案
├── scripts/         # (可選) 可執行腳本
│   └── helper.py
└── resources/       # (可選) 支援檔案
    └── template.json
```

### SKILL.md 格式

創建包含 frontmatter 的 SKILL.md：

```yaml
---
name: my-skill
description: 用於技能探索的簡短描述 (保持簡潔，最多 1024 字元)
allowed-tools:  # 可選：限制此 Skill 可使用的工具
  - Read
  - Write
  - Bash
---

# 詳細說明

當技能被啟動時，Claude 將閱讀這些說明。

## 用法 (Usage)

解釋如何使用此技能...

## 範例 (Examples)

提供清晰的範例...
```

#### frontmatter 欄位說明

- `name`（必填）：Skill 的唯一識別名稱，小寫字母，使用連字號分隔，最多 64 字元
- `description`（必填）：Skill 的簡短描述，最多 1024 字元，**這個欄位非常重要，影響 Claude 是否會觸發此 Skill**
- `allowed-tools`（可選）：限制此 Skill 可使用的工具列表，用於安全控制

### 其他建立工具

也可以使用開源的 [Skill_Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) 來完成。

### Skill 的存儲位置

Skills 有三種存儲位置，依照不同的使用場景：

#### 1. Personal Skills (個人技能)

```bash
~/.claude/skills/{skill-name}/
```

- 存放在用戶主目錄
- 適用於個人專用的技能
- 不會被加入版本控制

#### 2. Project Skills (專案技能)

```bash
.claude/skills/{skill-name}/
```

- 存放在專案目錄下
- 可透過 Git 與團隊共享
- 適用於團隊協作的技能

#### 3. Plugin Skills (插件技能)

- 隨 Claude Code 插件一起安裝
- 透過 Plugin Marketplace 管理
- 由社群或官方提供

### 安裝 Skill 到 Claude Code

1. 選擇適當的存儲位置（Personal 或 Project）
2. 把整個 skill 資料夾放到對應目錄
3. 重新打開 Claude Code
4. 驗證：詢問 Claude 目前有哪些 skill

---

## 安全性

### ⚠️ 重要提示

技能 (Skills) 可以在 Claude 的環境中執行任意程式碼。**請僅安裝來自受信任來源的技能**。

### allowed-tools：工具權限控制

`allowed-tools` 是 SKILL.md frontmatter 中的重要安全功能，用於限制 Skill 可以使用的工具。

#### 功能說明

當 Skill 啟用時，如果設定了 `allowed-tools`，Claude **只能使用指定的工具，且不需要額外請求權限**。

#### 使用範例

```yaml
---
name: read-only-analyzer
description: 唯讀分析工具，僅能讀取檔案進行分析
allowed-tools:
  - Read
  - Grep
  - Glob
---

# 唯讀分析器

此技能只能讀取和搜尋檔案，無法進行任何寫入或修改操作。
```

#### 使用場景

**1. 防止意外修改**

```yaml
allowed-tools:
  - Read
  - Bash
# 排除 Write、Edit 等修改工具
```

**2. 限制範圍**

```yaml
allowed-tools:
  - Read
  - Write
# 只允許檔案操作，不允許執行 Bash 命令
```

**3. 安全敏感的工作流程**

```yaml
allowed-tools:
  - Read
  - Grep
# 僅允許讀取操作，適用於稽核或檢查場景
```
