# Giai đoạn E — Golden page · GUIDE (cho người)

> **Mục tiêu:** build 1 trang HOÀN CHỈNH (thường là Home) end-to-end làm "khuôn" cho 19 trang sau.
> **Output:** 1 trang khớp desktop + responsive + SEO; "page recipe" trong `CLAUDE.md` được xác nhận/tinh chỉnh.
> **Thời lượng:** ~1.5–2 giờ · **Session:** 1 (vòng lặp build → fix diff).

---

## Điều kiện vào
- A→D xong: token + component dùng chung + layout đã sẵn.
- Playwright MCP đã cài (để self-verify).

## Việc của BẠN — CỔNG CHẤT LƯỢNG QUAN TRỌNG NHẤT
- **Review cực kỹ** trang này: spacing/màu/font/responsive/markup. Đây là khuôn — đẹp ở đây thì 19 trang sau mới đều.
- Xác nhận "page recipe" trong `CLAUDE.md` phản ánh đúng cách trang này được dựng (nếu cần, tinh chỉnh recipe).
- **Đừng vội** sang F khi chưa thật ưng E.

## Việc giao CLAUDE
Dựng trang theo PAGE RECIPE → self-verify bằng Playwright (chụp site so Figma) → tự sửa cho khớp.

## Mô hình prompt (vòng lặp, có cổng Plan mode)
0. **Plan mode** — Claude đề xuất cấu trúc trang + component tái dùng + cách layout, CHỜ bạn duyệt (chưa sửa file).
1. **Prompt dựng** — build trang theo recipe + tự verify.
2. **Prompt sửa theo diff** — lặp lại đến khi khớp tolerance ("sửa các điểm lệch sau: …").

## Điểm REVIEW trước khi qua F
- [ ] Khớp desktop trong ngưỡng `<TOLERANCE_PX>`px.
- [ ] Reflow đúng ở tablet/mobile.
- [ ] Tái dùng component dùng chung, không đẻ trùng; chỉ dùng token.
- [ ] `generateMetadata` đầy đủ; heading/alt/aria ổn.
- [ ] PAGE RECIPE trong `CLAUDE.md` đã khớp thực tế → KHOÁ KHUÔN.

## Cạm bẫy (mục 10 playbook)
- **#1** fan-out trước khi khoá khuôn · **#16** đòi pixel-perfect tuyệt đối · **#17** markup div-lồng-div.

## Commit gợi ý
```
feat(page): home (golden template)
```

→ **Tiếp theo:** `guides/F-fanout/`
