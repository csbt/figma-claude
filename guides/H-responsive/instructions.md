# Chỉ thị — Giai đoạn H: Responsive pass (cho Claude thực thi)

Mục tiêu: reflow mobile/tablet đúng Reflow Rules, **không đụng desktop**. Tuân luật cứng CLAUDE.md.

## Bước 1 — Kiểm bằng Playwright
- `pnpm dev`. Dùng Playwright MCP mở từng trang ở viewport: 360, 768, 1024, 1440.
- Chụp & soi: scroll ngang bất thường, vỡ grid, chữ/ảnh tràn (overflow), nav chưa hamburger ở `<lg`,
  touch target < 44px, spacing nhảy bậc thô.
- Liệt kê lỗi theo trang × breakpoint.

## Bước 2 — Sửa theo Reflow Rules
- Grid: desktop N cột → tablet 2 → mobile 1.
- Nav ngang → hamburger/drawer ở `<lg`.
- Hero 2 cột → stack dọc (text trước media).
- Typography/spacing: `clamp()` thay vì nhảy bậc.
- Bảng → card stack ở mobile.
- **KHÔNG thay đổi giao diện desktop** (chỉ thêm/chỉnh ở base + `md:` thấp hơn, không động `lg:` đang đúng).

## Bước 3 — Test nội dung thật
- Thử text/tên dài, nhiều item, ảnh thiếu → đảm bảo không vỡ.

## Bước 4 — Xác nhận
- Chụp lại các breakpoint sau sửa. So sánh desktop trước/sau để chắc không đổi. Cập nhật `PROJECT.md` (tick H).

## Self-check
- [ ] Không scroll ngang ở mọi breakpoint mục tiêu.
- [ ] Reflow đúng quy ước; touch target ≥ 44px; ảnh responsive.
- [ ] Desktop không bị thay đổi.

## KHÔNG làm
- KHÔNG sửa desktop (spec).
- KHÔNG đổi token/component dùng chung tuỳ tiện (ảnh hưởng nhiều trang) — nếu cần, báo người.
