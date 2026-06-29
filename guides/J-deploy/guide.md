# Giai đoạn J — Build & deploy · GUIDE (cho người)

> **Mục tiêu:** build sạch, đạt ngưỡng Lighthouse, chạy review cuối, deploy.
> **Output:** bản build production + URL; `/code-review` & `/security-review` đã xử lý.
> **Thời lượng:** ~45–60 phút · **Session:** 1.

---

## Điều kiện vào
- A→I xong. Tất cả trang trong `PROJECT.md` = Xong; scope đã chốt.

## Việc của BẠN
- Cấp thông tin deploy (target, env vars, domain).
- Duyệt kết quả `/code-review` & `/security-review` trước khi go-live.
- Bấm nút deploy (hoặc cấp quyền) — hành động ra ngoài cần bạn xác nhận.

## Việc giao CLAUDE
`pnpm build` → fix lỗi SSR/TS/hydration → Lighthouse perf/a11y → chạy review → chuẩn bị deploy.

## Mô hình prompt
- 1 prompt chính. Review/deploy có thể tách prompt riêng tuỳ kết quả.

## Điểm REVIEW (Definition of Done — mục 11 playbook)
- [ ] `pnpm build` sạch, không lỗi hydration.
- [ ] Lighthouse: SEO ≥ 95, Performance & A11y đạt ngưỡng đã chốt.
- [ ] `PROJECT.md`: tất cả trang = Xong; scope đã chốt.
- [ ] `/code-review` & `/security-review` đã chạy và xử lý.
- [ ] Deploy thành công, URL kiểm tra được.

## Cạm bẫy (mục 10 playbook)
- Deploy khi build có warning hydration · quên env vars · bỏ qua review bảo mật.

## Commit gợi ý
```
chore(release): production build + deploy
```

→ **Hoàn tất.** Đối chiếu Definition of Done (mục 11 playbook).
