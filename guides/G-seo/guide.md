# Giai đoạn G — SEO toàn cục · GUIDE (cho người)

> **Mục tiêu:** hoàn thiện SEO toàn site (sitemap, robots, JSON-LD) + audit metadata/heading/alt từng trang.
> **Output:** `app/sitemap.ts`, `app/robots.ts`, JSON-LD, báo cáo audit; Lighthouse SEO ≥ 95.
> **Thời lượng:** ~45–60 phút · **Session:** 1 (one-shot + sửa theo audit).

---

## Điều kiện vào
- Các trang đã có (F xong). Mỗi trang nên đã có `generateMetadata` từ recipe — G rà soát & bổ khuyết.

## Việc của BẠN
- Cung cấp thông tin SEO: tên brand, mô tả, domain, ảnh OG mặc định, social links (cho JSON-LD).
- Xem báo cáo audit, quyết các title/description còn thiếu.

## Việc giao CLAUDE
Tạo sitemap/robots, gắn JSON-LD phù hợp loại trang, audit toàn bộ metadata + heading + alt, sửa chỗ thiếu.

## Mô hình prompt
- 1 prompt chính (tạo + audit + sửa). Có thể +1 prompt sửa theo quyết định title/description của bạn.

## Điểm REVIEW trước khi qua H
- [ ] `sitemap.ts` + `robots.ts` đúng route thật.
- [ ] JSON-LD: Organization + WebSite (+ Breadcrumb/loại trang phù hợp), validate được.
- [ ] Mỗi trang: 1 `<h1>`, heading đúng bậc, title/description riêng, OG/canonical đủ.
- [ ] Ảnh có alt; lang attribute; internal link ổn.
- [ ] Lighthouse SEO ≥ 95; test với JS tắt vẫn render (RSC).

## Cạm bẫy (mục 10 playbook)
- **#14** làm SEO quá muộn (đỡ hơn nếu recipe đã gắn metadata) · thiếu `<h1>` hoặc nhiều `<h1>`.

## Commit gợi ý
```
feat(seo): sitemap, robots, JSON-LD, metadata audit
```

→ **Tiếp theo:** `guides/H-responsive/`
