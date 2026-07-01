# Component Catalog
_Sinh cuối giai đoạn D bởi Claude. Copy file này → `components/CATALOG.md`, điền variant/props thực tế._
_Cập nhật mỗi khi thêm component mới hoặc đổi variant._

---

## Primitives — `components/ui`

| Component | Import | Variants / Props | Dùng khi |
|---|---|---|---|
| Button | `import { Button } from '@/components/ui'` | variant: primary · secondary · ghost · outline · destructive · size: sm · md · lg | Mọi CTA, form submit, link styled as button |
| Card | `import { Card } from '@/components/ui'` | variant: default · hover · selected · padding: sm · md · lg | Product card, blog card, feature item, pricing tier |
| Badge | `import { Badge } from '@/components/ui'` | variant: default · success · warning · error · info | Label, tag, trạng thái |
| Input | `import { Input } from '@/components/ui'` | type: text · email · password · search · state: default · error · disabled | Form field |
| Container | `import { Container } from '@/components/ui'` | size: sm · md · lg · xl · full | Wrapper canh giữa nội dung theo breakpoint |
| Section | `import { Section } from '@/components/ui'` | padding: sm · md · lg | Wrapper section có vertical padding |
| _(thêm dòng cho mỗi primitive mới)_ | | | |

## Layout — `components/layout`

| Component | Import | Props key | Dùng khi |
|---|---|---|---|
| Header | `import { Header } from '@/components/layout'` | transparent?: boolean | **Đã mount trong `app/layout.tsx`** — KHÔNG import lại trong trang |
| Footer | `import { Footer } from '@/components/layout'` | — | **Đã mount trong `app/layout.tsx`** — KHÔNG import lại trong trang |
| MobileNav | `import { MobileNav } from '@/components/layout'` | — | Header tự dùng ở breakpoint `<lg` — không gọi trực tiếp |
| _(thêm dòng cho mỗi layout mới)_ | | | |

---

## Hướng dẫn điền (cho Claude — xoá section này sau khi điền xong)

- **Variants / Props**: liệt kê tên prop thực tế trong code, không viết tắt tự bịa.
- **Dùng khi**: 3–6 từ khoá ngắn đủ để Claude map từ "ý đồ Figma" → "component cần dùng".
- **Header/Footer**: luôn ghi rõ "Đã mount trong layout.tsx" để E/F không tạo trùng.
- **Sau khi điền**: xoá section "Hướng dẫn điền" này trước khi commit.
