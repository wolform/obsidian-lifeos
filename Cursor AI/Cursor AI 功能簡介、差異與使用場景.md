
## 1. 三種主要模式

### Ask Mode（詢問模式）- 目前你使用的模式
- **用途**：純問答，不會修改任何檔案
- **場景**：理解程式碼邏輯、詢問架構設計、學習概念、Code Review
- **特點**：只讀工具（Read、Grep、Search），不會執行寫入操作

### Agent Mode（代理模式）
- **用途**：自主完成複雜的多步驟任務
- **場景**：新增功能、重構程式碼、修 Bug、建立專案、執行終端指令
- **特點**：可讀寫檔案、執行 Shell 指令、建立/刪除檔案、Git 操作、啟動子代理

### Manual Edit（手動編輯）/ Cmd+K (Ctrl+K)
- **用途**：針對選取的程式碼區塊做 inline 編輯
- **場景**：快速修改某段函式、產生 docstring、重寫一小段邏輯
- **特點**：精準、快速，適合小範圍修改

---

## 2. `@` 符號引用

| 引用方式 | 用途 |
|---------|------|
| `@file.php` | 引用特定檔案作為上下文 |
| `@folder/` | 引用整個資料夾 |
| `@Codebase` | 搜尋整個程式庫 |
| `@Web` | 搜尋網路獲取最新資訊 |
| `@Docs` | 引用已索引的文件庫 |
| `@Git` | 引用 Git 歷史紀錄（diff、log） |

---

## 3. Rules（規則）vs Skills（技能）

### Cursor Rules (`.cursor/rules/`)
- **用途**：持久化的 AI 行為設定，例如「回覆使用繁體中文」、「遵循特定程式碼風格」
- **場景**：專案層級的 coding convention、命名規範、架構偏好
- **生效方式**：每次對話自動載入

### Skills (`.cursor/skills/`)
- **用途**：可重複使用的任務指令模板
- **場景**：標準化流程，如「建立新的 Service」、「設定 Rule」
- **生效方式**：需要時由 AI 主動讀取並遵循

---

## 4. Tab Completion（自動補全）
- **用途**：寫程式時的即時 AI 補全建議
- **場景**：日常 coding，按 `Tab` 接受建議
- **特點**：根據上下文預測下一步程式碼，速度極快

---

## 5. 常見使用場景對照

| 場景                              | 建議使用                            |
| ------------------------------- | ------------------------------- |
| 「這段 Service 的邏輯是什麼？」            | **Ask Mode**                    |
| 「幫我在 BLNominateService 加一個新方法」  | **Agent Mode**                  |
| 「把這個 for 迴圈改成 Collection map」   | **Ctrl+K** inline edit          |
| 「整個專案 Repository 架構怎麼運作的？」      | **Ask Mode** + `@Codebase`      |
| 「幫我重構這 5 個檔案的命名」                | **Agent Mode**（多檔案操作）           |
| 「Laravel 11 的新 middleware 怎麼用？」 | **Ask Mode** + `@Web` 或 `@Docs` |
| 寫程式途中想要快速補全                     | **Tab Completion**              |

---

## 6. 小技巧

- **複雜任務先 Ask 再 Agent**：先用 Ask Mode 理解現有程式碼，確認方案後切換到 Agent Mode 執行
- **善用 `@` 縮小上下文**：提供精確的檔案引用，AI 回答更準確
- **Rules 設定好省重複溝通**：像你已經設定了「回覆用繁體中文」，這就不用每次再說一次

簡單來說：**問問題用 Ask、做事情用 Agent、小修改用 Ctrl+K、寫 code 中靠 Tab**。

# Cursor 的四種模式介紹

## 1. Ask 模式（詢問模式）

**用途：** 唯讀模式，用於提問、理解程式碼、獲取建議。不會對檔案做任何修改。

**適合場景：**
- 理解程式碼邏輯
- 學習新概念
- 獲取最佳實踐建議
- 探索 codebase 結構

**Prompt 範例：**

```
這個 useAuth hook 是怎麼運作的？請解釋認證流程。
```

