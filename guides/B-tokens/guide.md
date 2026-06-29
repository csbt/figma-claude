# Giai đoạn B — Design tokens · GUIDE (cho người)

> **Mục tiêu:** trích token (màu/typo/spacing/radius/shadow) từ Figma → định nghĩa trong
> `globals.css` `@theme` (Tailwind v4).
> **Output:** `globals.css @theme` có scale token nhất quán; áp được bằng class Tailwind.
> **Thời lượng:** ~45–60 phút · **Session:** 1 (+1 prompt sửa sau review).

---

## Điều kiện vào
- Giai đoạn A xong: project chạy, `@theme` rỗng đã có, Figma MCP OK.
- `PROJECT.md` đã có **Frame nguồn token** (2–4 frame phủ nhiều kiểu: home/nội dung/form).

## Việc của BẠN — đây là CỔNG REVIEW (đừng giao máy)
- Duyệt **bằng mắt** scale Claude đề xuất: màu có gom đúng không, thiếu/thừa token, font đúng chưa.
- Quyết các "giá trị lẻ cần hỏi" Claude liệt kê (gộp vào token nào / tách mới).
- Token sai ở đây = sai cả site → review kỹ.

## Việc giao CLAUDE
Đọc 2–4 frame nguồn token → trích giá trị thật → gom thành scale → ghi `@theme` → liệt kê giá trị lẻ vào `PROJECT.md`.

## Mô hình prompt (review-gated)
1. **Prompt 1** — trích + đề xuất scale + ghi `@theme` + liệt kê "cần quyết".
2. **Bạn review.**
3. **Prompt 2** (nếu cần) — sửa theo quyết định của bạn.

## Điểm REVIEW trước khi qua C
- [ ] `@theme` đủ nhóm: color, font, text size, spacing, radius, shadow, breakpoint.
- [ ] Không còn giá trị lẻ vô tổ chức; mục "Cần hỏi" trong `PROJECT.md` đã xử lý.
- [ ] Áp thử `bg-primary` / `text-h1` / `p-section` / `rounded-md` → đúng, dev không lỗi.
- [ ] (Nếu design có) dark mode/theme đã đưa vào dạng biến.

## Cạm bẫy (mục 10 playbook)
- **#4** hardcode thay vì token · **#18** bỏ qua dark mode · **#10** dùng font gần giống thay font thật.

## Commit gợi ý
```
feat(tokens): design tokens in globals.css @theme
```

→ **Tiếp theo:** `guides/C-inventory/`
