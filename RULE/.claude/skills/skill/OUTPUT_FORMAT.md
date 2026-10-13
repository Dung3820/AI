# Test Case Output Format

## File Structure

### 1.1 Sheets
Mỗi file Excel có **2 sheets**: `GUI` và `FUNCTION`

### 1.2 Column structure (A–W, 23 columns)

> [!IMPORTANT]
> Đây là danh sách cột DUY NHẤT được phép dùng. **KHÔNG thêm** `Pre-condition`, `Type`, `No.`, `Status`, `Test Data` thành cột riêng.

| Col | Header row 5 | Header row 6 | Nội dung |
|-----|-------------|-------------|----------|
| A | `Test case ID` (merge A5:C5) | `L1` | Level 1 ID |
| B | _(merge)_ | `L2` | Level 2 ID |
| C | _(merge)_ | `L3` | Level 3 ID |
| D | `Roles` | _(blank)_ | Role hoặc blank — **KHÔNG PHẢI Pre-condition** |
| E | `Test Title/Summary of test cases` | _(blank)_ | Tiêu đề TC / tên section |
| F | `Test Steps` | _(blank)_ | Steps đánh số |
| G | `Expected Results` | _(blank)_ | Expected đánh số |
| H | `Test Priority` | _(blank)_ | High / Medium / Low |
| I | `Confirmation Data` | _(blank)_ | Test data cụ thể |
| J–M | `Round1` / `Chrome` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới |
| N–Q | `Round1` / `Safari` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới |
| R–U | `Round2` / `Chrome` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới |
| V–Y | `Round2` / `Safari` | Assignee · Execution Date · Test Result · BugID | Để trống khi sinh mới |

### 1.3 Header block (rows 1–6), data starts at row 7

| Row | Col A–C | Col D–E | Col F–H | Col I+ |
|-----|---------|---------|---------|--------|
| 1 | `System Name` | `BTS Groupware` | `Document Name` · giá trị | Stats block Chrome |
| 2 | `Phase` | `Integration testing` | `Screen Name` · giá trị | Stats block Safari |
| 3 | `Created By` | tên | `Created Date` · ngày | Stats block Total |
| 4 | `Updated By` | tên | `Updated Date` · ngày | |
| 5 | `Test case ID` (merge A5:C5) | `Roles` | `Test Title/Summary of test cases` · `Test Steps` · `Expected Results` · `Test Priority` · `Confirmation Data` | Round headers (vàng) |
| 6 | `L1` · `L2` · `L3` | _(blank)_ | _(blank)_ | Sub-headers: Assignee · Execution Date · Test Result · BugID |

**Merged cells:**
- A1:C1 → A4:C4, A5:C5
- D1:E1 → D4:E4
- G1:H1 → G4:H4
- D5:D6, E5:F6, G5:G6, H5:H6, I5:I6
- J5:K5, L5:M5, N5:O5, P5:Q5
- R5:S5, T5:U5, V5:W5

**Numbering rules:**
- Feature letters tăng A, B, C... (mỗi feature/screen mới)
- L1 tăng theo SEC trong 1 FEAT, reset khi qua FEAT mới
- L2 tăng trong L1, reset về 1 khi L1 đổi
- L3 reset về 1 khi L2 đổi

**TC reference format:** `[FeatureLetter].[L1]-[L2]-[L3]` — ví dụ: `A.2-1-3`

**Pre-condition text format:**
```
Pre-condition:
- User is logged into the system
- User has navigated to [Screen]
```

**Colors — màu nền theo row type:**

> [!IMPORTANT]
> **Quy tắc cột A–C (BẮT BUỘC):** Cột A, B, C luôn là `CCFFCC` cho **mọi row type** (FEAT, PRE, SEC, SUB, TC). Màu row type chỉ áp dụng từ **cột D trở đi**.

| Row Type | Cột A–C | Cột D–Y | Font |
|----------|---------|---------|------|
| Header rows 1–6 | `1F3864` (navy) | `1F3864` (navy) | `FFFFFF` trắng |
| FEAT | `CCFFCC` | `FFCC99` (cam) | đen, **bold** |
| PRE | `CCFFCC` | `FFFFFF` (trắng) | đen |
| SEC | `CCFFCC` | `99CCFF` (xanh dương nhạt) | đen, **bold** |
| SUB | `CCFFCC` | `FFF2CC` (vàng nhạt) | đen, **bold** |
| TC | `CCFFCC` | `FFFFFF` (trắng) | đen |

