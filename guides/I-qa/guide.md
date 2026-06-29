# Giai đoạn I — QA visual diff · GUIDE (cho người)

> **Mục tiêu:** đối chiếu pixel desktop (site thật vs Figma) từng trang & sửa cho khớp.
> **Output:** mọi trang khớp desktop trong ngưỡng tolerance; báo cáo diff.
> **Thời lượng:** ~1–1.5 giờ · **Session:** 1 (vòng lặp diff → fix).

---

## Điều kiện vào
- F (+ H) xong. Playwright MCP hoạt động.

## Việc của BẠN
- Quyết các lệch "ý đồ" (vd Figma vẽ thừa/thiếu so với nội dung thật) — không phải lệch nào cũng phải theo Figma.
- Chốt khi nào "đủ khớp" (tolerance), tránh sa đà pixel-perfect.

## Việc giao CLAUDE
Chụp từng trang ở desktop bằng Playwright → so với screenshot Figma → liệt kê lệch → sửa → chụp lại.

## Mô hình prompt (vòng lặp)
1. **Prompt diff** — chụp & liệt kê lệch toàn bộ trang (chưa sửa).
2. **Prompt fix** — sửa theo danh sách, chụp lại xác nhận. Lặp tới đạt tolerance.

## Điểm REVIEW (đây gần như là nghiệm thu desktop)
- [ ] Mỗi trang khớp Figma desktop trong ngưỡng `<TOLERANCE_PX>`px.
- [ ] Token khớp (màu/spacing/font không lệch ngầm).
- [ ] State theo scope (hover/focus/empty/error) đã làm.
- [ ] a11y: contrast, keyboard, focus ring, aria.
- [ ] Build SSR không lỗi hydration.

## Cạm bẫy (mục 10 playbook)
- **#8** không verify bằng ảnh ("trông giống") · **#16** pixel-perfect tuyệt đối · **#12** quên state.

## Commit gợi ý
```
fix(qa): visual diff fixes vs Figma desktop
```

→ **Tiếp theo:** `guides/J-deploy/`
