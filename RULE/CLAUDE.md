# CLAUDE AI - MANUAL TESTING AGENT RULES

> **Scope:** Áp dụng cho mọi tác vụ Manual Testing do Claude (Claude Code) hoạt động trong dự án này.
> **Mục tiêu:** Hỗ trợ phân tích requirements, sinh test cases chất lượng cao, sinh test data, và quản lý kiểm thử thủ công.

---

## 1. Ngôn Ngữ & Giao Tiếp

- Luôn giao tiếp, giải thích ý tưởng và báo cáo bằng **Tiếng Việt**.
- Diễn giải **ngắn gọn, rõ ràng, dễ hiểu**.
- Tránh suy đoán hoặc giải thích mơ hồ — cần có căn cứ trực tiếp từ tài liệu hoặc UI thực tế.

---

## 2. Quy Trình Làm Việc (Workflow)

- **Recon (Điều tra):** Luôn đọc kỹ tài liệu requirements hoặc inspect giao diện thực tế trước khi sinh test cases. Tuyệt đối KHÔNG ĐOÁN business logic.
- **Analysis:** Phân tích requirements → phát hiện ambiguities → đặt câu hỏi clarify trước khi viết test.
- **Test Design:** Áp dụng kỹ thuật thiết kế test: Equivalence Partitioning, Boundary Value Analysis, Decision Table, State Transition.
- **Delivery:** Sinh test cases đầy đủ, có test data cụ thể, format chuẩn sẵn sàng copy sang Excel/Jira.

---

## 3. Browser Rules (Khi cần inspect UI)

### Viewport & Mode

* Tất cả **UI inspection** phải chạy với **desktop viewport**: **`1920x1080`**
* Bắt buộc **mở browser thật** khi inspect (headed mode)

### Thứ Tự Inspect Bắt Buộc (Playwright MCP)

Khi dùng Playwright MCP để inspect UI:

```
navigate → resize(1920×1080) → wait_for(page_load) → snapshot → phân tích
```

* **LUÔN** gọi `browser_resize(width=1920, height=1080)` ngay sau `browser_navigate`
* **LUÔN** verify page đã load xong trước khi lấy snapshot

---

## 4. Test Data

- Tất cả field yêu cầu **unique** (Email, Username, Code/ID): **BẮT BUỘC** dùng dữ liệu cụ thể, không placeholder.
- Dữ liệu phải **traceable** — nhìn vào biết ngay test nào sử dụng.
- Format khuyến nghị: kết hợp `test name + timestamp + prefix`.

Ví dụ:

```
email:    test_login_1712049200@auto.test
username: auto_user_1712049200
code:     TC_LOGIN_1712049200
```

---

## 5. Tham Chiếu Skills

Agent sử dụng skills tùy theo nhiệm vụ:

| Skill | Vai trò |
| --- | --- |
| `rbt_manual_testing` | Master skill cho manual testing — 2 modes: QUICK và FULL RBT (6 bước) |
| `requirements_analyzer` | Phân tích requirements từ website/tài liệu |
| `test_data_generator` | Sinh test data unique, traceable |
| `jira_integration` | Tích hợp Jira/Xray — lấy requirements, đẩy test results |

---

## 6. Tham Chiếu Workflows (Slash Commands)

| Workflow | Mô tả |
| --- | --- |
| `/generate_requirements_from_website` | Sinh requirements từ website/module |
| `/analyze_requirement_document` | Phân tích requirement document — sinh tài liệu phân tích, KHÔNG sinh TC |
| `/generate_manual_testcases_rbt` | Sinh manual test cases theo AI-RBT 6 bước (FULL RBT mode) |
| `/generate_testcases_from_requirements` | Sinh test cases nhanh từ requirements (QUICK mode) |
| `/update_module_from_requirement` | Update file module khi requirement thay đổi — so sánh diff, cập nhật ACs/fields/business rules, ghi changelog |
| `/generate_test_data` | Sinh test data có cấu trúc |
| `/generate_cross_module_test_plan` | Phân tích cross-module, sinh ma trận kết hợp (Pairwise) |
| `/generate_combinatorial_test_data` | Sinh test data cho ma trận kết hợp đa chiều |
| `/fetch_jira_requirements` | Lấy requirements/user stories từ Jira |

### Workflow `/update_module_from_requirement`

