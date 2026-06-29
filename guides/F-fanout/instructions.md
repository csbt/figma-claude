# Chỉ thị — Giai đoạn F: Fan-out (cho Claude thực thi)

Mục tiêu: dựng các trang còn lại song song theo PAGE RECIPE, tái dùng component. Tuân luật cứng CLAUDE.md.

## Orchestration mỗi batch
- Với mỗi trang trong batch → spawn 1 subagent, `isolation: worktree` (tránh xung đột file).
- Mỗi subagent thực thi PAGE RECIPE (trong CLAUDE.md) cho đúng trang của nó.
- Sau khi các agent xong → gộp worktree về nhánh làm việc.

## Mỗi subagent (1 trang) làm
1. Khám phá: liệt kê `components/` + barrel; xem token `@theme`.
2. `get_metadata` → `get_screenshot` (desktop) → `get_design_context` theo node URL trong `PROJECT.md`.
3. `layoutMode`: auto-layout → flex/gap; không auto-layout → tái dựng từ screenshot, **KHÔNG toạ độ / KHÔNG absolute** (trừ overlay).
4. Lắp ráp bằng component sẵn có; cần khác → variant/prop; **KHÔNG đẻ component trùng**. Chỉ token; markup gọn.
5. Reflow tablet/mobile; `generateMetadata`; semantic HTML + alt/aria.
6. Self-verify bằng Playwright MCP (chụp site so Figma desktop), tự sửa trong ngưỡng `<TOLERANCE_PX>`px.

## Trang nhiều trạng thái (tab/step)
- Cùng URL → đọc TẤT CẢ frame state của trang trong `PROJECT.md`; build **1 trang** + cơ chế tab/step,
  tái dùng component xuyên các state. Khác URL → đã là dòng riêng, làm như trang thường.

## Component dùng chung mới phát sinh
- Nếu cần một component dùng chung CHƯA có → KHÔNG để agent tự tạo lặng lẽ. Dừng batch, thêm ở trung tâm
  (`components/` + barrel) như giai đoạn D, rồi cho các agent dùng lại.

## Sau batch — dedup
- Quét component trùng + JSX lặp → gộp về 1, export qua barrel, thay thế chỗ dùng.
- `pnpm build` pass. Cập nhật `PROJECT.md` (tick trang). Đề xuất commit batch.

## Self-check mỗi batch
- [ ] Mọi trang trong batch build pass; reflow ổn.
- [ ] Không có component trùng (đã dedup); biến thể là prop.
- [ ] Trang multi-state đúng mô hình 1-trang-nhiều-state.
- [ ] `PROJECT.md` đã tick.

## KHÔNG làm
- KHÔNG sửa token hay đổi component dùng chung một cách tuỳ tiện trong worktree (gây xung đột).
- KHÔNG cài thư viện ngoài whitelist.
- KHÔNG nhồi quá nhiều trang vào 1 batch (≈5–7).
