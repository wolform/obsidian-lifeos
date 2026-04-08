以下以 Laravel 專案為背景，提供實用的 prompt 範例與撰寫技巧。

---

## 1. 功能開發

### 差的 Prompt
> 幫我加一個新功能

### 好的 Prompt
> 在 `@BLNominateService.php` 新增一個 `cancelNomination(string $blNo): array` 方法，邏輯如下：
> 1. 透過 `PartnershipHBLRepository` 查詢該 BL 是否存在且狀態為 nominated
> 2. 呼叫 Logink API 發送取消請求
> 3. 更新 DB 狀態為 cancelled
> 4. 回傳操作結果陣列，格式與現有的 `nominateBL()` 回傳一致
> 
> 請參考現有的 `nominateBL()` 方法風格撰寫。

**為什麼好**：明確的方法簽名、逐步邏輯、參考範本、回傳格式要求。

---

## 2. Bug 修復

### 差的 Prompt
> 這個功能壞了，幫我修

### 好的 Prompt
> `@ScheduleLoginkShipmentEvents_full.php` 在處理 BL 事件時，當 API 回傳空陣列時會拋出 `Undefined index` 錯誤。
> 請加入 null check 防禦性處理，並在遇到空回傳時 log warning 後 continue，不要中斷整個排程。

**為什麼好**：精確描述問題、指出檔案、說明期望行為。

---

## 3. 重構

### 好的 Prompt
> `@BLNominateService.php` 和 `@AWBNominateService.php` 有大量重複邏輯（API 呼叫、錯誤處理、日誌記錄）。
> 請抽取一個共用的 `AbstractNominateService` base class 到 `app/Services/API/` 目錄下，將共用邏輯放入 base class，讓兩個 Service 繼承它。
> 保持現有的 public method 簽名不變，確保不影響外部呼叫。

---

## 4. 測試撰寫

### 好的 Prompt
> 為 `@BLNominateService.php` 的 `nominateBL()` 方法撰寫 PHPUnit 測試：
> - Mock `PartnershipHBLRepository` 和 `LoginkAPIBLNominate`
> - 測試案例：正常提名成功、BL 不存在、Logink API 回傳錯誤
> - 放在 `tests/Unit/Services/API/BLNominateServiceTest.php`
> - 遵循專案現有測試風格

---

## 5. 資料庫 / Migration

### 好的 Prompt
> 建立一個 migration，在 `partnership_hbl` 表新增以下欄位：
> - `cancelled_at` (nullable timestamp)
> - `cancel_reason` (nullable string, max 500)
> - `cancelled_by` (nullable unsigned bigint, foreign key 到 users.id)
> 
> 同時更新 `@PartnershipHBLRepository.php` 的 `$fillable` 和相關查詢方法。

---

## 6. 批次修改

### 好的 Prompt
> 專案中所有 `ExternalAPI/Logink/` 下的類別，目前的 HTTP timeout 是 hardcoded 30 秒。
> 請改為從 `config/global.php` 讀取 `logink.timeout` 設定值，並在 config 檔中新增此設定，預設值 30。

---

## 7. 除錯 / 分析

### 好的 Prompt
> `@SchedulePollToDataProvider.php` 排程執行時偶爾會跑超過 10 分鐘。
> 請分析程式碼中可能造成效能瓶頸的地方，特別是：
> - 是否有 N+1 查詢問題
> - 迴圈中是否有不必要的 API 呼叫
> - 提出具體的優化方案並實作

---

## Prompt 撰寫公式

```
[動作] + [目標檔案/範圍] + [具體需求] + [約束條件] + [參考範本]
```

| 元素 | 說明 | 範例 |
|------|------|------|
| **動作** | 要做什麼 | 新增、修改、重構、修復、刪除 |
| **目標** | 用 `@` 指定檔案 | `@BLNominateService.php` |
| **具體需求** | 步驟化描述 | 1. 查詢 → 2. 驗證 → 3. 儲存 |
| **約束條件** | 限制與要求 | 不改變現有 public API、使用現有 Repository |
| **參考範本** | 風格參考 | 「參考 `nominateBL()` 的寫法」 |

---

## 進階技巧

1. **分階段下指令**：大功能拆成多個 prompt，每步確認後再進行下一步
2. **善用 `@` 引用**：給 Agent 精確的上下文，避免它猜錯檔案
3. **明確說「不要」什麼**：例如「不要修改現有的 API route」、「不要動 migration」
4. **要求遵循現有模式**：「參考專案中現有的 Service 寫法」比從零描述風格有效得多
5. **一次一件事**：Agent 對單一明確任務的完成度遠高於模糊的大範圍指令

核心原則：**你描述得越精確，Agent 的輸出品質越高。** 把 Agent 當作一個很聰明但不了解你業務背景的新進工程師來溝通。