> [!IMPORTANT]
> **Quy tắc FEAT/SEC/SUB row — hiển thị 1 dòng duy nhất (BẮT BUỘC):**
> - FEAT, SEC, SUB rows: **KHÔNG wrap text** — title phải nằm trên 1 dòng duy nhất.
> - Merge cột D đến Y thành 1 ô để title không bị cắt.
> - TC rows và PRE rows: giữ wrap text bình thường (KHÔNG merge).
> - ❌ SAI: để wrap_text=True cho FEAT/SEC/SUB → title bị xuống dòng
> - ✅ ĐÚNG: merge D:Y + wrap_text=False cho FEAT/SEC/SUB

**Code snippet Python:**
```python
C = {
    'hdr':   '1F3864',  # Header rows 1-6 — fill navy, font trắng
    'feat':  'FFCC99',  # FEAT cols D-Y — cam
    'pre':   'FFFFFF',  # PRE cols D-Y — trắng
    'sec':   '99CCFF',  # SEC cols D-Y — xanh dương nhạt
    'sub':   'FFF2CC',  # SUB cols D-Y — vàng nhạt
    'tc':    'FFFFFF',  # TC cols D-Y — trắng
    'tc_id': 'CCFFCC',  # Cols A-C tất cả row types — xanh lá
}
# Cols A-C: luôn tc_id (CCFFCC) cho mọi data row (row 7+)
# Cols D-Y: màu theo row type
# Font FEAT/SEC/SUB: bold=True
# Font PRE/TC: bold=False
# Font header: FFFFFF trắng
```

> [!IMPORTANT]
> - ❌ SAI: tô cột A-C theo màu row type (cam/xanh/vàng) — cột A-C luôn phải là `CCFFCC`
> - ❌ SAI: dùng màu dark navy cho data rows — navy chỉ dành cho header rows 1–6
> - ❌ SAI: FEAT/SEC/SUB không bold — 3 row type này bắt buộc bold

**Stats block (rows 1–3, cols I+):** để trống khi sinh file mới.

### 1.4 Row types

| Row Type | Col A (L1) | Col B (L2) | Col C (L3) | Col D | Col E (Test Title) |
|----------|-----------|-----------|-----------|------|-------------------|
| Feature group header (FEAT) | `A`/`B`/`C`... | `-` | `-` | **Feature name + DTOF code** | blank |
| Pre-condition (GUI) | số | blank | blank | blank | **Pre-condition text** |
| Pre-condition (FUNCTION) | `-` | `-` | `-` | blank | **Pre-condition text** |
| Section header (SEC) | số | `-` | `-` | **Section title** | blank |
| Sub-section header (SUB) | số | số | `-` | **Sub-section title** | blank |
| Individual TC | số | số | số | Role hoặc blank | **Test case title** |

> [!IMPORTANT]
> - **SEC và SUB đặt title ở col D**, KHÔNG phải col E
> - Col E chỉ dùng cho PRE text và TC title
> - L2/L3 "không áp dụng" dùng ký tự **`-`** (dash), KHÔNG để blank
> - **GUI:** L2 tiếp nối qua các SUB trong cùng FEAT, KHÔNG reset khi L1 đổi
> - **FUNCTION:** L2 reset về 1 khi L1 đổi

> [!WARNING]
> **KHÔNG được tạo cột riêng cho Pre-condition.** Pre-condition luôn đặt ở row PRE, **cột E**. Cột D là title cho SEC/SUB, không phải `Pre-condition`.

---

## GUI Sheet Structure

**Cấu trúc tổng quan:**
```
A — [Screen/Feature Name] (DTOF-XX)
├── Pre-condition
├── 1. Check Title (BẮT BUỘC cho mọi màn hình)
│   └── TC 1-1-1: Check title — Observe title display — Display title [Screen Name]
├── 2. [Section name theo UI components]
│   ├── 2.1 [Sub-section: ví dụ Confirm display of columns]
│   │   ├── TC 2-1-1: Verify column [X] is displayed
│   │   ├── TC 2-1-2: Verify column [Y] is displayed
│   │   └── TC 2-1-N...
│   ├── 2.2 [Sub-section: ví dụ Confirm display of buttons]
│   │   └── ...
│   └── 2.3 [Sub-section khác]
├── 3. [Section name khác]
│   └── ...
└── ...

B — [Screen/Feature Name] (DTOF-XX)
└── ...
```

