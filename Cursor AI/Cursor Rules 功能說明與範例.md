## 什麼是 Cursor Rules？

Cursor Rules 是放在專案 `.cursor/rules/` 目錄下的設定檔，用來**持久化地指導 AI 的行為**。每次對話時 AI 會自動載入這些規則，不需要你每次重複說明。

可以把它想成：**給 AI 的專案說明書 + 行為守則**。

---

## Rules 的種類

| 類型 | 觸發方式 | 適用場景 |
|------|---------|---------|
| **Always** | 每次對話都自動載入 | 通用規則：語言、風格、團隊規範 |
| **Auto Attached** | 當開啟符合 glob pattern 的檔案時載入 | 檔案類型專屬規則，如 `*.php`、`*.tsx` |
| **Agent Requested** | AI 自行判斷是否需要載入 | 特定工具/流程的使用說明 |
| **Manual** | 使用者在對話中用 `@ruleName` 手動引用 | 偶爾才需要的特殊規則 |

---

## 實際範例

### 範例 1：通用專案規則（Always）

**檔案**：`.cursor/rules/project-general.mdc`

```markdown
---
description: General project conventions
globs:
alwaysApply: true
---

# Project Rules

- This is a Laravel 10 PHP project with a Service-Repository pattern
- Always respond in 繁體中文
- Use PHP 8.1+ features (enums, readonly properties, named arguments)
- Follow PSR-12 coding standard
- All comments must be written in English
- Never modify migration files that already exist
- Always use dependency injection, never use `new` directly in Service classes
```

---

### 範例 2：PHP 檔案專屬規則（Auto Attached）

**檔案**：`.cursor/rules/php-conventions.mdc`

```markdown
---
description: PHP coding conventions for this Laravel project
globs: ["**/*.php"]
alwaysApply: false
---

# PHP Conventions

- Use strict types: always add `declare(strict_types=1);`
- Type hint all method parameters and return types
- Use early return pattern to reduce nesting
- Repository methods should return Eloquent Collection or Model, never raw arrays
- Service methods should return arrays with consistent structure: `['success' => bool, 'data' => mixed, 'message' => string]`
- Log all external API calls with request/response payload using Laravel Log facade
```

---

### 範例 3：API Service 層規則（Auto Attached）

**檔案**：`.cursor/rules/api-services.mdc`

```markdown
---
description: Rules for API Service classes
globs: ["**/Services/API/*.php"]
alwaysApply: false
---

# API Service Rules

- All API Services must extend `BaseAPIService`
- External API calls must be wrapped in try-catch with proper logging
- Nominate services (BL/AWB) follow this flow: validate → query DB → call external API → update DB → return result
- Timeout for external API should be read from `config('global.logink.timeout')`
- Never call Repository methods directly from Controllers; always go through Service layer
```

---

### 範例 4：Git Commit 規則（Agent Requested）

**檔案**：`.cursor/rules/git-commit.mdc`

```markdown
---
description: Git commit message conventions for this project
globs:
alwaysApply: false
---

# Git Commit Rules

- Use conventional commit format: `type(scope): description`
- Types: feat, fix, refactor, docs, test, chore
- Scope examples: bl-nominate, awb-nominate, schedule, config
- Description in English, imperative mood, max 72 chars
- Example: `feat(bl-nominate): add cancel nomination endpoint`
```

---

### 範例 5：測試規則（Auto Attached）

**檔案**：`.cursor/rules/testing.mdc`

```markdown
---
description: Testing conventions
globs: ["**/tests/**/*.php"]
alwaysApply: false
---

# Testing Rules

- Use PHPUnit with Laravel TestCase
- Mock all external API calls, never make real HTTP requests in tests
- Use Factories for model creation
- Test file naming: `{ClassName}Test.php`
- Each test method should test ONE behavior
- Name pattern: `test_{action}_{condition}_{expectedResult}`
- Example: `test_nominateBL_withInvalidBLNo_returnsError()`
```

---

### 範例 6：資料庫 / Migration 規則（Manual）

**檔案**：`.cursor/rules/database.mdc`

```markdown
---
description: Database and migration rules - use @database to activate
globs:
alwaysApply: false
---

# Database Rules

- Table names: snake_case, plural (e.g. `partnership_hbls`)
- Always add index on foreign key columns
- Use `unsignedBigInteger` for foreign keys
- Nullable columns must have explicit `->nullable()`
- Never modify existing migrations; create new ones for changes
- Always add `down()` method for rollback support
```

---

## Rules 的效益

| 沒有 Rules | 有 Rules |
|-----------|---------|
| 每次要說「用繁體中文回答」 | 自動生效 |
| AI 可能用不同風格寫程式碼 | 統一遵循 PSR-12 |
| 要反覆解釋專案架構 | AI 已知 Service-Repository pattern |
| Commit message 格式不一致 | 自動遵循 conventional commit |
| 測試風格每次不同 | 統一的測試命名和結構 |

---
# Cursor 的 Rules、Skills 與 Subagents 詳解

