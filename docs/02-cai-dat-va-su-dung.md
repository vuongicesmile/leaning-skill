# Cài đặt và sử dụng

Các lệnh dưới đây mô tả đường cài trong snapshot đã đọc. Chúng chưa được chạy để thay đổi cấu hình máy trong quá trình viết tài liệu này. Bạn cần có quyền đọc repo nguồn; repo học tập này không chứa plugin để cài thay thế.

## 1. Chuẩn bị

Bootstrap kiểm tra Node.js >= 18, Python 3 và Git. Có Claude Code CLI thì script thử đăng ký marketplace và cài plugin. Node >= 18 là điều kiện script kiểm tra, chưa phải bằng chứng toàn bộ dependencies đều tương thích mọi phiên bản Node >= 18.

Nên bắt đầu trong một môi trường thử nghiệm và lưu bản cấu hình Claude hiện có trước khi chạy wizard. Lý do cụ thể: `installHooks()` gán `settings.hooks = hooks.hooks || hooks`, thay toàn bộ trường hooks; helper symlink có thể xóa mục trùng tên ở đích trước khi tạo link mới.

## 2. Cài từ repo nguồn

```bash
git clone https://github.com/frogasia/claude-skills ytlailabs-claude-skills
cd ytlailabs-claude-skills
git checkout 190776b25ef9a741a5edb73de655d21064ef212f
./scripts/bootstrap.sh
```

Pin commit để tái hiện bản được phân tích. Nếu dùng commit khác, hãy đối chiếu lại các lệnh và phát hiện trong tài liệu này.

Bootstrap chạy `npm install`, thử đăng ký marketplace local và cài plugin, rồi hỏi có mở wizard không. Nếu không cài được plugin bằng CLI, fallback chỉ symlink các command setup/doctor; không nên hiểu fallback là toàn bộ plugin đã hoạt động đầy đủ.

## 3. Chọn profile

| Lựa chọn trong wizard | Cách dùng |
| --- | --- |
| `ilmuchat` | Profile đầy đủ: rules, agents, skills dev/ops, hooks, commands, contexts |
| `custom` | Chọn từng nhóm; phù hợp khi chỉ muốn học workflow lập trình |
| `skills-dev` | Những skill không có classification `external` theo helper |
| `skills-ops` | Những skill có classification `external`, thường cần credentials |

Chọn `custom`, bật phần lập trình cần học trước. Màn hình env hiện quét thư mục skills của nguồn, nên có thể vẫn hỏi credentials của dịch vụ bạn chưa định dùng. Có thể bỏ qua cấu hình credentials để làm sau.

Chạy lại wizard từ thư mục clone:

```bash
node scripts/setup-wizard.js
```

Trong Claude Code, command được bộ nguồn giới thiệu là `/ytlailabs:setup`.

## 4. Xác minh từng lớp

Sau khi cài, mở lại phiên Claude Code nếu host cần tải lại plugin. Kiểm tra command được nhận diện, thử một workflow chỉ đọc, rồi mới thêm dịch vụ ngoài.

```text
/ytlailabs:plan Lập kế hoạch thêm tìm kiếm cho một ứng dụng ghi chú thử nghiệm.
```

Kỳ vọng theo nội dung command: AI trả kế hoạch, rủi ro, các bước và chờ bạn xác nhận trước khi sửa code. Đây là hành vi được chỉ dẫn, không phải kiểm thử đã chạy trên một phiên Claude Code thật.

Command chẩn đoán được giới thiệu là `/ytlailabs:doctor`. Nếu command không tìm được script, gọi trực tiếp từ thư mục clone:

```bash
node scripts/doctor.js --help
node scripts/doctor.js --no-drift
```

Doctor kiểm tra thư mục, biến môi trường và tùy chọn drift. Một báo cáo biến “đã có” chưa chứng minh token còn hiệu lực hay API truy cập được; cần thử request chỉ đọc phù hợp nếu muốn kiểm tra kết nối thật.

## 5. Các command để dùng hằng ngày

| Nhu cầu | Command trong plugin | Kết quả muốn có |
| --- | --- | --- |
| Lập kế hoạch | `/ytlailabs:plan` | Phạm vi, rủi ro, bước triển khai |
| Làm theo TDD | `/ytlailabs:tdd` | Test thất bại đúng lý do rồi sửa để pass |
| Review code | `/ytlailabs:code-review` | Nhận xét gắn với diff và mức độ ảnh hưởng |
| Xác minh | `/ytlailabs:verify` | Bằng chứng phù hợp thay đổi |
| Xem workload | `/ytlailabs:whatsonmyplate` | Ticket được nhóm theo trạng thái |
| Tìm Jira | `/ytlailabs:jira` | Kết quả theo yêu cầu/JQL |
| Thiết lập credentials | `/ytlailabs:env-setup` | Hướng dẫn cấu hình biến còn thiếu |

Namespace phụ thuộc đường cài. Plugin dùng tên `ytlailabs`; wizard còn symlink command không có prefix vào thư mục user. Đừng coi `/plan` luôn tương đương `/ytlailabs:plan` nếu đang cài nhiều bộ command.

## 6. Dịch vụ ngoài

Wizard tạo `~/.claude/.env` từ file example nếu chưa có, đặt quyền `0600`. Skill `whatsonmyplate` nêu ba tên biến: `JIRA_BASE_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN`. Nhập giá trị qua wizard hoặc cơ chế quản lý bí mật của môi trường bạn.

Ở snapshot này, skill Jira còn mô tả `.env` ở project root, trong khi workload skill dùng `~/.claude/.env` hoặc biến shell. Khi gặp lỗi thiếu biến, kiểm tra đoạn code thực thi đang nạp từ đâu; doctor kiểm tra một nơi chưa đảm bảo mọi workflow nạp cùng nơi đó.

Chỉ cấu hình dịch vụ cần dùng. Những workflow ghi dữ liệu, phát hành hoặc vận hành hạ tầng cần được đọc kỹ theo thao tác thật; classification là metadata chứ không phải một hàng rào quyền hạn.

## 7. Gỡ lỗi nhanh

| Hiện tượng | Kiểm tra đầu tiên |
| --- | --- |
| Command không tồn tại | Plugin đã được cài hay mới đăng ký marketplace? Đúng namespace và phiên đã tải lại chưa? |
| Skill không được chọn | Nêu rõ tên skill trong yêu cầu, kiểm tra đường cài và description |
| Symlink hỏng | Clone nguồn có bị đổi vị trí hoặc xóa không? |
| Hook không chạy | Sự kiện/matcher có khớp, profile có cho phép và script có tồn tại không? |
| Có cảnh báo nhưng thao tác vẫn chạy | Hook đó có chỉ log và trả pass-through không? Có chạy async không? |
| API trả 401/403 | Token, tài khoản, phạm vi và quyền dịch vụ thực tế |
| Không thấy ticket | Filter mặc định có đang giới hạn project/status không? |
| CLI được README nhắc nhưng không có file | Đối chiếu snapshot; `scripts/ytlailabs-skill` không tồn tại ở bản này |