### 1. Check Title (BẮT BUỘC)

**Có trong mọi màn hình/screen.** Check title hiển thị đúng.

> [!IMPORTANT]
> **Quy tắc đánh số SEC trong GUI sheet:**
> - Check Title **luôn bắt buộc** nhưng **L1 số đếm bắt đầu từ 2**, không từ 1.
> - Lý do: SEC 1 được dành cho dữ liệu chung đã có sẵn (shared across screens), không sinh lại trong từng module.
> - Cấu trúc chuẩn:
>   ```
>   FEAT A
>   ├── PRE
>   ├── SEC L1=2: Check title        ← BẮT BUỘC, nhưng đếm từ 2
>   ├── SEC L1=3: [UI Component 1]
>   ├── SEC L1=4: [UI Component 2]
>   └── ...
>   ```
> - ❌ SAI: SEC L1=1 Check title (số 1 đã được dùng cho dữ liệu chung)
> - ✅ ĐÚNG: SEC L1=2 Check title

### UI Components (Section chia theo loại element)

**Chia theo UI components, KHÔNG theo AC:**

| UI Component | Check nội dung |
|--------------|----------------|
| **Columns/Fields** | Tên cột đúng, đủ cột, thứ tự đúng |
| **Buttons** | Hiển thị, enabled/disabled theo role |
| **Input Fields** | Hiển thị đủ, empty/enabled state |
| **Dropdowns** | Hiển thị đủ options |
| **Tables/Lists** | Dữ liệu, pagination, sort order |
| **Checkboxes/Radios** | Trạng thái mặc định |
| **Status Indicators** | Unread/read, attachment icon... |
| **Pagination** | Page size, next/prev buttons |

**Mỗi component = 1 sub-section riêng (L1.L2):**

```
1. Confirm display of Inbox list columns
   └── TC 1-1-1: Verify column [Sender Name / Email] is displayed
   └── TC 1-1-2: Verify column [Subject Line] is displayed

2. Confirm display of Inbox action buttons / controls
   └── TC 2-2-1: Verify [Refresh] button is displayed
   └── TC 2-2-2: Verify pagination controls are displayed
```

**Priority: Medium**

> [!IMPORTANT]
> **Quy tắc Priority trong GUI sheet:**
> - Tất cả TC trong GUI sheet đều dùng priority **Medium** — không dùng High hay Low.
> - Lý do: GUI check là kiểm tra hiển thị, không phải business logic critical.

### Quy tắc viết Steps trong GUI (BẮT BUỘC)

Mỗi TC check 1 element cụ thể → Steps và Expected phải nói rõ element đó, KHÔNG được viết chung chung.

| ❌ SAI — chung chung | ✅ ĐÚNG — cụ thể element |
|---------------------|--------------------------|
| `1. Observe the column headers of the inbox list` (dùng cho mọi column TC) | `1. Observe the [Sender Name / Email] column in the inbox list` |
| `1. Observe the available buttons` | `1. Observe the [Refresh] button in the inbox toolbar` |
| `1. Check the screen` | `1. Observe the pagination controls at the bottom of the inbox list` |
| Expected: `Column is displayed` | Expected: `Column 'Sender Name / Email' is displayed and visible in the email list` |

**Pattern chuẩn cho GUI TC:**
```
TC: Verify [Specific Element] is displayed
  Steps:   1. Observe the [Specific Element] in [exact location on screen]
  Expected: 1. [Specific Element] is displayed and visible / enabled / [exact state]
```

Ví dụ thực tế từ template:
```
TC: Verify column [Sender Name / Email] is displayed in the inbox list
  Steps:   1. Observe the [Sender Name / Email] column in the inbox list
  Expected: Column 'Sender Name / Email' is displayed and visible in the email list

TC: Verify [Refresh] button is displayed in the Inbox
  Steps:   1. Observe the controls available at the top of the inbox list
  Expected: A [Refresh] button (or equivalent control) is visible in the Inbox toolbar

TC: Verify attachment indicator is displayed for emails with attachments
  Steps:   1. Observe an email row that contains an attachment
  Expected: An attachment icon or indicator is visible on the email row that has attachment(s)
```

