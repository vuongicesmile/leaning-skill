# Bằng chứng, phát hiện và giới hạn

## Snapshot phân tích

Repo nguồn local khai báo remote `git@github.com:frogasia/claude-skills.git`, commit `190776b25ef9a741a5edb73de655d21064ef212f`, version `1.0.0`. Worktree nguồn không có thay đổi khi bắt đầu đọc.

GitHub CLI của phiên viết tài liệu không truy xuất được metadata repo nguồn. Điều này không xác định repo đã bị xóa hay private; có thể do quyền truy cập. Phân tích dựa trên bản local, không khẳng định trạng thái upstream hiện tại.

## Nguồn cho từng kết luận

Các link dưới đây pin commit. Người không có quyền repo nguồn có thể nhận 404. Tài liệu giải thích độc lập vẫn đọc được trong repo này.

| Kết luận | Nguồn đã đọc |
| --- | --- |
| Plugin tên ytlailabs, liệt kê 17 agents | [plugin.json](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/.claude-plugin/plugin.json) |
| Bootstrap kiểm tra prerequisites và thử cài plugin | [bootstrap.sh](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/bootstrap.sh) |
| Profile, components, env và tích hợp CLAUDE.md | [setup-wizard.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/setup-wizard.js) |
| Symlink, copy, thay hooks, quyền env | [setup-helpers.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/lib/setup-helpers.js): `symlink`, `installHooks`, `copyEnvExample` |
| Sự kiện, matcher, async và profile của hook | [hooks.json](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/hooks/hooks.json) |
| Profile mặc định standard và disabled IDs | [hook-flags.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/lib/hook-flags.js) |
| Runtime gọi export run hoặc tiến trình con | [run-with-flags.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/hooks/run-with-flags.js) |
| Quality gate pass-through và các formatter | [quality-gate.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/hooks/quality-gate.js) |
| Plan chỉ dẫn gọi planner và chờ xác nhận | [plan.md](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/commands/plan.md) |
| Workload command chuyển sang skill | [command](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/commands/whatsonmyplate.md), [skill](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/skills/whatsonmyplate/SKILL.md) |
| Jira hướng dẫn thực hiện HTTP qua shell/Python | [jira/SKILL.md](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/skills/jira/SKILL.md) |
| TDD yêu cầu checkpoint và coverage | [tdd-workflow/SKILL.md](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/skills/tdd-workflow/SKILL.md) |
| Evaluator nhắc đánh giá, không tự tạo SKILL.md | [evaluate-session.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/hooks/evaluate-session.js) |
| Doctor kiểm tra thư mục/env và drift | [doctor.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/doctor.js), [doctor-checks.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/lib/doctor-checks.js) |
| Validator skill kiểm tra file tồn tại/không rỗng | [validate-skills.js](https://github.com/frogasia/claude-skills/blob/190776b25ef9a741a5edb73de655d21064ef212f/scripts/ci/validate-skills.js) |

## Những điểm cần đọc cẩn thận

| Quan sát trong snapshot | Ý nghĩa thực tế |
| --- | --- |
| README ghi 51 command, đếm được 53 file command cấp cao | Số liệu giới thiệu chưa đồng bộ; count không phải số command đã chạy thành công |
| Wizard báo hooks “merged”, helper thay toàn bộ `settings.hooks` | Có thể mất hook tùy chỉnh đang có trong trường đó |
| Helper symlink xóa mục đích trùng tên trước khi tạo link | Không nên chạy lại setup trên cấu hình quan trọng mà chưa kiểm tra đích |
| README nhắc `scripts/ytlailabs-skill`, file này không tồn tại | Không dùng nguyên lệnh CLI đó làm đường cài đã xác minh |
| Policy nhắc `manifests/install-modules.json`, snapshot không có file đó | Một số mô tả đường cài còn lệch nội dung được checkout |
| `--quick` vẫn gọi phần setup env có prompt | Không nên coi tùy chọn này chắc chắn phù hợp CI không tương tác |
| `classification` thiếu được helper coi là safe | Đây là giá trị mặc định của parser, không phải chứng nhận an toàn |
| Quality gate strict vẫn trả input gốc và được đăng ký async | Cần phân biệt log cảnh báo với chặn hành động |
| Evaluator chỉ đếm user message và log lời nhắc | Không có bằng chứng script đó tự sinh skill hay fine-tune model |
| Env path khác nhau giữa các hướng dẫn Jira | Có thể doctor thấy biến nhưng workflow cụ thể không nạp đúng file |

Đây là quan sát và rủi ro suy ra từ code đã đọc, không phải báo cáo rằng một sự cố vận hành đã xảy ra.

## Phần đã kiểm chứng và chưa kiểm chứng

Đã đọc các file trong bảng bằng chứng; đếm agents/commands/skills bằng filesystem; đối chiếu frontmatter, registration và nhánh return của runtime. Bản đồ skill là danh mục định hướng, không phải audit chi tiết toàn bộ 64 skill.

Chưa chạy bootstrap/wizard để cài bộ nguồn; chưa thử end-to-end trong Claude Code; chưa gọi Jira, Slack, Datadog hay hạ tầng; chưa đo hiệu quả, độ trễ hoặc coverage của plugin. Ví dụ trong bài thực hành dùng dữ liệu giả. Repo này không chứng nhận plugin sẵn sàng cho mọi môi trường production.

Không sửa hay publish repo nguồn. Nội dung public là phần giải thích đã viết lại, không chứa credentials, dữ liệu ticket, log vận hành, endpoint nội bộ hoặc bản sao source plugin.

## Tự đếm lại trên bản clone của bạn

Lệnh chỉ đọc sau cần chạy từ gốc repo nguồn:

```bash
node -e 'const fs=require("fs"); for (const d of ["agents","commands","skills"]) { const items=fs.readdirSync(d).filter(n => d==="skills" ? fs.existsSync(`${d}/${n}/SKILL.md`) : n.endsWith(".md")); console.log(d, items.length); }'
```

Kết quả quan sát ở snapshot: `agents 17`, `commands 53`, `skills 64`. Khi cập nhật commit, kiểm đếm lại và sửa tài liệu nếu cấu trúc thay đổi.