---

## 1. Rules（規則）

**什麼是 Rules：** 規則是持久化的指令，讓 AI 在每次對話中都自動遵循你設定的慣例、風格和約束。規則存放在 `.cursor/rules/` 資料夾中。

**運作方式：**
- 規則會被自動注入到 AI 的 system prompt 中
- 可以設定為「全域」或「僅針對特定檔案/資料夾」生效
- 支援 `globs` 欄位來指定適用的檔案模式

**規則類型：**

| 類型 | 說明 |
|------|------|
| **Always** | 每次對話都會載入 |
| **Auto Attached** | 當匹配到特定檔案 glob 時自動載入 |
| **Agent Requested** | AI 根據描述判斷是否需要載入 |
| **Manual** | 使用者用 `@ruleName` 手動引用時才載入 |

### 範例：建立一個 Python 專案規則

檔案路徑：`.cursor/rules/python-style.mdc`

```markdown
---
description: Python coding conventions for this project
globs: "**/*.py"
alwaysApply: false
---

# Python 規範

- 使用 Python 3.11+
- 所有函式都要加 type hints
- 使用 Google style docstrings
- 使用 black 格式化，行寬 88
- import 順序：stdlib → third-party → local，用空行分隔
- 變數命名用 snake_case，類別命名用 PascalCase
- 不要用 print()，改用 logging module
- 所有 API endpoint 都要有錯誤處理
```

### 範例：建立一個 React 元件規則

檔案路徑：`.cursor/rules/react-components.mdc`

```markdown
---
description: React component conventions
globs: "src/components/**/*.tsx"
alwaysApply: false
---

# React 元件規範

- 使用 functional components + hooks，不用 class components
- Props 用 interface 定義，命名為 {ComponentName}Props
- 使用 named export，不用 default export
- 樣式用 Tailwind CSS utility classes
- 每個元件檔案只放一個元件
- 事件處理函式命名用 handle 前綴，如 handleClick、handleSubmit
```

### 範例：全域規則

檔案路徑：`.cursor/rules/global.mdc`

```markdown
---
description: Global project rules
alwaysApply: true
---

- 這是一個 monorepo，前端在 /frontend，後端在 /backend
- 後端用 FastAPI + SQLAlchemy + PostgreSQL
- 前端用 Next.js 14 (App Router) + TypeScript
- 所有 commit message 用英文，遵循 Conventional Commits
- 回答時使用繁體中文
```

---

## 2. Skills（技能）

**什麼是 Skills：** Skills 是比 Rules 更進階的指令集，存放在 `SKILL.md` 檔案中。它們是一套「操作流程指南」，當特定任務觸發時，AI 會讀取並按照指南一步步執行。

**與 Rules 的差異：**

| | Rules | Skills |
|---|---|---|
| **本質** | 靜態的約束/風格指令 | 動態的操作流程/SOP |
| **何時載入** | 自動或手動 attach | AI 判斷相關時主動讀取 |
| **內容** | 簡短的規範條列 | 詳細的步驟指南，可含決策樹 |
| **用途** | 編碼風格、命名慣例 | 建立專案、部署流程、特定任務 |

**運作方式：**
1. Skills 會在 `<agent_skills>` 區塊中被列出，帶有簡短描述
2. 當使用者的請求匹配到某個 skill 的描述時，AI 會先用 `Read` 工具讀取 `SKILL.md`
3. 然後按照裡面的指示逐步執行

### 範例：建立一個「建立 API Endpoint」的 Skill

檔案路徑：`.cursor/skills/create-api-endpoint/SKILL.md`

```markdown
# Create API Endpoint Skill

## 觸發條件
當使用者要求建立新的 API endpoint 時使用此技能。

## 步驟

### Step 1: 收集資訊
詢問使用者以下資訊（如果尚未提供）：
- Endpoint 路徑（如 /api/users）
- HTTP method（GET/POST/PUT/DELETE）
- 請求參數/body schema
- 回應格式

### Step 2: 建立 Schema
在 `backend/schemas/` 建立對應的 Pydantic model：
- Request model: `{Resource}Request`
- Response model: `{Resource}Response`

### Step 3: 建立 Service Layer
在 `backend/services/` 建立業務邏輯：
- 檔名: `{resource}_service.py`
- 包含錯誤處理和 logging

### Step 4: 建立 Router
在 `backend/routers/` 建立路由：
- 使用 FastAPI APIRouter
- 加上適當的 status code 和 response_model
- 加上 OpenAPI 文件描述

### Step 5: 註冊 Router
在 `backend/main.py` 中 include 新的 router

### Step 6: 建立測試
在 `backend/tests/` 建立對應測試：
- 成功案例
- 驗證失敗案例
- 404/權限錯誤案例
```

### 範例：建立一個「資料庫 Migration」的 Skill

檔案路徑：`.cursor/skills/db-migration/SKILL.md`

