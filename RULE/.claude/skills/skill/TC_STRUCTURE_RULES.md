# TC Structure Rules

## 1. Hierarchy

```
FEAT → PRE → SEC → SUB → TC
```

| Row | Khi nào dùng |
|-----|-------------|
| `FEAT` | Mỗi feature/screen mới (A, B, C...) |
| `PRE`  | 1 lần sau FEAT, trước SEC đầu tiên |
| `SEC`  | Mỗi AC hoặc nhóm logic (L1) |
| `SUB`  | Nhóm nhỏ trong SEC (L1.L2) |
| `TC`   | Test case cụ thể (L1.L2.L3) |

---

## 2. JSON Schema

```json
{"type":"FEAT", "letter":"A", "title":"Screen Name (DTOF-XX)"}
{"type":"PRE",  "text":"Pre-condition:\n- User is logged in\n- User navigated to [Screen]"}
{"type":"SEC",  "l1":1, "title":"DTOF-XX-AC01 — AC Name"}
{"type":"SUB",  "l1":1, "l2":1, "title":"Confirm [behavior]"}
{"type":"TC",   "l1":1,"l2":1,"l3":1, "title":"...", "steps":"...", "expected":"...", "priority":"High", "data":""}
```

---

## 6. Excel Column Mapping (BẮT BUỘC — KHÔNG được thay đổi)

> [!IMPORTANT]
> Khi xuất Excel, ánh xạ từng trường vào **đúng cột** theo bảng dưới. Sai cột = file không dùng được.

| Cột | Tên header row 5 | Tên header row 6 | Nội dung | Ghi chú |
|-----|-----------------|-----------------|----------|---------|
| A | `Test case ID` (merge A5:C5) | `L1` | Level 1 ID | A, B, C... (FEAT) hoặc số (SEC/SUB/TC) |
| B | _(merge)_ | `L2` | Level 2 ID | |
| C | _(merge)_ | `L3` | Level 3 ID | |
| D | `Roles` | _(blank)_ | Tên role hoặc blank | **KHÔNG phải Pre-condition** |
| E | `Test Title/Summary of test cases` | _(blank)_ | Tiêu đề TC hoặc tên section/sub | |
| F | `Test Steps` | _(blank)_ | Steps đánh số | |
| G | `Expected Results` | _(blank)_ | Expected đánh số | |
| H | `Test Priority` | _(blank)_ | High / Medium / Low | |
| I | `Confirmation Data` | _(blank)_ | Test data cụ thể | |
| J–M | `Round1` / `Chrome` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới | |
| N–Q | `Round1` / `Safari` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới | |
| R–U | `Round2` / `Chrome` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới | |
| V–Y | `Round2` / `Safari` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới | |

**❌ NGHIÊM CẤM:**
- Thêm cột `Pre-condition` — KHÔNG tồn tại trong template. Pre-condition đặt ở row PRE, cột E.
- Đặt cột `Type` / `No.` / `Title/Scenario` — đây là format cũ sai, KHÔNG dùng.
- Đặt `Test Data` ở cột riêng — Test Data đặt ở cột I (`Confirmation Data`).
- Đặt `Status` ở bất kỳ cột nào — KHÔNG có cột Status.

**Row cho từng type:**

| Type | Col A (L1) | Col B (L2) | Col C (L3) | Col D (Roles) | Col E (Test Title) |
|------|-----------|-----------|-----------|--------------|-------------------|
| FEAT | `A`/`B`... | `-` | `-` | **Tên feature** | blank |
| PRE (GUI) | số | blank | blank | blank | **Pre-condition text** |
| PRE (FUNC) | `-` | `-` | `-` | blank | **Pre-condition text** |
| SEC  | số (1,2,3) | `-` | `-` | **Tên section** | blank |
| SUB  | số | số | `-` | **Tên sub-section** | blank |
| TC   | số | số | số | Role hoặc blank | **Tiêu đề TC** |