> [!IMPORTANT]
> **Ranh giới GUI vs FUNCTION (NGHIÊM CẤM vi phạm):**
>
> GUI sheet chỉ check **trạng thái hiển thị tĩnh** của element — KHÔNG check behavior, KHÔNG check logic.
>
> | ✅ ĐÚNG — GUI check | ❌ SAI — phải chuyển sang FUNCTION |
> |---|---|
> | Verify search box is displayed | Verify search returns matching results |
> | Verify search box is enabled | Verify search is case-insensitive |
> | Verify button is visible | Verify clicking button navigates to form |
> | Verify dropdown is displayed | Verify dropdown shows correct options from system |
> | Verify field is enabled | Verify field accepts valid input |
> | Verify toggle is visible | Verify toggle triggers conditional behavior |
>
> **Rule cứng:**
> - Mỗi TC trong GUI chỉ cần **Observe** → **Is displayed / Is visible / Is enabled / Shows [default state]**
> - Nếu Expected Result cần nhập dữ liệu, click, hoặc mô tả hệ thống xử lý → **đó là FUNCTION case, không phải GUI**
> - Input field trong GUI: chỉ check displayed + enabled. KHÔNG check accept/reject input, KHÔNG check validation.
> - Dropdown trong GUI: chỉ check displayed. KHÔNG check options list, KHÔNG check select behavior.
> - Button trong GUI: chỉ check displayed + enabled/disabled state. KHÔNG check navigation hay action khi click.

---

## FUNCTION Sheet Structure

**Cấu trúc tổng quan:**
```
A — [Feature Name] (DTOF-XX)
├── Pre-condition
├── 1. DTOF-XX-AC01 — [AC Name]
│   ├── Sub-section: Confirm [behavior/condition]
│   │   ├── TC 1-1-1: Happy path
│   │   ├── TC 1-1-2: Edge case
│   │   └── TC 1-1-N: Negative cases
│   ├── Sub-section khác (nếu có nhiều conditions)
│   └── ...
├── 2. DTOF-XX-AC02 — [AC Name]
│   ├── Sub-section...
│   └── ...
├── 3. DTOF-XX-AC03 — [AC Name]
│   └── ...
├── ...
├── N. Check validation (nếu có fields chưa cover)
│   ├── [Field Name] field validation
│   │   ├── TC N-1-1: Valid case
│   │   ├── TC N-1-2: Invalid cases (theo nhóm)
│   │   └── TC N-1-N: Edge cases
│   ├── [Field Name 2] field validation
│   └── ...

B — [Feature Name] (DTOF-XX)
└── ...
```

### Pre-condition

**Đặt sau feature header, trước AC đầu tiên.**

Format:
```
Pre-condition:
- User is logged into the system
- User has navigated to [Screen/Module]
```

### AC Sections

**Mỗi AC = 1 L1 section. Format header:**
```
[L1] DTOF-XX-AC0[N] — [AC Name]
```

**Thứ tự case trong sub-section:**
1. Happy path (luồng chính)
2. Alternate paths (luồng rẽ nhánh)
3. Negative cases
4. Edge cases (timeout, boundary...)

### Section 1 — Confirm initializing the screen

**Có khi feature là màn hình/modal/form riêng biệt.**
**Không có khi feature là component nhúng (Search, Pagination...).**

**Scope:** Check màn hình/form khởi tạo đúng.

### Check validation Section

**Tạo section DUY NHẤT cho tất cả fields chưa được validate trong AC.**

**Mỗi field = 1 sub-section riêng. Tất cả validation conditions của field đó = TCs trong cùng 1 SUB:**
```
N. Check validation
├── Name field validation          ← 1 SUB cho toàn bộ field Name
│   ├── TC N-1-1: Enter valid name
│   ├── TC N-1-2: Enter only spaces
│   ├── TC N-1-3: Enter special characters
│   └── TC N-1-4: Enter max length (255 chars)
├── Email field validation         ← 1 SUB cho toàn bộ field Email
│   ├── TC N-2-1: Enter valid email
│   ├── TC N-2-2: Enter email without @
│   └── ...
└── ...
```

