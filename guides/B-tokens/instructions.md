# Chỉ thị — Giai đoạn B: Design tokens (Tailwind v4)

Mục tiêu: token sống trong `src/app/globals.css` khối `@theme`. Tuân luật cứng CLAUDE.md.

## Bước 1 — Thu thập nguyên liệu
- Nếu Figma có Variables/Styles: gọi `get_variable_defs` trước (nhanh & chính xác nhất).
- `get_design_context` + `get_screenshot` trên các frame đại diện trong `PROJECT.md` để lấy
  màu/typography/spacing thực tế.

## Bước 2 — Gom thành scale (KHÔNG bê nguyên giá trị lẻ)
- **Màu:** gom sắc gần nhau thành token có nghĩa (primary, secondary, accent, background,
  foreground, muted, border, destructive, success…), kèm `-foreground` khi cần.
- **Typography:** family (heading/body) + scale (display/h1…h3/body/small) gồm size + line-height + weight.
- **Spacing:** scale bậc (xs…2xl) + section padding + container max-width.
- **Radius, shadow:** theo dùng thực tế.
- **Breakpoint:** mobile `<768`, tablet `768–1023`, desktop `≥1024`.

## Bước 3 — Ghi vào `globals.css` `@theme` (cú pháp v4)
```css
@import "tailwindcss";

@theme {
  --color-primary: #...;
  --color-primary-foreground: #...;
  --color-muted: #...;
  --font-sans: "...", ui-sans-serif, system-ui;
  --text-h1: 2.5rem;
  --spacing-section: 5rem;
  --radius-md: 0.5rem;
  --shadow-card: 0 1px 3px rgb(0 0 0 / .1);
}
```
- Token map thẳng sang utility v4: `--color-*` → `bg-/text-/border-*`; `--spacing-*` → `p-/m-/gap-*`;
  `--radius-*` → `rounded-*`; `--text-*` → `text-*`; `--font-*` → `font-*`.
- (Dark mode nếu có) định nghĩa biến override trong `:root` / `.dark` phù hợp.

## Bước 4 — Liệt kê "giá trị lẻ cần quyết"
- Giá trị không gom được vào scale → ghi mục "Cần hỏi" trong `PROJECT.md`. **KHÔNG tự thêm token bừa.**

## Bước 5 — Verify nhanh
- Áp thử vài class (`bg-primary`, `text-h1`, `p-section`, `rounded-md`) ở một đoạn tạm →
  `pnpm dev` không lỗi, hiển thị đúng. Xoá đoạn tạm sau khi xem.

## Self-check trước khi báo xong
- [ ] `@theme` đủ nhóm token; tên nhất quán; map ra utility được.
- [ ] Giá trị lẻ đã liệt kê vào `PROJECT.md`, không tự thêm bừa.
- [ ] KHÔNG tạo `tailwind.config.ts` cho token.
- [ ] dev/build không lỗi.

## KHÔNG làm
- KHÔNG build component/trang (việc C/D/E/F).
- KHÔNG hardcode màu/spacing ở bất kỳ đâu ngoài `@theme`.
- KHÔNG tự quyết "giá trị lẻ" — để người duyệt.
