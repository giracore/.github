## Why
Issue: #

## What changed

## Architecture affected

## User-visible behaviour

## Security / privacy impact

## Tests

## Risks

## Rollback

## Checklist
- [ ] Đã đọc hiến pháp + ADR liên quan; không phá điều nào (hoặc nêu rõ điều nào, vì sao, và link ADR mới)
- [ ] Đổi API contract? Có thì chạy contract-sentinel và báo mobile trước khi merge
- [ ] Có migration? Có thì chạy migration-safety
- [ ] Đụng AI / PII / auth / log? Có thì chạy pii-redaction-guard
- [ ] Không secret, không PII trong diff, log, test fixture
- [ ] Không trailer AI attribution trong commit
