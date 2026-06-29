# Giai đoạn A — Setup · GUIDE (cho người)

> **Mục tiêu:** dựng nền dự án + 2 file xương sống + công cụ, để các giai đoạn sau chạy trơn.
> **Output:** project chạy `pnpm dev`; có `CLAUDE.md` + `PROJECT.md`; có commit đầu tiên.
> **Thời lượng:** ~30–45 phút · **Số session:** 1.

---

## Điều kiện vào
- Đã xong Pre-flight (mục 1 playbook): khảo sát Figma, chốt scope sơ bộ với khách.
- Đã có **node URL các trang** (sẽ dán vào `PROJECT.md`).
- **Figma desktop đang mở** để test MCP.

## Việc của BẠN (không giao máy)
1. Chốt **version pin**: Node LTS, pnpm, Next, Tailwind v4.
2. Sau khi Claude copy template, **điền placeholder**: `<PROJECT_NAME>`, `<DESIGN_WIDTH>`,
   `<TOLERANCE_PX>`, `<FIGMA_FILE_URL>`.
3. **Cài Playwright MCP** khi Claude nhắc (lệnh cần quyền của bạn — xem `instructions.md` Bước 7).
4. **Review `settings.json` allowlist** — đừng allow quá rộng (vd cấm `rm -rf`, `git push` mở).

## Việc giao CLAUDE
Chạy `prompt.md` → Claude thực thi `instructions.md`: init Next.js + Tailwind v4, scaffold 2 file
xương sống, dựng cấu trúc `components/`, cấu hình settings + hook format, git init, verify `pnpm dev`.

## Điểm REVIEW trước khi qua giai đoạn B
- [ ] `pnpm dev` mở trang mặc định, không lỗi.
- [ ] `CLAUDE.md` + `PROJECT.md` tồn tại, placeholder đã điền.
- [ ] Tailwind v4 chạy: `globals.css` có `@import "tailwindcss";` + khối `@theme {}` (rỗng tạm).
      **Không** có `tailwind.config.ts` cho token.
- [ ] `components/{ui,layout,sections}/index.ts` đã tạo (barrel rỗng).
- [ ] Figma MCP đọc được 1 node thử; **Playwright MCP đã cài**.
- [ ] Version đã pin; `pnpm-lock.yaml` đã commit.
- [ ] Commit đầu tiên xong; `PROJECT.md` tick "A — Setup".

## Cạm bẫy của giai đoạn này (xem mục 10 playbook)
- **#15 Không pin version** → build trôi giữa các session. Pin ngay ở đây.
- **Allow permission quá rộng** → rủi ro xoá/đẩy nhầm. Chỉ allow lệnh thật sự cần.
- **Quên cài Playwright MCP** → giai đoạn I và self-verify sẽ kẹt. Cài ngay từ A.

## Commit gợi ý
```
chore: scaffold Next.js + Tailwind v4 + project docs
```

→ **Tiếp theo:** `guides/B-tokens/`
