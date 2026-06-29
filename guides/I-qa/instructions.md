# Chỉ thị — Giai đoạn I: QA visual diff (cho Claude thực thi)

Mục tiêu: khớp desktop với Figma trong ngưỡng tolerance. Dùng Playwright MCP. Tuân luật cứng CLAUDE.md.

## Bước 1 — Chụp & đối chiếu (mỗi trang)
- `pnpm dev`. Playwright MCP mở trang ở viewport desktop `<DESIGN_WIDTH>`px → chụp.
- `get_screenshot` node Figma tương ứng (trong `PROJECT.md`).
- Đặt cạnh, liệt kê lệch: spacing, màu, font/leading, căn lề, kích thước, thiếu/thừa phần tử.
- Phân loại lệch: (a) lỗi cần sửa; (b) "khác do nội dung thật" → báo người quyết, đừng tự bẻ theo Figma.

## Bước 2 — Sửa (vòng lặp)
- Sửa nhóm (a), ưu tiên đúng **token/scale** hơn là khớp từng pixel.
- Chụp lại, so tiếp, lặp tới khi trong ngưỡng `<TOLERANCE_PX>`px.

## Bước 3 — Kiểm bổ sung
- State theo scope: hover/focus/empty/error/loading (xem `PROJECT.md` mục scope).
- a11y: contrast AA, focus ring, điều hướng bàn phím, aria.
- `pnpm build` → không lỗi hydration.

## Self-check
- [ ] Mỗi trang khớp Figma desktop trong ngưỡng tolerance (có ảnh đối chiếu).
- [ ] Token không lệch ngầm; state & a11y đạt.
- [ ] Build sạch. `PROJECT.md` đã cập nhật trạng thái diff OK.

## KHÔNG làm
- KHÔNG đập đi xây lại trang — chỉ sửa lệch.
- KHÔNG chạy theo pixel-perfect tuyệt đối (tốn vô ích).
- KHÔNG tự quyết lệch "ý đồ" — hỏi người.
