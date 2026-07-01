# Prompt — Giai đoạn F (fan-out, đa prompt)

> Trình tự: **Plan 1 lần** (duyệt cách chia batch) → lặp "Prompt batch" → sau mỗi batch "Prompt dedup".

## Prompt 0 — Lập kế hoạch fan-out (Plan mode, 1 lần)
```
Đọc CLAUDE.md + PROJECT.md và guides/F-fanout/instructions.md. CHƯA sửa file.
Đọc components/CATALOG.md để biết component sẵn có và variant trước khi lập kế hoạch.
Đề xuất: cách chia ~3 batch cho các trang còn lại, trang nào cần làm riêng (phức tạp/multi-state),
mỗi trang dùng lại component nào (tham chiếu CATALOG.md), rủi ro xung đột file dùng chung. Chờ tôi duyệt rồi mới chạy batch.
```

## Prompt batch (dùng lại cho từng batch)
```
Đọc CLAUDE.md + PROJECT.md và guides/F-fanout/instructions.md, rồi thực thi.
Batch này gồm các trang: <slug1, slug2, …> (node URL trong PROJECT.md).
Với mỗi trang, spawn 1 subagent trong git worktree riêng. Mỗi agent làm đúng PAGE RECIPE:
- get_metadata + get_design_context + get_screenshot (desktop).
- Frame không auto-layout: tái dựng flex/grid từ ý đồ, KHÔNG toạ độ.
- Đọc components/CATALOG.md — dùng đúng tên component và variant trong đó, KHÔNG đẻ trùng.
- generateMetadata; mobile-first neo desktop.
- Self-verify bằng Playwright (chụp so Figma desktop), tự sửa trong ngưỡng <TOLERANCE_PX>px.
Trang multi-state (tab/step cùng URL): build 1 trang + cơ chế state, đọc tất cả frame state trong PROJECT.md.
Gộp worktree, cập nhật PROJECT.md (tick các trang), đề xuất commit.
```

## Prompt dedup (sau mỗi batch)
```
Quét các trang vừa làm: tìm component bị tạo trùng và JSX lặp đáng lẽ phải là component dùng chung.
Gộp về 1 component, export qua barrel index.ts, thay thế chỗ dùng. Verify build không lỗi.
(Tuỳ chọn: gợi ý tôi chạy /code-review.)
```
