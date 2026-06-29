# Chỉ thị — Giai đoạn G: SEO toàn cục (cho Claude thực thi)

Mục tiêu: SEO toàn site + audit. Tuân luật cứng CLAUDE.md.

## Bước 1 — sitemap & robots
- `app/sitemap.ts`: liệt kê mọi route thật (đọc cây `app/` + `PROJECT.md`). Dùng domain người cấp.
- `app/robots.ts`: allow hợp lý + trỏ tới sitemap.

## Bước 2 — JSON-LD (structured data)
- `lib/jsonld.ts`: helper tạo schema. Gắn:
  - `Organization` + `WebSite` (toàn site, trong layout).
  - `BreadcrumbList` cho trang con; schema phù hợp loại trang (Article/Product/FAQ… nếu có).
- Nhúng bằng `<script type="application/ld+json">` (server component).

## Bước 3 — Audit từng trang
- Mỗi trang: `generateMetadata` đủ title (template), description riêng, `alternates.canonical`, OG/Twitter.
- Đúng **1 `<h1>`**/trang, heading không nhảy bậc.
- Ảnh có `alt`; `<html lang>` đúng; internal link không gãy.
- Ghi báo cáo: trang nào thiếu gì; tự sửa chỗ có thể, liệt kê chỗ cần người cấp nội dung.

## Bước 4 — Verify
- `pnpm build`; chạy Lighthouse (hoặc nhờ người) mục SEO ≥ 95.
- Kiểm RSC render khi JS tắt (nội dung chính vẫn có trong HTML).

## Self-check
- [ ] sitemap/robots đúng route; JSON-LD validate được.
- [ ] Mỗi trang metadata + 1 h1 + heading bậc + alt + lang.
- [ ] Lighthouse SEO ≥ 95. `PROJECT.md` tick G.

## KHÔNG làm
- KHÔNG đổi layout/diện mạo trang (việc H/I).
- KHÔNG bịa nội dung brand/social — hỏi người nếu thiếu.