> [!WARNING]
> **❌ NGHIÊM CẤM: Tách điều kiện thành SUB riêng**
>
> ```
> ❌ SAI — mỗi điều kiện = 1 SUB:
> ├── Name field - Required validation    ← SUB
> │   └── TC: Enter empty name
> ├── Name field - Max length validation  ← SUB (tách riêng = SAI)
> │   └── TC: Enter 256 chars
> ├── Name field - Special characters     ← SUB (tách riêng = SAI)
> │   └── TC: Enter <script>...
>
> ✅ ĐÚNG — toàn bộ điều kiện gộp trong 1 SUB:
> ├── Name field validation               ← 1 SUB duy nhất
> │   ├── TC N-1-1: Enter valid name
> │   ├── TC N-1-2: Enter empty (required check)
> │   ├── TC N-1-3: Enter 256 chars (max length)
> │   └── TC N-1-4: Enter <script>... (special chars)
> ```
>
> **Quy tắc:** SUB = Field. TC = Condition. Không bao giờ SUB = Condition.

**Validation cases theo nhóm (không test từng ký tự riêng lẻ):**

> [!IMPORTANT]
> **Nguyên tắc bắt buộc — Coverage 2 chiều:**
> Mỗi field PHẢI có đủ CẢ HAI phía:
> - **Valid case** (input đúng → hệ thống accept / save thành công)
> - **Invalid case** (input sai → hệ thống báo lỗi / reject)
>
> Nếu chỉ có invalid mà thiếu valid → không chứng minh được hệ thống chạy đúng.
> Nếu chỉ có valid mà thiếu invalid → không cover được error handling.
>
> **Áp dụng cho TẤT CẢ field types — không có ngoại lệ.**

| Field Type | Cases BẮT BUỘC (không được thiếu) |
|------------|---------------------|
| **Text (required)** | ✅ Valid input (nói rõ ký tự chấp nhận) · ❌ Invalid input (nói rõ ký tự bị từ chối) · ❌ Required (empty) · ❌ Whitespace only · ✅ Min length (1 ký tự hợp lệ) · ❌ Over max length · ✅ Exactly max length · ✅ Leading/trailing spaces trimmed |
| **Text (optional)** | ✅ Valid input · ❌ Invalid input · ✅ Empty/blank (accepted) · ✅ Min length · ❌ Over max length · ✅ Exactly max length · ✅ Whitespace only → treated as empty · ✅ Leading/trailing spaces trimmed |
| **Email** | ✅ Valid format · ❌ Required · ❌ Missing `@` · ❌ Missing domain · ❌ Invalid domain · ❌ Max length exceeded · ✅ Leading/trailing spaces trimmed · ❌ Duplicate (nếu field unique) |
| **Phone** | ✅ Valid format · ❌ Required · ❌ Non-numeric chars · ✅ Valid prefix (vd: +84, 0) · ❌ Invalid prefix · ❌ Min length · ❌ Max length exceeded · ✅ Leading/trailing spaces trimmed |
| **Date** | ✅ Valid format · ❌ Required · ❌ Invalid date (vd: 31/02) · ❌ Future date (nếu bị cấm) · ❌ Past date (nếu bị cấm) · ✅ Leap year (29/02) |
| **Number** | ✅ Valid value · ❌ Required · ❌ Invalid format (nhập chữ) · ✅ Min value · ❌ Below min · ✅ Max value · ❌ Above max · ❌ Negative · ❌ Zero (nếu bị cấm) · ❌ Decimal (nếu bị cấm) |
| **Dropdown (single, required)** | ❌ Required/empty shows error · ✅ All options displayed · ✅ Select a valid option · ✅ Change selection to different option |
| **Dropdown (single, optional)** | ✅ Default value shown · ✅ All options displayed · ✅ Select an option · ✅ Change selection · ✅ Clear/empty accepted |
| **Multi-select** | ✅ Empty/clear (accepted nếu optional) · ✅ Select 1 option · ✅ Select multiple options · ✅ Remove 1 option · ✅ Change selection (clear + reselect) |
| **Toggle / Radio** | ✅ Default state shown · ✅ Toggle on (behavior đúng) · ✅ Toggle off (behavior đúng) · ✅ Conditional behavior (nếu toggle trigger hiện/ẩn field khác: cả case show lẫn case hide) · ✅ Happy path no side-effect (nếu toggle có conflict rule: cần 1 case toggle on khi KHÔNG có conflict) |
| **File** | ✅ Valid type · ❌ Invalid type · ❌ Required · ❌ Max size exceeded · ❌ Empty file (0 KB) · ❌ Special chars trong tên file · ❌ Duplicate filename (nếu applicable) |

