# Prompt — Giai đoạn E (vòng lặp)

> Trình tự: **Plan** (duyệt cách làm) → Prompt dựng → lặp Prompt sửa cho tới khi khớp tolerance.
> Bật Plan mode (Shift+Tab) trước khi dán Prompt 0, hoặc cứ dán — prompt đã yêu cầu chờ bạn duyệt.

## Prompt 0 — Lập kế hoạch (Plan mode)
```
Đọc CLAUDE.md + PROJECT.md và guides/E-golden-page/instructions.md. CHƯA sửa file.
Đọc components/CATALOG.md để biết component sẵn có, variant, và cái nào đã mount trong layout.tsx.
Trang golden: <slug> (node URL trong PROJECT.md).
Đề xuất kế hoạch: cấu trúc trang theo section, component nào tái dùng / cần tạo mới (variant?),
cách dựng layout (chỗ nào auto-layout/không auto-layout), điểm rủi ro. Chờ tôi duyệt rồi mới build.
```

## Prompt dựng
```
Đọc CLAUDE.md + PROJECT.md và guides/E-golden-page/instructions.md, rồi thực thi.
Đọc components/CATALOG.md — dùng đúng tên component và variant trong đó, không tự bịa component mới.
Build trang GOLDEN: <slug> (node URL trong PROJECT.md) theo đúng PAGE RECIPE trong CLAUDE.md.
Chỉ dùng token @theme.
Sau khi dựng, dùng Playwright MCP chụp trang đang chạy ở desktop và so với screenshot Figma;
liệt kê điểm lệch và tự sửa. Báo cáo các điểm còn lệch để tôi xem.
```

## Prompt sửa theo diff (lặp)
```
Các điểm cần sửa cho khớp Figma: <dán danh sách / mô tả>.
Sửa, rồi chụp lại bằng Playwright so với Figma desktop để xác nhận đã khớp trong ngưỡng <TOLERANCE_PX>px.
```

## Prompt khoá khuôn (sau khi ưng)
```
Trang golden đã đạt. Rà lại PAGE RECIPE trong CLAUDE.md cho khớp đúng cách trang này được dựng
(thứ tự bước, mẹo xử lý). Cập nhật nếu cần. Cập nhật PROJECT.md (tick E) và đề xuất commit.
```
