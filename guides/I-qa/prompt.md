# Prompt — Giai đoạn I (vòng lặp)

## Prompt diff
```
Đọc CLAUDE.md + PROJECT.md và guides/I-qa/instructions.md, rồi thực thi.
Với từng trang: dùng Playwright MCP chụp site đang chạy ở viewport desktop (<DESIGN_WIDTH>px),
get_screenshot node Figma tương ứng, đặt cạnh và liệt kê điểm lệch (spacing/màu/font/căn lề/kích thước).
CHƯA sửa — xuất báo cáo diff theo trang để tôi xem trước.
```

## Prompt fix (lặp)
```
Sửa các điểm lệch sau cho khớp Figma desktop (giữ trong ngưỡng <TOLERANCE_PX>px, đừng sa đà pixel-perfect):
<dán danh sách / theo báo cáo trang …>.
Sau khi sửa, chụp lại bằng Playwright để xác nhận đã khớp. Cập nhật PROJECT.md khi trang đạt.
```