> **Ký hiệu:** ✅ = valid/positive case · ❌ = invalid/negative case
>
> **Rule đọc bảng:** Trước khi viết TCs cho 1 field, copy toàn bộ dòng tương ứng ra, tick từng case. Nếu thiếu 1 case → PHẢI thêm trước khi sang field khác.

---

## Writing Rules

### Test Steps
- Numbered list, bắt đầu bằng verb (Click, Enter, Navigate, Select, Observe...)
- Mỗi step = 1 hành động cụ thể

**Quy tắc Step 1 trong FUNCTION (BẮT BUỘC):**
- **TC khởi tạo** (TC đầu tiên của FEAT, thường là SEC 1, L1-1-1): viết steps đầy đủ từ đầu
  ```
  1. Log into the system
  2. Click on [Module] in the navigation
  3. Observe the screen
  ```
- **MỌI TC tiếp theo** trong cùng FEAT: Step 1 PHẢI là:
  ```
  1. Check immediately after TCs [FeatureLetter].1-1-1
  ```
  Ví dụ: Feature A → `1. Check immediately after TCs A.1-1-1`
  Ví dụ: Feature B → `1. Check immediately after TCs B.1-1-1`
- ❌ NGHIÊM CẤM repeat navigation/login steps ở TC không phải TC khởi tạo
- Expected của step 1 (`Check immediately after...`) không cần ghi expected riêng

**Ví dụ:**
```
1. Check immediately after TCs B.1-1-1
2. Click on any email in the inbox list
3. Observe the email reading view
```

### Expected Results
- Numbered list tương ứng với step number
- Step không có kết quả quan sát được → bỏ qua

**Format trong ô:** Mỗi ô chứa nhiều dòng, phân cách bằng line break (`\n` trong Excel)

| Col F - Test Steps | Col G - Expected Results |
|---------------------|--------------------------|
| 1. Navigate to Login screen<br>2. Enter email: `test@domain.com`<br>3. Click [Login] button<br>4. Observe error message | 1. Email field displays value<br>2. System validates input<br>3. Error message "Invalid format" appears |

→ Step 1 (setup) → bỏ expected. Steps 2, 3, 4 → expected đánh số 1, 2, 3 tương ứng.

**Phân loại step & Expected:**

| Step Type | Cần Expected? | Ví dụ |
|-----------|---------------|-------|
| **Setup/Navigate** | ❌ Không | "Navigate to Login screen", "Open application" |
| **Action** | ✅ Có | "Click Submit button", "Enter email", "Select option" |
| **Verify/Observe** | ✅ Có | "Check message display", "Confirm field value" |

- Tham chiếu Screen Layout nếu cần: `(Refer sheet Screen Layout - [Page Name])`

### Priority
- AC sections: **High**
- Check all items (Section 1 GUI): **Medium**
- Common cases: **Medium**

### Confirmation Data
- Giá trị cụ thể: email, số boundary, filename
- **KHÔNG vague**

### Language
- Test Title, Steps, Expected, Data: **tiếng Anh**
- Pre-condition: **tiếng Anh**

---

## Field-Level Validation Reference

| Loại Field | Validation cần test |
|------------|---------------------|
| **Text** | Required/Optional · Max length · Whitespace only · Special chars `<>&"'` · Leading/trailing spaces · XSS/SQL injection |
| **Email** | Valid format · Missing `@` · Missing domain · Multiple `@` · Max length · Case sensitivity · Duplicate |
| **Phone** | Numeric only · Valid prefix · Min/Max length · Non-numeric chars |
| **Date** | Valid format · Invalid date · Leap year · Future/past dates |
| **Number** | Min/Max value · Negative · Zero · Decimal · Overflow · Leading zeros |
| **Dropdown** | Default value · All options · Disabled option · Required (empty/clear) |
| **Checkbox/Radio** | Default state · Check/Uncheck · Required · Group behavior |
| **File Upload** | Valid type · Invalid type · Max size · Empty file · Special chars in name |
| **Password** | Min/Max length · Character requirements · Show/hide · Confirm match |
| **Textarea** | Max length · Line breaks · HTML tags · Character counter |

