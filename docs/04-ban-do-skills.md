# Bản đồ 64 skills

Danh sách được kiểm đếm từ các thư mục có `skills/<name>/SKILL.md` ở commit nguồn đã ghi nhận. Phân nhóm dưới đây phục vụ học tập, không phải taxonomy chính thức. Mô tả nhóm giúp định hướng; khi dùng một skill, vẫn phải đọc file cụ thể để biết phạm vi, dependencies và thao tác.

| Nhóm | Tên skills trong snapshot |
| --- | --- |
| Tư duy và tổ chức agent (7) | `agent-harness-construction`, `agentic-engineering`, `ai-first-engineering`, `autonomous-loops`, `continuous-agent-loop`, `blueprint`, `project-guidelines-example` |
| Code và thiết kế ứng dụng (8) | `api-design`, `backend-patterns`, `frontend-patterns`, `coding-standards`, `python-patterns`, `django-patterns`, `pytorch-patterns`, `mcp-server-patterns` |
| Test và xác minh (9) | `tdd-workflow`, `python-testing`, `django-tdd`, `django-verification`, `verification-loop`, `e2e-testing`, `eval-harness`, `ai-regression-testing`, `plankton-code-quality` |
| Bảo mật và quản trị (5) | `security-review`, `security-scan`, `django-security`, `audit-export`, `audit-report` |
| Dữ liệu và hiệu năng (5) | `database-migrations`, `postgres-patterns`, `db-query`, `content-hash-cache-pattern`, `regex-vs-llm-structured-text` |
| Context, chi phí và học lại (8) | `context-budget`, `cost-aware-llm-pipeline`, `iterative-retrieval`, `search-first`, `strategic-compact`, `continuous-learning`, `continuous-learning-v2`, `skill-stocktake` |
| Deploy và vận hành (8) | `deployment-patterns`, `docker-patterns`, `eks-investigate`, `eks-redis-check`, `canary-watch`, `release-checklist`, `git-release`, `runbook` |
| Quan sát sự cố (4) | `datadog-alerts`, `datadog-logs`, `datadog-metrics`, `incident-report` |
| Công cụ làm việc (9) | `jira`, `jira-digest`, `whatsonmyplate`, `slack-read`, `slack-search`, `confluence`, `flagsmith`, `github-env-diff`, `github-vars` |
| API model (1) | `claude-api` |

Tổng: 64. Bảng này không khẳng định mọi skill đều có command cùng tên. Ví dụ `/ytlailabs:tdd` tương ứng workflow liên quan đến `tdd-workflow`, không phải một thư mục skill tên `tdd`.

## Đọc một skill theo sáu câu hỏi

1. **Khi nào chọn nó?** Đọc `name`, `description` và phần When to Use/Activate.
2. **Nó có thực hiện thao tác ngoài không?** Đọc classification, external access và các bước thực thi, không chỉ metadata.
3. **Nó cần gì?** Kiểm tra `required-env`, CLI, thư viện, file và quyền truy cập.
4. **Các bước thật là gì?** Theo từ command đến skill, script, tool và hệ thống đích.
5. **Đầu ra có kiểm chứng được không?** Tìm query, test target, path, exit code hoặc acceptance criteria.
6. **Có điều kiện đặc thù không?** Ví dụ filter project mặc định, tên custom field, package manager hoặc đường dẫn env.

## Classification trong wizard

Helper `listSkills()` tìm `classification` trong frontmatter; nếu thiếu thì mặc định `safe`. Wizard phân `external` vào nhóm ops, còn lại vào dev. Vì vậy `safe` trong màn hình setup có thể là giá trị suy ra khi thiếu metadata, không phải kết quả một cuộc audit.

`required-env` giúp setup/doctor tìm biến cần cấu hình. Nó không chứng minh API key hợp lệ, không thực thi phân quyền và không làm tool tự có mặt trong host.

## Chọn điểm bắt đầu

| Mục tiêu học | Bắt đầu đọc | Bài kiểm tra hiểu |
| --- | --- | --- |
| Hiểu workflow lập trình | `tdd-workflow`, `verification-loop` | Chỉ ra RED/GREEN và bằng chứng phải có |
| Hiểu skill gọi API | `whatsonmyplate`, `jira` | Vẽ đường từ command đến HTTP request |
| Hiểu tự động hóa | `hooks/hooks.json`, runtime scripts | Phân biệt cảnh báo, chặn và async |
| Hiểu quản lý context | `context-budget`, `strategic-compact` | Xác định thông tin nào cần giữ khi thu gọn |
| Hiểu học từ phiên trước | `continuous-learning` và evaluator | Chỉ ra script nào thực sự ghi file, script nào chỉ nhắc |

Không cần học hết 64 skill theo thứ tự. Học một workflow xuyên suốt thường giúp hiểu cơ chế nhanh hơn đọc từng tên riêng lẻ.
