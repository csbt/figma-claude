# Giai đoạn H — Responsive pass · GUIDE (cho người)

> **Mục tiêu:** rà & hoàn thiện reflow ở mobile/tablet theo Reflow Rules — **không đụng desktop**.
> **Output:** mọi trang co giãn đúng ở các breakpoint mục tiêu.
> **Thời lượng:** ~1 giờ · **Session:** 1 (vòng lặp kiểm → sửa).

---

## Điều kiện vào
- F xong (các trang đã có, đã reflow sơ bộ trong recipe). H là lượt rà chuyên sâu.

## Việc của BẠN
- Quyết tranh chấp reflow nếu có (vd bảng phức tạp thành gì ở mobile) — ghi vào `PROJECT.md` mục scope.
- Nhớ: desktop là SPEC, không sửa desktop ở giai đoạn này.

## Việc giao CLAUDE
Dùng Playwright chụp từng trang ở mobile/tablet → soi lỗi reflow (overflow, vỡ grid, chữ tràn, touch target) → sửa.

## Mô hình prompt (vòng lặp)
1. **Prompt kiểm** — chụp các breakpoint, liệt kê lỗi reflow.
2. **Prompt sửa** — sửa theo danh sách, chụp lại xác nhận. Lặp tới sạch.

## Điểm REVIEW trước khi qua I
- [ ] Không scroll ngang ngoài ý muốn ở 360 / 768 / 1024 / 1440.
- [ ] Grid collapse, nav→hamburger, hero stack, bảng→card đúng Reflow Rules.
- [ ] Typography/spacing co giãn mượt (`clamp()`), không nhảy bậc thô.
- [ ] Touch target ≥ 44px; ảnh responsive (`sizes`).
- [ ] **Desktop không bị thay đổi** so với sau F.

## Cạm bẫy (mục 10 playbook)
- **#19** chỉ test nội dung mẫu (text dài thật làm vỡ) · sửa mobile làm lệch desktop.

## Commit gợi ý
```
fix(responsive): mobile/tablet reflow pass
```

→ **Tiếp theo:** `guides/I-qa/`