> [!IMPORTANT]
> **SEC và SUB đặt title ở col D (Roles), KHÔNG phải col E.**
> Col E chỉ dùng cho PRE và TC.
> L2/L3 "không áp dụng" dùng ký tự `-` (dash), KHÔNG để blank.

---

## 3. L1/L2/L3 Numbering

### Quy tắc chung

| Row | L1 | L2 | L3 |
|-----|----|----|-----|
| FEAT | `A`/`B`/`C`... | `-` | `-` |
| PRE (GUI) | số | blank | blank |
| PRE (FUNC) | `-` | `-` | `-` |
| SEC | tăng trong FEAT, reset khi qua FEAT mới | `-` | `-` |
| SUB | = L1 của SEC cha | xem rule riêng từng sheet bên dưới | `-` |
| TC | = L1 của SEC cha | = L2 của SUB cha | tăng, reset khi sang SUB mới |

### GUI sheet — L2 reset về 1 khi L1 đổi (giống FUNCTION)

> [!IMPORTANT]
> **Đã xác nhận từ thực tế:** GUI sheet cũng dùng rule giống FUNCTION — L2 reset về 1 mỗi khi L1 (SEC) đổi.

L2 của SUB **reset về 1** mỗi khi L1 (SEC) đổi trong cùng 1 FEAT.

```
FEAT A
  SEC L1=1  →  SUB L1=1, L2=1  →  TC 1-1-1, 1-1-2
  SEC L1=2  →  SUB L1=2, L2=1  →  TC 2-1-1, 2-1-2   ← L2 reset về 1 vì L1 đổi
              SUB L1=2, L2=2  →  TC 2-2-1, 2-2-2
  SEC L1=3  →  SUB L1=3, L2=1  →  TC 3-1-1
  SEC L1=4  →  SUB L1=4, L2=1  →  TC 4-1-1
  SEC L1=5  →  SUB L1=5, L2=1  →  TC 5-1-1

FEAT B  ← L1 reset về 1
  SEC L1=1  →  SUB L1=1, L2=1  →  TC 1-1-1
```

### FUNCTION sheet — L2 reset về 1 khi L1 đổi

L2 của SUB **reset về 1** mỗi khi L1 (SEC) đổi trong cùng 1 FEAT.

```
FEAT B
  SEC L1=1  →  SUB L1=1, L2=1  →  TC 1-1-1
  SEC L1=2  →  SUB L1=2, L2=1  →  TC 2-1-1, 2-1-2   ← L2 reset về 1 vì L1 đổi
              SUB L1=2, L2=2  →  TC 2-2-1, 2-2-2, 2-2-3
  SEC L1=3  →  SUB L1=3, L2=1  →  TC 3-1-1, 3-1-2
  SEC L1=4  →  SUB L1=4, L2=1  →  TC 4-1-1 ... 4-1-7

FEAT C  ← L1 reset về 1
  SEC L1=1  →  SUB L1=1, L2=1  →  TC 1-1-1
```

- TC reference: `[FeatureLetter].[L1]-[L2]-[L3]` (ví dụ: `A.2-3-1`, `B.2-2-3`)

---

## 4. GUI vs FUNCTION

**GUI sheet** — chia theo UI components (KHÔNG theo AC):
- SEC 1: Check all items on the screen (check names, positions, fonts, alignments)
- SEC 2+: Confirm display of specific components (columns / buttons / inputs / pagination...)
- Mỗi component type = 1 SUB riêng
- **L2 tiếp nối** qua các SUB trong cùng FEAT (không reset khi L1 đổi)

**Quy tắc viết Steps GUI:**
- Mỗi TC verify 1 element cụ thể → Step phải chỉ đích danh element đó
- ❌ SAI: `1. Observe the column headers` (dùng chung cho mọi TC column)
- ✅ ĐÚNG: `1. Observe the [Sender Name / Email] column in the inbox list`
- ❌ SAI: `Expected: Column is displayed`
- ✅ ĐÚNG: `Expected: Column 'Sender Name / Email' is displayed and visible in the email list`

