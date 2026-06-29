# Giai đoạn C — Component inventory · GUIDE (cho người)

> **Mục tiêu:** lập danh mục component CHUẨN (đặt tên bằng mắt, **bỏ qua tên Figma**) →
> tạo stub + barrel `index.ts`.
> **Output:** `components/{ui,layout,sections}/index.ts` export stub các component dùng lại; tên do người chốt.
> **Thời lượng:** ~45–60 phút · **Session:** 1 (+1 prompt sửa sau review).

---

## Điều kiện vào
- A, B xong. `PROJECT.md` có node URL các trang.

## Việc của BẠN — CỔNG REVIEW
- Duyệt danh sách Claude đề xuất: tên hợp lý chưa, **gộp/tách đúng chưa**
  (`frame1` ≠ thông tin; trùng tên ≠ trùng component; khác tên có thể là cùng component).
- Quyết các trường hợp "trùng/nhầm cần xử lý".

## Việc giao CLAUDE
Screenshot các trang → cluster theo **HÌNH DẠNG** → đề xuất tên chuẩn + map trang dùng → tạo stub + barrel.

## Mô hình prompt (review-gated)
1. **Prompt 1** — cluster + đề xuất danh mục + tạo stub + barrel.
2. **Bạn review** (đổi tên/gộp/tách).
3. **Prompt 2** (nếu cần) — áp quyết định của bạn.

## Điểm REVIEW trước khi qua D
- [ ] Mỗi component lặp lại chỉ có **1 tên chuẩn**; không trùng nghĩa.
- [ ] Phân loại đúng `ui` / `layout` / `sections`.
- [ ] Barrel `index.ts` export stub; **biến thể dự kiến ghi là prop/variant**, không phải component riêng.
- [ ] Stub build/dev không lỗi (import được).

## Cạm bẫy (mục 10 playbook)
- **#3** tin tên Figma để định danh · **#1** fan-out khi chưa có inventory.

## Commit gợi ý
```
chore(components): inventory stubs + barrels
```

→ **Tiếp theo:** `guides/D-shared-components/`