> **Nguyên tắc:** Mỗi field có checklist riêng. KHÔNG dùng chung validation cho tất cả fields.

---

## Anti-Patterns

### Cleanup Rule — Xóa file tạm

**Script Python sinh Excel KHÔNG xóa ở Bước 6 — chỉ xóa ở Bước 7.**

Lý do: Sau khi gen Excel xong, nếu cần sửa TC thì edit script + rerun nhanh hơn viết lại từ đầu.

| Loại file | Ví dụ | Bước 6 | Bước 7 |
|-----------|-------|--------|--------|
| **Script sinh test** | `gen_*.py` | GIỮ LẠI | Xóa |
| **CSV tạm** | `*.csv` | Xóa | — |
| **Markdown tạm** | `*.md` (trừ skill files, tc_*.md) | Xóa | — |
| **File debug/log** | `debug_*.txt`, `temp_*` | Xóa | — |

**Giữ lại vĩnh viễn:**
- File Excel đầu ra: `*_TestCases.xlsx`
- Skill files: `OUTPUT_FORMAT.md`, `TC_STRUCTURE_RULES.md`, `TEST_CASE_TEMPLATE.xlsx`
- TC source: `tc_[module].md`

---

### ❌ FUNCTION Anti-Patterns (NGHIÊM CẤM)

| Sai | Đúng |
|-----|------|
| Viết valid input TC nhưng không nói rõ ký tự nào được chấp nhận | Ghi cụ thể: "field accepts alphanumeric, space, and Vietnamese" |
| Viết invalid TC nhưng không nói rõ ký tự nào bị từ chối | Ghi cụ thể: "special characters @, ! are rejected" |
| Chỉ có invalid case, không có valid case cho 1 field | Mỗi field PHẢI có ít nhất 1 valid case (happy path) |
| Chỉ có valid case, không có invalid case cho 1 field | Mỗi field PHẢI có ít nhất 1 invalid/negative case |
| Toggle: chỉ có case trigger conflict, không có case happy path | Phải có case toggle on khi KHÔNG có conflict → save bình thường |
| Dropdown: chỉ có Required case, thiếu Select và Change | Dropdown required phải có: Required · All options · Select · Change |
| Multi-select: thiếu multi-select case | Multi-select phải có: Empty · Select 1 · Select multiple · Remove · Change |
| Viết TC search chỉ có valid case (kết quả tìm thấy) | Search phải có: found · not found (empty state) · clear → restore · edge cases |
| Text field optional: không có whitespace-only case | Optional text phải test whitespace-only → verify treated as empty |
| Bỏ qua min length vì "chỉ cần test max" | Phải có: min length (1 ký tự) + exactly max length + over max — cả 3 |

---

**Tất cả ô trong vùng dữ liệu (rows 1 đến row cuối, cols A–Y) phải có border thin 4 phía.**

```python
from openpyxl.styles import Border, Side
thin = Side(style="thin")
border = Border(left=thin, right=thin, top=thin, bottom=thin)
# Áp dụng cho mọi cell khi viết row
cell.border = border
```

**Quy tắc áp dụng:**
- Header rows 1–6: border thin toàn bộ A–Y
- FEAT / PRE / SEC / SUB rows: border thin toàn bộ A–Y
- TC rows: border thin toàn bộ A–Y (kể cả Round cols J–Y dù để trống)
- **KHÔNG bỏ border cho bất kỳ ô nào** — kể cả ô trống trong Round cols

---

### ❌ Column Anti-Patterns (NGHIÊM CẤM)

| Sai | Đúng |
|-----|------|
| Thêm cột `Pre-condition` (cột D) | Cột D = `Roles`. Pre-condition đặt ở row PRE cột E |
| Dùng cột `Type` và `No.` thay vì `L1/L2/L3` | Luôn dùng 3 cột L1, L2, L3 |
| Thêm cột `Test Data` riêng | Test data đặt ở cột I (`Confirmation Data`) |
| Thêm cột `Status` | Không có cột Status trong template |
| Cột Round dùng tên `R1..R14` | Dùng đúng format: Round1/Chrome, Round1/Safari, Round2/Chrome, Round2/Safari |