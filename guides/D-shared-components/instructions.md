# Chỉ thị — Giai đoạn D: Shared layout + primitives (cho Claude thực thi)

Mục tiêu: implement đầy đủ component dùng chung + layout gốc, dùng token `@theme`. Tuân luật cứng CLAUDE.md.

## Bước 1 — Đọc danh mục
- Liệt kê `components/` + đọc barrel `index.ts` để biết các stub cần implement.
- Xem token trong `globals.css @theme` để dùng đúng.

## Bước 2 — Primitives (`components/ui`)
- Implement từng stub: Button, Card, Container, Section, Badge, Input… theo Figma — mở frame TRANG có chứa component đó (node URL trang trong PROJECT.md), không bịa diện mạo.
- **Variant = prop**, không tạo component mới. Ví dụ Button:
  ```tsx
  const styles = {
    variant: { primary: 'bg-primary text-primary-foreground',
               secondary: 'bg-secondary text-foreground',
               ghost: 'bg-transparent hover:bg-muted',
               outline: 'border border-border' },
    size: { sm: 'h-9 px-3', md: 'h-11 px-5', lg: 'h-13 px-7' },
  }
  export function Button({ variant='primary', size='md', className, ...props }) { /* gộp class */ }
  ```
- **Khai báo sẵn MỌI variant** thấy trên các trang (tham chiếu inventory C) → fan-out chỉ chọn, không sửa file này.

## Bước 3 — Layout (`components/layout`)
- Header/Footer/Nav theo Figma. Header → mobile thành hamburger/drawer (MobileNav) ở `<lg` theo Reflow Rules.
- Export tất cả qua barrel.

## Bước 4 — `app/layout.tsx`
- Nạp font bằng `next/font` (tự host) → gán vào `<html>`/`<body>`.
- `metadata` gốc (title template, description, OG default).
- Bọc `Header` + `Footer` quanh `children`.

## Bước 5 — Render thử
- Tạo route demo tạm (vd `app/_demo/page.tsx`) render các primitive + layout để mắt thường kiểm.
- `pnpm dev` không lỗi → xoá route tạm.

## Bước 6 — Sinh Component Catalog
- Copy template `guides/D-shared-components/CATALOG.md` → tạo `components/CATALOG.md`.
- Điền **tên component, import path, variant/props thực tế** vừa implement — không bịa, không bỏ sót variant.
- Cột "Dùng khi": 3–6 từ khoá đủ để map từ ý đồ Figma → component đúng.
- Header/Footer: ghi rõ "Đã mount trong `app/layout.tsx`" để E/F không import lại.
- Xoá section "Hướng dẫn điền" trong template trước khi commit.
- **Đây là nguồn sự thật cho E/F** — sai ở đây dẫn tới dùng sai component hoặc tạo trùng.

## Self-check trước khi báo xong
- [ ] Mọi stub đã implement; barrel export đúng.
- [ ] Primitive khai báo đủ variant (prop), không có component trùng nghĩa.
- [ ] Header responsive (hamburger ở mobile); `layout.tsx` có font + metadata gốc.
- [ ] Chỉ dùng token `@theme`, không hardcode; dev không lỗi.
- [ ] `PROJECT.md` tick D.

## KHÔNG làm
- KHÔNG build trang nội dung (việc E/F).
- KHÔNG cài thư viện ngoài whitelist nếu chưa HỎI.
- KHÔNG để biến thể đẻ thành component mới — dùng prop/variant.
