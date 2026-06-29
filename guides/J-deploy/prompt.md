# Prompt — Giai đoạn J

## Prompt chính
```
Đọc CLAUDE.md + PROJECT.md và guides/J-deploy/instructions.md, rồi thực thi.
1. pnpm build — sửa mọi lỗi TS/SSR/hydration.
2. Chạy Lighthouse (hoặc hướng dẫn tôi) cho vài trang đại diện: perf, a11y, SEO. Sửa các điểm dễ đạt.
3. Đề xuất tôi chạy /code-review và /security-review; xử lý các phát hiện.
Báo cáo trạng thái + checklist Definition of Done. CHƯA deploy cho tới khi tôi xác nhận.
```

## Prompt deploy (sau khi tôi xác nhận)
```
Deploy lên <target> với env: <…>, domain: <…>.
Sau deploy, kiểm URL hoạt động + vài trang chính. Cập nhật PROJECT.md (tick J) và ghi URL.
```
