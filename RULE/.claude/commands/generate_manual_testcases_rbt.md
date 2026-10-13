---
description: Sinh manual test cases chất lượng cao theo quy trình AI-RBT 6 bước (Risk-Based Testing) từ requirements.
skills:
  - rbt_manual_testing
  - requirements_analyzer
---

> **BẮT BUỘC (MANDATORY SKILL):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`rbt_manual_testing`** (tại `.claude/skills/SKILL.md`) trước khi bắt đầu thực hiện tác vụ này. Sử dụng **Mode FULL RBT** của skill. Ngoài ra, tham khảo thêm skill **`requirements_analyzer`** để hiểu cách phân tích giao diện nếu cần.

# Workflow: Sinh Manual Test Cases theo AI-RBT Framework (FULL RBT Mode)

Workflow này sử dụng **Mode FULL RBT** của skill `rbt_manual_testing` — quy trình **AI-RBT (AI-Driven Risk-Based Testing)** gồm 6 bước tuần tự để sinh manual test cases từ tài liệu yêu cầu.

> [!NOTE]
> **Luồng này dành cho Antigravity (slash command).** Agent thực hiện theo hướng dẫn trong skill, KHÔNG cần đọc file prompt.txt.
> Nếu QA team muốn dùng prompt chi tiết hơn (ChatGPT/Claude), hãy copy-paste từng bước từ `plans/manual/01-06/prompt.txt`.

## ⚠️ Nguyên tắc thực thi

- **Mode:** FULL RBT (6 bước tuần tự)
- **BẮT BUỘC chạy tuần tự** từng bước, KHÔNG gộp nhiều bước
- **PHẢI dừng lại** chờ user phản hồi tại Bước 2 (Q&A) và Bước 4 (Review Scenarios)
- Nếu user chưa cung cấp requirements, hỏi user cung cấp trước khi bắt đầu
- Tất cả output bằng **Tiếng Việt**

## Các bước thực hiện

Thực hiện theo hướng dẫn chi tiết trong skill `rbt_manual_testing` → phần **Mode 2: FULL RBT**.

### Bước 1: Khởi tạo ngữ cảnh (Context & Role-play)
1. Yêu cầu user cung cấp: tên dự án, mô tả hệ thống, mục tiêu MVP, tài liệu yêu cầu
2. Đọc kỹ tài liệu, xác nhận hiểu bối cảnh
3. **Chờ user xác nhận** → sang Bước 2

### Bước 2: Phân tích yêu cầu (Analysis & QnA)
1. Xác định Happy Path, Alternate Paths, Exception Paths
2. Phát hiện Ambiguities (thiếu sót, mâu thuẫn, chưa rõ ràng)
3. Đặt câu hỏi Q&A có đánh số (Q1, Q2...) cho user/PO/BA, kèm ngữ cảnh + assumption
4. **DỪNG LẠI — Chờ user trả lời câu hỏi** → sang Bước 3

### Bước 3: Phân rã hệ thống (Decomposition)
1. Chia tính năng thành Modules / Sub-modules
2. Mô tả chức năng từng Module + Dependencies giữa chúng

### Bước 4: Đảm bảo độ bao phủ (Traceability)
1. Map Module → mã Yêu cầu (REQ-01, REQ-02...)
2. Cross-check thiếu sót (Gap Analysis), liệt kê High-Level Scenarios
3. **Chờ user review** scenarios → sang Bước 5

### Bước 5: Sinh Test Case chi tiết (RBT & TC Generation)
1. Đánh giá Risk Level (High/Medium/Low) cho mỗi Module
2. Sinh test cases đầy đủ: Title, Pre-condition, Steps, Expected, Test Data, Priority
3. Áp dụng kỹ thuật: EP, BVA, Decision Table, State Transition
4. **Validation chuyên biệt từng trường (Field-Level Validation):**
   - Liệt kê tất cả input fields trên form/UI đang test
   - Sinh validation TCs **riêng cho TỪNG trường** theo đặc tính riêng
   - Tham chiếu **Bảng Field-Level Validation** trong skill `rbt_manual_testing`
   - **KHÔNG** gộp validation nhiều trường vào 1 TC
5. Bao phủ đầy đủ: Happy Path, Negative, Boundary, Edge Cases
6. Test Data phải cụ thể (không placeholder chung)
7. Nếu quá nhiều → sinh từng Module, hỏi user để tiếp tục

### Bước 6: Chuẩn hóa Format (Template Mapping)
1. Đóng gói toàn bộ test cases vào bảng Markdown chuẩn:
   `| TC ID | Module | Risk Level | Test Title | Pre-Condition | Test Steps | Expected Result | Priority | Test Data |`
2. Không được bỏ sót test case nào
3. Xuất dưới dạng Artifact nếu dài

### Bước 7: Xoá file tạm 
- Sau khi xác nhận Excel OK → xóa file `.py` theo Cleanup Rule bên dưới


**Khi xuất ra file Python (.py) để gen Excel:**
- KHÔNG dùng `Write` cho toàn bộ script cùng lúc
- Dùng `Write` cho **50 dòng đầu tiên** (imports + helpers)
- Dùng `Edit` để append từng chunk 50 dòng tiếp theo cho đến hết
- Sau khi viết xong, chạy `python3 <tên_file>.py` để gen Excel

## Output

- Bảng Test Cases Markdown hoàn chỉnh, sẵn sàng copy sang Excel/Jira/TestRail => chuyển sang file excel
- Chuyển các file test case vừa sinh vào thưu mục là Test Case nếu chưa có thì tạo mới 
- viết testcase với ngôn ngữ là tiếng anh 
- Traceability Matrix
- Danh sách Ambiguities đã giải quyết

## ⚠️ Anti-Patterns — Cleanup Rule

**Thời điểm xóa file tạm:**

| Loại file | Ví dụ | Khi nào xóa |
|-----------|-------|-------------|
| **Script sinh test** | `generate_*.py`, `gen_*.py` | **Bước 7** — sau khi user confirm Excel OK |
| **CSV tạm** | `*.csv` (trừ file Excel đầu ra) | Ngay sau Bước 6 |
| **Markdown tạm** | `*.md` (trừ skill files, `tc_*.md`) | Ngay sau Bước 6 |
| **File debug/log** | `debug_*.txt`, `temp_*` | Ngay sau Bước 6 |

**Giữ lại:**
- ✅ File Excel đầu ra: `*_TestCases.xlsx`
- ✅ Skill files: `OUTPUT_FORMAT.md`, `TEST_CASE_TEMPLATE.xlsx`
- ✅ TC source: `tc_[module].md`
- ✅ Script `.py` — giữ đến khi user confirm Bước 7

**Xóa tại Bước 7 (sau khi user confirm Excel OK):**
```bash
cd [output_folder]
rm -f gen_*.py generate_*.py
```

**❌ Anti-pattern:** Tự xóa script `.py` ngay sau Bước 6 mà chưa được user xác nhận.
