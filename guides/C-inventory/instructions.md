# Chỉ thị — Giai đoạn C: Component inventory (cho Claude thực thi)

Mục tiêu: danh mục component chuẩn = `components/**/index.ts` (stub + barrel). Tuân luật cứng CLAUDE.md.

## Bước 1 — Chụp & quan sát
- `get_screenshot` từng trang theo node URL trong `PROJECT.md`.
- KHÔNG dùng tên layer Figma để định danh (có thể là `frame1`, trùng tên, hoặc loạn).

## Bước 2 — Cluster theo HÌNH DẠNG
- Gom các khối lặp lại theo cấu trúc/diện mạo, không theo tên.
- Phân biệt: **cùng một thứ khác diện mạo** → là VARIANT (prop); **khác hẳn cấu trúc** → component riêng.
- Phân loại:
  - `ui/` — primitive: Button, Card, Container, Section, Badge, Input…
  - `layout/` — Header, Footer, Nav, MobileNav…
  - `sections/` — khối trang lặp lại: Hero, FeatureGrid, CTASection, Testimonials…

## Bước 3 — Đặt tên chuẩn
- PascalCase, mô tả CHỨC NĂNG (`FeatureCard`, `PricingTile`), không theo Figma.
- Mỗi khái niệm đúng 1 tên. Ghi rõ biến thể dự kiến (vd `Button` có variant primary/secondary/ghost/outline).

## Bước 4 — Tạo stub + barrel
- Với mỗi component DÙNG LẠI, tạo file stub tối thiểu, ví dụ:
  ```tsx
  // components/ui/button.tsx
  export function Button(/* props sẽ định nghĩa ở D */) { return null }
  ```
- Export qua barrel:
  ```ts
  // components/ui/index.ts
  export { Button } from './button'
  export { Card } from './card'
  ```
- Component **chỉ xuất hiện ở 1 trang** (one-off) → KHÔNG stub ở đây, tạo lúc fan-out.

## Bước 5 — Liệt kê trùng/nhầm
- Ghi các trường hợp cần người quyết (gộp 2 tên về 1, tách 1 tên thành 2…) để review.

## Self-check trước khi báo xong
- [ ] Mỗi component lặp lại có đúng 1 tên chuẩn; phân loại đúng ui/layout/sections.
- [ ] Barrel `index.ts` export hết stub; biến thể ghi là prop/variant, không tạo component trùng.
- [ ] `pnpm dev`/build không lỗi import.

## KHÔNG làm
- KHÔNG implement chi tiết component (việc D) — chỉ stub.
- KHÔNG build trang.
- KHÔNG dựa vào tên Figma để gộp/định danh.