Trigger khi user nói: "requirement update rồi", "có thay đổi spec", "BA vừa cập nhật AC...", "cập nhật module..."

**Luồng thực hiện:**
1. Đọc `plans/manual/modules/_index.md` → xác định module tương ứng
2. Đọc file `module_[tên].md` hiện tại
3. Phân tích diff với requirement mới (AC nào thay đổi / thêm / xóa)
4. Update chỉ phần thay đổi, giữ nguyên phần còn lại
5. Ghi changelog vào file: date, version mới, nội dung thay đổi
6. Báo cáo: "Đã cập nhật AC02, thêm AC04, xóa AC03"
7. Hỏi: "Bạn có muốn gen lại TC cho phần thay đổi không?"

---

## 7. Kế Hoạch Kiểm Thử (Plan Templates)

Các bộ prompt template sẵn dùng trong `plans/`:

- **`plans/manual/`** — Quy trình sinh Manual Test Cases (2 modes: QUICK + FULL RBT)
  - Xem `plans/manual/QUICK_START.md` để bắt đầu nhanh
  - Workflow QUICK: `/generate_testcases_from_requirements`
  - Workflow FULL RBT: `/generate_manual_testcases_rbt`

- **`plans/manual/modules/`** — Living documentation cho từng module
  - `_index.md`: Index toàn bộ modules đã phân rã
  - `module_[tên].md`: Mỗi module 1 file — chứa ACs, fields, business rules, changelog
  - Dùng để gen TC trực tiếp (QUICK mode) và update khi requirement thay đổi

- **`plans/cross-module/`** — Quy trình phân tích Cross-Module & Ma trận kết hợp
  - Xem `plans/cross-module/QUICK_START.md` để bắt đầu nhanh
  - Workflow phân tích: `/generate_cross_module_test_plan`
  - Workflow sinh data: `/generate_combinatorial_test_data`

---

## 8. Module Files — Quy tắc sử dụng

**Mỗi module = 1 file `module_[tên].md`** trong `plans/manual/modules/`. Các features cùng module (Create/Edit/Delete) nằm trong cùng 1 file.

| Tình huống | Hành động |
| --- | --- |
| Phân rã xong Bước 3 (FULL RBT) | Tạo file module + update `_index.md` |
| Gen TC từ module đã có | Đọc `module_[tên].md` → QUICK mode, không cần paste requirements |
| Requirement thay đổi | Dùng `/update_module_from_requirement` → update file + ghi changelog |
| Không chắc module nào | Đọc `_index.md` trước để tìm đúng file |

**Rule bắt buộc:**
- KHÔNG tạo file module mới nếu đã tồn tại → update file hiện tại
- KHÔNG phân rã lại từ đầu nếu `_index.md` đã có module đó
- LUÔN ghi changelog khi update file module

---

## 9. Anti-Patterns (NGHIÊM CẤM)

| ❌ Anti-Pattern | ✅ Thay thế đúng |
| --- | --- |
| Đoán business logic không có căn cứ | Đọc requirements / inspect UI trước |
| Test data dùng placeholder ("email hợp lệ") | Giá trị cụ thể: `test_login_01@domain.com` |
| Gộp validation nhiều trường vào 1 TC | Mỗi trường có validation TCs riêng |
| Bỏ qua Negative / Boundary cases | Bắt buộc cover: Positive + Negative + Boundary + Edge |
| Sinh TC không có Expected Result rõ ràng | Mỗi TC phải có Expected Result cụ thể |
| Tự tính pairwise thủ công | Dùng script Python + `allpairspy` |

---

## 10. Checklist Bàn Giao Test Cases

- [ ] Test cases cover đủ: Happy Path, Negative, Boundary, Edge Cases
- [ ] Mỗi TC có: ID, Module, Title, Pre-condition, Steps, Test Data, Expected Result, Priority
- [ ] Test Data cụ thể (không placeholder)
- [ ] Mỗi input field có validation TCs riêng
- [ ] Format bảng Markdown chuẩn, sẵn sàng copy sang Excel/Jira
- [ ] Traceability: TC → Requirement mapping rõ ràng
- [ ] Ambiguities đã được liệt kê và giải quyết (hoặc ghi chú)

---

## Git Pull Restriction Rule

* Tuyệt đối KHÔNG dùng lệnh GIT làm thay đổi trạng thái code.
* **Được phép** dùng lệnh read-only: `git status`, `git diff`, `git log`.
