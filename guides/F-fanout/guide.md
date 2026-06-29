# Giai đoạn F — Fan-out các trang · GUIDE (cho người)

> **Mục tiêu:** dựng các trang còn lại song song theo "khuôn" golden, tái dùng component.
> **Output:** tất cả trang trong `PROJECT.md` = Xong.
> **Thời lượng:** phần dài nhất · **Session:** chia ~3 batch, **mỗi batch 1 prompt** (+1 prompt dọn dedup).

---

## Điều kiện vào
- A→E xong, **golden page đã khoá khuôn**. Token + component dùng chung ổn định.

## Việc của BẠN
- Chia trang thành batch (~5–7 trang/batch). Sau mỗi batch: chạy prompt dọn dedup, review nhanh, commit.
- Nếu phát sinh nhu cầu **component dùng chung mới** → dừng, thêm ở trung tâm (như D), rồi tiếp — đừng để
  mỗi agent tự đẻ trong worktree.
- Trang nào trả về bị lệch → **vá theo section trong session chính**, KHÔNG re-spawn subagent (subagent đã đóng
  context). Cách vá: xem *"Sửa lỗi theo section"* ở `guides/E-golden-page/guide.md`. Lệch nặng/sai cấu trúc → kéo
  riêng trang đó ra làm lại bằng `/build-one-page`.

## Việc giao CLAUDE
Mỗi batch: spawn subagent trong git worktree riêng, mỗi agent 1 trang theo PAGE RECIPE; gộp worktree;
cập nhật `PROJECT.md`.

## Mô hình prompt (fan-out, dùng lại mỗi batch)
0. **Plan mode** (1 lần, trước batch đầu) — Claude đề xuất cách chia batch + map component tái dùng + cách xử lý trang đặc biệt, CHỜ bạn duyệt.
1. **Prompt batch** (lặp cho từng batch) — liệt kê trang của batch.
2. **Prompt dedup** (sau mỗi batch) — quét component trùng/JSX lặp → gộp.

## Điểm REVIEW sau mỗi batch
- [ ] Mỗi trang `pnpm build` pass.
- [ ] Không phát sinh component trùng (chạy prompt dedup + liếc barrel).
- [ ] Trang multi-state (tab/step) build thành 1 trang + cơ chế state, không tách nhầm.
- [ ] `PROJECT.md` tick các trang đã xong.

## Cạm bẫy (mục 10 playbook)
- **#6** nhồi quá nhiều vào 1 session → chia batch · component trùng khi song song → prompt dedup ·
  **#2** dịch absolute thô.

## Commit gợi ý
```
feat(pages): batch <n> — <danh sách slug>
```

→ **Tiếp theo:** `guides/G-seo/`
