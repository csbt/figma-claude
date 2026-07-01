# Giai đoạn D — Shared layout + primitives · GUIDE (cho người)

> **Mục tiêu:** implement đầy đủ các component DÙNG CHUNG (từ stub ở C) + layout gốc, để fan-out
> chỉ việc import.
> **Output:** `components/ui` + `components/layout` hoàn chỉnh (+ barrel), `app/layout.tsx` (font, metadata gốc, Header/Footer),
> **`components/CATALOG.md`** (bảng tra cứu component cho E/F — bắt buộc).
> **Thời lượng:** ~1.5–2 giờ · **Session:** 1 (có thể tách primitives / layout thành 2 prompt nếu nhiều).

---

## Điều kiện vào
- A, B, C xong. `@theme` có token; barrel có stub; tên đã chốt.

## Việc của BẠN
- Duyệt nhanh diện mạo + **bộ variant** mỗi primitive (Button có đủ primary/secondary/ghost/outline chưa…).
- Đây là tiền đề để fan-out KHÔNG phải sửa file dùng chung → giảm xung đột.

## Việc giao CLAUDE
Implement primitives (variant = prop), layout (Header/Footer/Nav/MobileNav), `layout.tsx` (next/font + metadata gốc),
tất cả dùng token từ `@theme`, export qua barrel.

## Mô hình prompt
- 1 prompt chính. Nếu khối lượng lớn → tách: **Prompt 1** primitives `ui/`, **Prompt 2** `layout/` + `layout.tsx`.

## Điểm REVIEW trước khi qua E
- [ ] Mỗi primitive **khai báo sẵn mọi variant** thấy trên 20 trang (để fan-out chỉ chọn, không sửa).
- [ ] Header → mobile thành hamburger/drawer (theo Reflow Rules).
- [ ] `layout.tsx` nạp font bằng `next/font`, có metadata gốc + OG default.
- [ ] Tất cả dùng token, không hardcode; render thử ổn ở 1 trang demo tạm.
- [ ] **`components/CATALOG.md` tồn tại**, variant/props đúng với code thực tế, Header/Footer ghi rõ "đã mount trong layout.tsx".

## Cạm bẫy (mục 10 playbook)
- **#9** tự cài UI kit ngoài whitelist · **#4** hardcode · biến thể đẻ thành component mới thay vì prop.

## Commit gợi ý
```
feat(components): shared primitives + layout
```

→ **Tiếp theo:** `guides/E-golden-page/`
