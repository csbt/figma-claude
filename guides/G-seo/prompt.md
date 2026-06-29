# Prompt — Giai đoạn G

## Prompt chính
```
Đọc CLAUDE.md + PROJECT.md và guides/G-seo/instructions.md, rồi thực thi.
Thông tin SEO: brand=<…>, domain=<…>, mô tả=<…>, ảnh OG mặc định=<…>, social=<…>.
1. Tạo app/sitemap.ts + app/robots.ts theo route thật.
2. Gắn JSON-LD (Organization, WebSite, + Breadcrumb/loại trang phù hợp) qua lib/jsonld.
3. Audit từng trang: title/description/canonical/OG, 1 <h1> + heading đúng bậc, alt ảnh, lang.
Xuất báo cáo các thiếu sót và tự sửa được chỗ nào; liệt kê chỗ cần tôi cấp nội dung.
Cập nhật PROJECT.md (tick G) và đề xuất commit.
```

## Prompt sửa theo nội dung (nếu cần)
```
Title/description tôi cấp cho các trang còn thiếu: <…>. Cập nhật generateMetadata tương ứng.
```