```
React 的 useEffect 和 useLayoutEffect 有什麼差別？什麼時候該用哪個？
```

```
這個專案的資料夾結構是什麼？API routes 在哪裡定義的？
```

```
這段 SQL query 有什麼效能問題嗎？有沒有更好的寫法？
```

---

## 2. Agent 模式（代理模式）

**用途：** 最強大的模式。AI 可以自主讀取檔案、搜尋程式碼、編輯檔案、執行終端指令、安裝套件等。適合實際開發工作。

**適合場景：**
- 實作新功能
- 重構程式碼
- 修 Bug
- 建立新專案
- 執行 Git 操作

**Prompt 範例：**

```
建立一個 React + TypeScript 的 Todo App，使用 Tailwind CSS 做樣式，
包含新增、刪除、標記完成功能，資料存在 localStorage。
```

```
把 src/utils/api.ts 裡所有的 fetch 呼叫改用 axios，
並加上統一的錯誤處理和 request interceptor。
```

```
在 models/user.py 新增一個 email 驗證的 method，
用 regex 檢查格式，並在對應的 test 檔案裡加上單元測試。
```

```
把這個 class component 重構成 functional component with hooks，
保持所有功能不變。
```

---

## 3. Debug 模式（除錯模式）

**用途：** 專門用於除錯。可以直接貼上錯誤訊息或終端輸出，AI 會自動分析錯誤原因並嘗試修復。通常可從終端的錯誤訊息一鍵觸發（點擊終端錯誤旁的「Debug with AI」按鈕）。

**適合場景：**
- 解讀錯誤訊息
- 修復 runtime errors
- 解決 build/compile 錯誤
- 修復測試失敗

**Prompt 範例：**

```
TypeError: Cannot read properties of undefined (reading 'map')
在 src/components/UserList.tsx 第 23 行，幫我修復這個錯誤。
```

```
npm run build 出現以下錯誤：
Module not found: Can't resolve '@/components/Header'
請幫我找出原因並修復。
```

```
這個 API endpoint 回傳 500 錯誤，後端 log 顯示：
"sqlalchemy.exc.IntegrityError: UNIQUE constraint failed: users.email"
幫我找出問題並加上適當的錯誤處理。
```

```
我的 Jest 測試失敗了：
Expected: 200
Received: 401
幫我看看為什麼認證沒有通過。
```

---

## 4. Plan 模式（規劃模式）

**用途：** 先規劃、再執行。AI 會先產生一份詳細的實作計畫（包含要修改哪些檔案、步驟順序等），讓你審閱確認後才開始執行。適合大型或複雜的變更。

**適合場景：**
- 大型功能開發
- 跨多個檔案的重構
- 架構變更
- 需要先討論方案再動手的情況

**Prompt 範例：**

```
我想在這個 Next.js 專案中加入 Stripe 付款功能，
包含訂閱方案選擇、結帳頁面、webhook 處理和付款紀錄頁面。
請先規劃完整的實作步驟。
```

```
把這個 monolith Express API 拆分成微服務架構，
使用者服務和訂單服務要分開。請先列出計畫。
```

```
把整個專案從 JavaScript 遷移到 TypeScript，
請規劃遷移順序和每個步驟要做的事。
```

```
實作一個完整的 RBAC（角色權限控制）系統，
包含 admin、editor、viewer 三種角色。請先給我實作計畫。
```

---

## 快速對照表

| 模式 | 能否修改檔案 | 能否執行指令 | 最適合 |
|------|:---:|:---:|--------|
| **Ask** | ✗ | ✗ | 提問、理解、學習 |
| **Agent** | ✓ | ✓ | 實際開發、修改程式碼 |
| **Debug** | ✓ | ✓ | 除錯、修復錯誤 |
| **Plan** | ✓（確認後） | ✓（確認後） | 複雜任務的規劃與執行 |

> **提示：** 你可以在聊天輸入框的下拉選單中切換模式。一般建議先用 **Ask** 或 **Plan** 釐清需求，再切到 **Agent** 執行實作。遇到錯誤時用 **Debug** 快速定位問題。