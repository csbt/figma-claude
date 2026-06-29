# Prompt — Giai đoạn C (review-gated)

> 2 prompt: chạy Prompt 1, tự review (đổi tên/gộp/tách), rồi (tuỳ) chạy Prompt 2.

## Prompt 1 — Cluster & đề xuất danh mục
```
Đọc CLAUDE.md + PROJECT.md và guides/C-inventory/instructions.md, rồi thực thi.
Lấy node URL các trang trong PROJECT.md, get_screenshot từng trang.
Cluster các khối UI lặp lại theo HÌNH DẠNG (BỎ QUA tên layer Figma), đặt tên chuẩn của riêng ta,
phân loại ui/layout/sections, ghi rõ component nào nên là VARIANT của component khác.
Tạo stub tối thiểu cho các component DÙNG LẠI + export qua components/**/index.ts.
Liệt kê các trường hợp "trùng/nhầm cần xử lý" để tôi quyết. DỪNG để tôi review.
```

## Prompt 2 — Áp quyết định (chỉ dùng nếu cần)
```
Theo quyết định của tôi (đổi tên / gộp / tách như sau: …), cập nhật lại stub + barrel index.ts.
Đảm bảo mỗi khái niệm chỉ 1 component; biến thể là prop/variant. Verify import không lỗi.
```
