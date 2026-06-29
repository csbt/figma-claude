# Chỉ thị — Giai đoạn E: Golden page (cho Claude thực thi)

Mục tiêu: 1 trang hoàn chỉnh làm khuôn + xác nhận PAGE RECIPE. Tuân luật cứng CLAUDE.md.

## Bước 1 — Theo PAGE RECIPE (trong CLAUDE.md), tóm tắt:
1. Khám phá: liệt kê `components/` + barrel; xem token `@theme`.
2. Lấy ngữ cảnh Figma trang golden: `get_metadata` → `get_screenshot` (desktop) → `get_design_context`.
3. Phân loại `layoutMode`: auto-layout → flex/gap; không auto-layout → tái dựng từ screenshot, KHÔNG toạ độ.
4. Lắp ráp bằng component sẵn có; cần khác → variant/prop; chỉ dùng token; markup gọn.
5. Reflow tablet/mobile theo Reflow Rules.
6. `generateMetadata` + semantic HTML + alt/aria.

## Bước 2 — Self-verify bằng Playwright MCP
- Chạy `pnpm dev`, dùng Playwright MCP mở route trang golden ở viewport desktop (`<DESIGN_WIDTH>`px), chụp ảnh.
- So với `get_screenshot` Figma cùng node. Liệt kê điểm lệch: spacing / màu / font / căn lề / kích thước.
- Tự sửa, chụp lại, lặp tới khi khớp trong ngưỡng `<TOLERANCE_PX>`px.
- Kiểm nhanh mobile/tablet.

## Bước 3 — Khoá khuôn
- Đối chiếu cách trang này được dựng với PAGE RECIPE trong `CLAUDE.md`; nếu có mẹo/bước nên bổ sung
  để 19 trang sau làm theo → cập nhật recipe.
- Cập nhật `PROJECT.md`: tick E + đổi trạng thái trang golden = Xong.

## Self-check trước khi báo xong
- [ ] Khớp desktop trong ngưỡng tolerance (có ảnh đối chiếu).
- [ ] Reflow đúng mobile/tablet.
- [ ] Tái dùng component, không đẻ trùng; chỉ token; markup gọn; a11y ổn.
- [ ] `generateMetadata` đầy đủ.
- [ ] PAGE RECIPE đã phản ánh đúng thực tế.

## KHÔNG làm
- KHÔNG build thêm trang khác (đó là F).
- KHÔNG đẻ component trùng — thêm variant nếu cần.
- KHÔNG bỏ qua self-verify bằng Playwright.
