# Chỉ thị — Giai đoạn J: Build & deploy (cho Claude thực thi)

Mục tiêu: build sạch + đạt ngưỡng + review + deploy. Tuân luật cứng CLAUDE.md.

## Bước 1 — Build
- `pnpm build`. Sửa mọi lỗi TypeScript, lỗi SSR, cảnh báo hydration.
- Kiểm các route động/`generateStaticParams` nếu có.

## Bước 2 — Lighthouse
- Chạy Lighthouse (hoặc hướng dẫn người chạy) cho vài trang đại diện: Performance, A11y, SEO, Best practices.
- Sửa các điểm dễ đạt: ảnh chưa tối ưu, thiếu `sizes`, contrast, thẻ thiếu. Ghi điểm vào báo cáo.

## Bước 3 — Review cuối
- Đề xuất người chạy `/code-review` (bug + dọn dẹp) và `/security-review` (rà bảo mật trước go-live).
- Xử lý các phát hiện hợp lý.

## Bước 4 — Deploy (chỉ khi người xác nhận)
- Deploy lên target người cấp (vd Vercel) với env vars + domain.
- Đây là hành động ra ngoài → KHÔNG tự thực hiện nếu chưa được phép.
- Sau deploy: kiểm URL + vài trang chính hoạt động.

## Bước 5 — Chốt
- Cập nhật `PROJECT.md`: tick J + ghi URL.
- Báo cáo theo Definition of Done (mục 11 playbook).

## Self-check
- [ ] `pnpm build` sạch, không lỗi hydration.
- [ ] Lighthouse đạt ngưỡng đã chốt.
- [ ] `/code-review` & `/security-review` đã chạy & xử lý.
- [ ] Deploy OK, URL kiểm tra được; `PROJECT.md` tick J + có URL.

## KHÔNG làm
- KHÔNG deploy khi build còn lỗi/cảnh báo hydration.
- KHÔNG tự deploy khi chưa được người xác nhận.
- KHÔNG commit secret/env vào repo.
