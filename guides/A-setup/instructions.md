# Chỉ thị — Giai đoạn A: Setup (cho Claude thực thi)

Mục tiêu: project chạy được + `CLAUDE.md` + `PROJECT.md` + công cụ + commit đầu tiên.
Tuân thủ luật cứng trong `CLAUDE.md`. Làm tuần tự, dừng hỏi ở chỗ ghi (HỎI).

## Bước 1 — Khởi tạo Next.js (kèm Tailwind v4)
- Chạy:
  ```
  pnpm dlx create-next-app@latest . --typescript --eslint --app --tailwind \
    --src-dir --import-alias "@/*" --use-pnpm
  ```
- create-next-app bản mới mặc định **Tailwind v4** (PostCSS plugin `@tailwindcss/postcss`,
  `globals.css` mở đầu bằng `@import "tailwindcss";`). XÁC NHẬN điều này; nếu lỡ ra v3 → nâng lên v4.
- `pnpm add lucide-react`

## Bước 2 — Pin version (chống "trôi" giữa các session)
- HỎI người version cụ thể, rồi: thêm `"packageManager": "pnpm@<x.y.z>"` vào `package.json`,
  tạo `.nvmrc` (Node LTS), ghim version `next` và `tailwindcss` ở mức cụ thể.
- Đảm bảo `pnpm-lock.yaml` sẽ được commit.

## Bước 3 — Token scaffold theo Tailwind v4
- Trong `src/app/globals.css`: giữ `@import "tailwindcss";` và thêm khối `@theme {}` **RỖNG**
  (token sẽ điền ở giai đoạn B). **KHÔNG** tạo `tailwind.config.ts` để chứa token — v4 dùng `@theme`.
  ```css
  @import "tailwindcss";

  @theme {
    /* Token điền ở giai đoạn B:
       --color-primary: …; --font-sans: …; --spacing-…: …; --radius-…: … */
  }
  ```

## Bước 4 — Cấu trúc thư mục + barrel
- Tạo `src/components/ui`, `src/components/layout`, `src/components/sections`,
  mỗi thư mục một `index.ts` **rỗng** (barrel — sẽ re-export component sau).
- Tạo `src/lib/`.

## Bước 5 — Hai file xương sống
- Copy template `CLAUDE.md` + `PROJECT.md` vào root.
- HỎI người các placeholder (`<PROJECT_NAME>`, `<DESIGN_WIDTH>`, `<TOLERANCE_PX>`,
  `<FIGMA_FILE_URL>`, version) rồi điền.
- Trong `CLAUDE.md`: sửa mọi mô tả token từ "tailwind.config.ts" → **"globals.css `@theme`"**
  (vì dùng Tailwind v4).

## Bước 6 — settings.json (`.claude/settings.json`)
- **Permissions allow tối thiểu:** `pnpm install/dev/build`, `pnpm dlx`, `git add/commit/status/diff`,
  các tool read-only của Figma MCP. KHÔNG allow lệnh xoá hoặc `git push` dạng mở.
- **Hook format sau mỗi sửa file:** ưu tiên dùng skill `update-config` để ghi đúng schema
  `PostToolUse` chạy `pnpm prettier --write` (+ `eslint --fix`). Nếu tự viết, kiểm tra lại
  schema hook theo bản Claude Code hiện tại.
- (Gợi ý cho người) sau vài session có thể chạy skill `fewer-permission-prompts` để bổ sung allowlist.

## Bước 7 — Công cụ MCP
- **Figma MCP:** gọi `get_metadata` trên 1 node URL trong `PROJECT.md` để xác nhận đọc được.
  Nếu rỗng → kiểm tra Figma desktop đã mở & node URL đúng.
- **Playwright MCP** (cho verify ảnh ở giai đoạn I): BÁO người chạy lệnh sau (cần quyền của họ):
  ```
  claude mcp add playwright -- npx @playwright/mcp@latest
  ```
  Không tự chạy nếu chưa được phép.

## Bước 8 — Verify & commit
- `pnpm dev` → xác nhận trang mặc định mở, không lỗi console nghiêm trọng. (Tuỳ chọn `pnpm build`.)
- `git init` (nếu chưa) → `git add -A` → đề xuất commit:
  `chore: scaffold Next.js + Tailwind v4 + project docs`.
- Cập nhật `PROJECT.md`: tick "A — Setup".

## Self-check trước khi báo xong
- [ ] `pnpm dev` chạy; `globals.css` có `@import "tailwindcss";` + `@theme {}` rỗng; KHÔNG có config token v3.
- [ ] `components/{ui,layout,sections}/index.ts` tồn tại.
- [ ] `CLAUDE.md` + `PROJECT.md` đã điền placeholder; mô tả token nói `@theme` (không phải tailwind.config.ts).
- [ ] Version pinned + `pnpm-lock.yaml` sẽ được commit.
- [ ] Figma MCP test OK; đã hướng dẫn người cài Playwright MCP.
- [ ] Commit xong; `PROJECT.md` tick A.

## KHÔNG làm trong giai đoạn này
- KHÔNG build trang hay section nào (việc của D/E/F).
- KHÔNG trích token (việc của B) — chỉ để `@theme` rỗng.
- KHÔNG thêm thư viện ngoài whitelist (Next, Tailwind, lucide-react) nếu chưa HỎI.
