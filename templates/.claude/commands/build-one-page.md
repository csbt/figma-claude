---
description: Build một trang từ Figma node theo Page Recipe trong CLAUDE.md
argument-hint: <page-slug> <figma-node-url>
allowed-tools: Read, Write, Edit, Bash, mcp__plugin_figma_figma__get_metadata, mcp__plugin_figma_figma__get_design_context, mcp__plugin_figma_figma__get_screenshot, mcp__plugin_figma_figma__download_assets
---

Bạn sẽ build MỘT trang duy nhất, theo đúng quy trình chuẩn của dự án.

- Page slug: `$1`
- Figma node URL: `$2` (nếu chỉ truyền slug, tra node URL trong bảng "Trang" của `PROJECT.md`)

## Bước 0 — Nạp ngữ cảnh (bắt buộc, không bỏ)
1. Đọc `CLAUDE.md` và `PROJECT.md`.
2. **Khám phá component sẵn có:** liệt kê `components/` và đọc barrel `components/**/index.ts`.
   Xem token trong `globals.css` (`@theme`, Tailwind v4).
Tuân thủ tuyệt đối **Hard Rules** và **Reflow Rules** trong `CLAUDE.md`.

## Bước 1 — Thực thi PAGE RECIPE
Làm tuần tự đúng mục "PAGE RECIPE" trong `CLAUDE.md` cho trang `$1`:
1. `get_metadata` → `get_screenshot` (desktop) → `get_design_context` cho node `$2`.
2. `layoutMode`: auto-layout → flex/gap; KHÔNG auto-layout → tái dựng từ screenshot,
   **không bê toạ độ, không position:absolute** (trừ overlay/trang trí thật sự).
3. **Lắp ráp bằng component SẴN CÓ.** Cần biến thể → thêm variant/prop, KHÔNG copy
   thành component mới. Chỉ tạo mới khi chưa tồn tại — và phải export qua `index.ts`
   + ghi vào `PROJECT.md`. Chỉ dùng token, không hardcode. Đơn giản hóa markup.
4. Áp Reflow Rules cho tablet/mobile.
5. `generateMetadata` (title, description, canonical, OG); semantic HTML; alt/aria.
6. Asset: icon → lucide-react; vector → SVG; raster → `download_assets` @2x + next/image.

## Bước 2 — Tự verify (không bỏ)
- Screenshot trang vừa build (desktop) và **so với screenshot Figma**.
- Liệt kê điểm lệch (spacing/màu/font/căn lề) và sửa tới khi khớp trong ngưỡng tolerance ở `CLAUDE.md`.
- Kiểm nhanh mobile/tablet theo Reflow Rules.

## Bước 3 — Chốt
- Chạy `pnpm build` (hoặc `pnpm lint`) để chắc không vỡ.
- Cập nhật `PROJECT.md`: đánh dấu `$1` = done, ghi component mới (nếu có).
- Tóm tắt đã làm gì + lệch còn lại (nếu có) → đề xuất commit `feat(page): $1`.

## Giới hạn
- KHÔNG đụng trang khác, không refactor ngoài phạm vi trang `$1`.
- KHÔNG cài thư viện ngoài whitelist trong `CLAUDE.md` — cần thì hỏi trước.
- Figma trả rỗng → kiểm tra Figma desktop đã mở & node URL đúng chưa, rồi báo lại.