```markdown
# Database Migration Skill

## 觸發條件
當需要修改資料庫 schema（新增 table、修改欄位等）時使用。

## 步驟

1. 先修改 `backend/models/` 中對應的 SQLAlchemy model
2. 執行 `alembic revision --autogenerate -m "描述"` 產生 migration
3. 檢查產生的 migration 檔案是否正確
4. 執行 `alembic upgrade head` 套用變更
5. 更新對應的 Pydantic schema
6. 更新受影響的 service 和 router
7. 更新測試

## 注意事項
- 永遠不要手動修改 alembic_versions table
- 刪除欄位前確認是否需要資料遷移
- 加 NOT NULL 欄位時必須提供 default 值
```

---

## 3. Subagents（子代理）

**什麼是 Subagents：** Subagents 是 AI 可以啟動的「子任務代理」。主 Agent 可以把複雜任務拆分，派給多個 Subagent 平行或串行執行，每個 Subagent 有自己獨立的 context 和專長。

**可用的 Subagent 類型：**

| 類型 | 用途 |
|------|------|
| **`explore`** | 快速探索 codebase，搜尋檔案、關鍵字、理解結構 |
| **`generalPurpose`** | 通用型，適合多步驟研究和複雜搜尋 |
| **`shell`** | 專門執行終端指令（git、npm、docker 等） |
| **`browser-use`** | 瀏覽器操作，測試 web app、截圖、表單互動 |
| **`best-of-n-runner`** | 在隔離的 git worktree 中嘗試方案，適合平行實驗 |

**運作方式：**
1. 主 Agent 分析任務，決定是否需要拆分
2. 啟動一個或多個 Subagent，給予明確的任務描述
3. Subagent 獨立執行，完成後回報結果
4. 主 Agent 整合結果，繼續後續工作

### 範例情境 1：探索陌生 Codebase

使用者 Prompt：
```
這個專案的認證系統是怎麼實作的？
```

AI 的內部行為（使用者看不到）：

```
啟動 explore subagent:
  prompt: "搜尋所有與 auth、authentication、login、JWT、
           session 相關的檔案。找出認證 middleware、
           login endpoint、token 驗證邏輯的位置。
           回報檔案路徑和關鍵程式碼片段。"
  thoroughness: "very thorough"
```

Subagent 會自動搜尋整個 codebase，回報所有相關檔案和邏輯。

### 範例情境 2：平行處理多個獨立任務

使用者 Prompt：
```
幫我同時檢查前端和後端的程式碼結構
```

AI 的內部行為（同時啟動兩個 subagent）：

```
Subagent A (explore):
  prompt: "探索 /frontend 資料夾，列出元件結構、
           路由配置、狀態管理方式、API 呼叫模式。"

Subagent B (explore):
  prompt: "探索 /backend 資料夾，列出 API endpoints、
           資料庫 models、middleware、服務架構。"
```

兩個 Subagent 平行執行，各自回報結果，主 Agent 整合後呈現。

### 範例情境 3：隔離實驗（Best-of-N）

使用者 Prompt：
```
這個效能問題有幾種解法，幫我都試試看
```

AI 的內部行為：

```
Subagent A (best-of-n-runner):
  prompt: "在隔離的 worktree 中，用 React.memo + useMemo
           優化 UserList 元件的效能。執行 benchmark。"

Subagent B (best-of-n-runner):
  prompt: "在隔離的 worktree 中，用 virtualization
          （react-window）優化 UserList。執行 benchmark。"
```

每個 Subagent 在自己的 git branch 上獨立實驗，不互相影響，最後比較結果。

### 範例情境 4：瀏覽器測試

使用者 Prompt：
```
幫我測試 login 頁面的表單驗證是否正常
```

AI 的內部行為：

```
Subagent (browser-use):
  prompt: "1. 開啟 http://localhost:3000/login
           2. 不填任何欄位直接按 Submit，確認出現錯誤提示
           3. 填入無效 email 'abc'，確認出現格式錯誤
           4. 填入正確 email 但密碼太短，確認出現密碼長度提示
           5. 填入正確的帳密，確認成功跳轉
           截圖並回報每個步驟的結果。"
```

---

## 三者的關係與協作

```
┌─────────────────────────────────────────────┐
│                  Cursor AI                   │
│                                              │
│  ┌─────────┐   載入風格約束                    │
│  │  Rules   │ ──────────────► AI 行為準則      │
│  └─────────┘                                 │
│                                              │
│  ┌─────────┐   讀取操作指南                    │
│  │  Skills  │ ──────────────► 標準化流程執行    │
│  └─────────┘                                 │
│                                              │
│  ┌──────────┐  拆分任務平行處理                 │
│  │Subagents │ ──────────────► 高效能執行複雜任務│
│  └──────────┘                                │
└─────────────────────────────────────────────┘
```

**簡單比喻：**
- **Rules** = 公司的「員工手冊」— 永遠要遵守的規範
- **Skills** = 公司的「SOP 文件」— 特定任務的標準作業流程
- **Subagents** = 「團隊成員」— 可以分工合作、平行處理不同工作