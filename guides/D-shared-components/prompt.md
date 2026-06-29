# Prompt — Giai đoạn D

> Mặc định 1 prompt. Nếu nhiều component → tách thành 2 (primitives, rồi layout).

## Prompt chính
```
Đọc CLAUDE.md + PROJECT.md và guides/D-shared-components/instructions.md, rồi thực thi.
Implement đầy đủ các stub trong components/ui + components/layout (đọc barrel index.ts để biết danh sách).
- Lấy diện mạo từ Figma: với mỗi component, mở frame TRANG nào có chứa nó (node URL các trang trong PROJECT.md).
- Biến thể = prop/variant; KHAI BÁO SẴN mọi variant thấy trên các trang để fan-out khỏi sửa lại.
- Chỉ dùng token trong globals.css @theme, không hardcode.
- Dựng app/layout.tsx: next/font, metadata gốc + OG default, bọc Header/Footer.
Render thử ở 1 route demo tạm để tôi xem, rồi xoá route tạm. Cập nhật PROJECT.md (tick D) và đề xuất commit.
```

## Prompt phụ (nếu tách) — layout
```
Tiếp tục giai đoạn D: implement components/layout (Header, Footer, Nav, MobileNav) + app/layout.tsx.
Header co về hamburger/drawer ở <lg theo Reflow Rules. Dùng token, export qua barrel.
```