**GUI — Quy tắc "Confirm display of input fields" (BẮT BUỘC):**
- Tất cả input fields trên 1 form/screen → gộp vào **1 SUB duy nhất** "Input fields displayed on [form name]"
- Mỗi field = 1 TC bên trong SUB đó
- ❌ SAI: mỗi field tạo 1 SUB riêng (Cost Center ID = SUB, Cost Center Name = SUB...)
- ✅ ĐÚNG: 1 SUB "Input fields displayed on create form" → TC 1: Cost Center ID, TC 2: Cost Center Name...

Tương tự với "Confirm display of columns", "Confirm display of buttons" — mỗi loại component = **1 SUB**, mỗi item = 1 TC.

---

**FUNCTION sheet** — chia theo AC:
- SEC 1: Confirm initializing/accessing screen — **TC đầu tiên (L1-1-1) là case khởi tạo** với steps đầy đủ
- SEC 2..N: DTOF-XX-AC0N — AC Name
- **Validation gộp vào đúng AC** — KHÔNG tách SEC "Field-Level Validation" riêng
- Thứ tự case trong SUB: Happy path → Alternate → Negative → Edge
- **L2 reset về 1** mỗi khi L1 đổi

**Quy tắc Field-Level Validation trong FUNCTION (BẮT BUỘC):**
- **KHÔNG tách SEC riêng** cho validation — gộp validation vào đúng SEC của AC quản lý field đó
- **1 SUB = 1 field**, chứa tất cả TCs: happy path + required + boundary + negative + edge
- ❌ SAI: sub "Template Name field" (happy path) tách rời sub "Template Name field validation"
- ✅ ĐÚNG: 1 SUB "Template Name" → TCs: valid input, required, max length, special chars, whitespace...

**Quy tắc Step 1 trong FUNCTION (BẮT BUỘC):**
- **TC khởi tạo** (`[FEAT].[L1]-1-1`): viết steps đầy đủ từ đầu (login → navigate → action)
- **MỌI TC tiếp theo** trong cùng FEAT: Step 1 BẮT BUỘC là:
  ```
  1. Check immediately after TCs [FeatureLetter].[L1 của TC khởi tạo]-1-1
  ```
  Ví dụ FEAT A → `1. Check immediately after TCs A.1-1-1`
  Ví dụ FEAT B → `1. Check immediately after TCs B.1-1-1`
- ❌ NGHIÊM CẤM repeat lại đầy đủ navigation steps ở các TC tiếp theo
- ❌ NGHIÊM CẤM để Step 1 là navigate/login ở TC không phải TC khởi tạo

**Quy tắc SUB cho từng field:**
- **SUB = Field**, **TC = Condition** — mỗi điều kiện validate = 1 TC trong SUB đó
- ❌ NGHIÊM CẤM: tạo sub riêng cho từng loại validation của cùng 1 field (Required sub, Max length sub...)

---

## 5. Writing Rules

**Steps:** verb đầu câu (Click, Enter, Navigate, Observe...) · mỗi step = 1 action

**Rule bắt buộc cho FUNCTION steps:**
- TC khởi tạo màn hình (`[FEAT].1-1-1`): steps đầy đủ từ đầu
- TC không phải khởi tạo: **Step 1 BẮT BUỘC** = `Check immediately after TCs [FeatureLetter].1-1-1`
- Sau step 1 là các steps thực hiện hành động cụ thể của TC đó
- Ví dụ đúng:
  ```
  Steps: 1. Check immediately after TCs A.1-1-1
         2. Observe the order of emails in the inbox list
  Expected: 2. Emails are sorted in descending order by date/time received
  ```

**Expected:** đánh số khớp step · bỏ qua Setup/Navigate steps · Action/Observe steps phải có expected

**Priority:** AC sections → High · GUI check-all → Medium · Common cases → Medium

**Data:** giá trị cụ thể, không placeholder (`CC_AUTO_1712049200`, không phải "valid ID")

**Language:** Steps, Expected, Title, Data → tiếng Anh
