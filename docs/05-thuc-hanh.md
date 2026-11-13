# Thực hành bằng dữ liệu giả

Các bài dưới đây là đề xuất học tập, không phải kết quả một phiên chạy thật. Dùng sandbox và đọc chỉ dẫn trước khi cho AI thao tác.

## Bài 1 — Theo dấu command trong 15 phút

Mở `commands/whatsonmyplate.md`, rồi `skills/whatsonmyplate/SKILL.md` trong repo nguồn.

```text
Giải thích command này chuyển tiếp sang skill nào.
Liệt kê biến môi trường, công cụ thực thi, loại request và định dạng kết quả.
Chỉ đọc code, không gọi API. Phân biệt điều code thực hiện với điều tài liệu hứa.
```

Bạn hoàn thành khi trả lời được: host nạp gì, AI quyết định gì, công cụ làm gì và dịch vụ ngoài trả gì.

## Bài 2 — Mô phỏng một lần dùng workload skill

```text
Dùng cách nhóm kết quả của whatsonmyplate trên dữ liệu giả sau:
DEMO-1: sửa tìm kiếm, To Do, High
DEMO-2: thêm unit test, In Progress, Medium
DEMO-3: review giao diện, In Review, Low
Không gọi Jira, không đọc credentials. Trả bảng theo status và tổng số.
```

Kỳ vọng: ba nhóm, mỗi nhóm một ticket, tổng ba. Không thêm ticket, sprint hay hạn chót chưa có trong dữ liệu. Bài này kiểm tra phần trình bày, không kiểm tra kết nối Jira.

## Bài 3 — Plan rồi mới triển khai

Trong một project sandbox:

```text
/ytlailabs:plan Thêm hàm tìm ghi chú theo tiêu đề, không phân biệt hoa thường.
Nêu tiêu chí cho chuỗi rỗng, Unicode và không có kết quả. Chưa sửa file.
```

Đọc kế hoạch rồi xác nhận phạm vi nếu muốn làm tiếp. Sau đó yêu cầu workflow TDD bằng command tương ứng. Kiểm tra báo cáo có nêu test đã chạy và lý do RED/GREEN thay vì chỉ nói “đã kiểm tra”.

Không dùng bài này để suy ra workflow nguồn phù hợp mọi loại thay đổi. Tài liệu, config nhỏ và code nghiệp vụ có cách xác minh khác nhau.

## Bài 4 — Đọc hook mà không thực thi

Mở registration của quality gate và hai file runtime được phân tích trong [workflow](03-mo-xe-workflow.md).

```text
Với ECC_HOOK_PROFILE=standard và một file Python vừa sửa:
1. Hook nào có thể được chọn?
2. Script sẽ thử gọi công cụ nào?
3. Nếu công cụ thiếu hoặc báo lỗi, code trả gì?
4. Hook là async hay sync? Có bằng chứng nó chặn thao tác không?
Không chạy script và không sửa file.
```

Đáp án cần dựa vào matcher, profile và return/exit code. Tên gọi “quality gate” riêng nó không đủ để kết luận.

## Bài 5 — Tự kiểm tra các phát biểu

| Phát biểu | Cách đối chiếu |
| --- | --- |
| “Có 51 command” | Đếm file `.md` trực tiếp trong `commands/` |
| “Setup merge hooks” | Đọc phép gán trong `installHooks()` |
| “Doctor pass là Jira hoạt động” | Kiểm tra doctor có gửi request xác thực không |
| “Stop tự tạo skill mới” | Tìm thao tác ghi `SKILL.md` trong evaluator |
| “Tất cả tích hợp đều qua MCP” | Đọc code thực thi của skill Jira/workload |

## Prompt dùng lại

```text
Phân tích skill <tên> tại commit <SHA>.
Giải thích: mục đích, điều kiện kích hoạt, dependencies, các bước,
tool được gọi, dữ liệu vào/ra, lỗi có thể gặp và bằng chứng hoàn thành.
Chỉ dẫn nguồn theo path và hàm. Đánh dấu phần chưa kiểm thử.
Không gọi dịch vụ ngoài khi chỉ phân tích.
```

Lộ trình gợi ý: ngày đầu đọc kiến trúc và command; ngày hai theo một request giả; ngày ba đọc hook; ngày bốn chạy một thay đổi nhỏ trong sandbox; ngày năm ghi lại điều tài liệu mô tả khác với hành vi quan sát.
