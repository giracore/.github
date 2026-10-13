# giracore/.github

Mặc định cấp org cho mọi repo của GiraCore. GitHub tự áp dụng các file ở đây cho repo nào chưa có file cùng loại.

- `.github/ISSUE_TEMPLATE/`: ba form issue. Hợp đồng làm việc (feature), Lỗi, Đề xuất quyết định (ADR).
- `PULL_REQUEST_TEMPLATE.md`: PR phải nói Why / What / Architecture / Behaviour / Security-privacy / Tests / Risks / Rollback.
- `profile/README.md`: trang org public, chỉ thương hiệu.

Repo này public theo yêu cầu của GitHub. Không để bất kỳ thông tin nội bộ nào ở đây. Luật và quyết định nằm ở `giracore/governance` (private).

## Luồng một issue / Issue workflow

Mỗi issue đi qua các label `stage:*` theo thứ tự. Đổi label là cách chuyển bước.
Each issue moves through the `stage:*` labels in order. Changing the label is how it moves on.

| Label | Nghĩa | Meaning | Ai chuyển tiếp / Who moves it on |
|---|---|---|---|
| `stage:triage` | Mới tạo, chưa ai đọc | New, not read yet | Người duyệt, hoặc người mở issue khi cần gấp / The reviewer, or the author when it is urgent |
| `stage:council` | Hội đồng AI đang phân tích | The AI council is analysing it | Tự động: hội đồng đăng một comment rồi chuyển sang `stage:decision` / Automatic: the council posts one comment, then moves it to `stage:decision` |
| `stage:decision` | Chờ người duyệt quyết | Waiting for the reviewer's decision | Người duyệt gắn `decision:approved` hoặc `decision:rejected` / The reviewer adds `decision:approved` or `decision:rejected` |
| `stage:build` | Đã duyệt, đang làm | Approved, being built | Người làm, khi mở PR / The builder, when the PR opens |
| `stage:verify` | Có PR, đang review và test | PR open, under review and test | Đóng khi merge / Closed on merge |

**Quan trọng:** hội đồng **chỉ chạy khi issue được gắn `stage:council`**. Issue đứng ở `stage:triage` sẽ nằm yên, không ai phân tích. Tạo issue bằng form `[ADR]` thì đã có sẵn label này; tạo bằng form khác, bằng CLI hay bằng agent thì phải tự gắn.
**Important:** the council **only runs when the issue gets the `stage:council` label**. An issue left in `stage:triage` just sits there. The `[ADR]` form adds the label for you; with the other forms, the CLI or an agent, add it yourself.

```sh
gh issue edit <số / number> -R giracore/<repo> --remove-label stage:triage --add-label stage:council
```

- Hội đồng hiện chạy ở những repo đã có `.github/workflows/council.yml`. Repo chưa có thì gắn label cũng không có gì xảy ra.
  The council runs only in repos that have `.github/workflows/council.yml`. Elsewhere the label does nothing.
- Không gắn `escalate` khi mở issue; hội đồng tự gắn khi có câu hỏi cần người quyết.
  Do not add `escalate` when opening an issue; the council adds it when a question needs a human.
- Hội đồng chạy lỗi sẽ comment kèm link log và gắn `escalate`. Gắn lại `stage:council` để chạy lại.
  If a council run fails it comments with the log link and adds `escalate`. Re-add `stage:council` to retry.
