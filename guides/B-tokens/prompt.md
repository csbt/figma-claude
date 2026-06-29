# Prompt — Giai đoạn B (review-gated)

> Giai đoạn này có **2 prompt**: chạy Prompt 1, tự review, rồi mới (tuỳ) chạy Prompt 2.

## Prompt 1 — Trích & đề xuất token
```
Đọc CLAUDE.md + PROJECT.md và guides/B-tokens/instructions.md, rồi thực thi.
Frame nguồn token: lấy các node URL ở mục "Frame nguồn token" trong PROJECT.md (2–4 frame phủ nhiều kiểu: home/nội dung/form).
Mục tiêu: trích token từ Figma, gom thành scale nhất quán, ghi vào src/app/globals.css trong khối @theme
(Tailwind v4 — KHÔNG tạo tailwind.config.ts cho token).
Liệt kê mọi "giá trị lẻ cần quyết" vào mục "Cần hỏi" của PROJECT.md.
DỪNG để tôi review trước khi tinh chỉnh.
```

## Prompt 2 — Sửa sau review (chỉ dùng nếu cần)
```
Theo các quyết định tôi vừa ghi ở mục "Cần hỏi" trong PROJECT.md, cập nhật lại @theme cho khớp.
Không thêm token nào ngoài các quyết định đó. Verify lại dev không lỗi.
```
