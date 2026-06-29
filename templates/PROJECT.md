<!--
  TEMPLATE — file trạng thái DUY NHẤT của dự án. Để trống lúc đầu, lớn dần khi làm.
  Gộp: danh sách trang + tiến độ + quyết định scope + điểm cần hỏi.
  Tokens KHÔNG ghi ở đây (đã nằm trong globals.css @theme — Tailwind v4). Component KHÔNG liệt kê
  ở đây (xem thư mục components/ + barrel index.ts).
-->

# <PROJECT_NAME>

- Figma: <FIGMA_FILE_URL> — **bản gốc chỉ có desktop → desktop là SPEC**.
- Cập nhật lần cuối: ____

> Quy ước trạng thái cột "Trạng thái": `Chưa làm` · `Đang làm` · `Xong`

## Trang (bạn tự dán link từ Figma)

| Slug | Route | Figma node URL | Trạng thái |
|---|---|---|---|
| home | `/` | <node-url> | Chưa làm |
| about | `/about` | <node-url> | Chưa làm |
| ... | | | Chưa làm |

> Frame đại diện để trích token & component: <node-url> (header/footer/card/hero…)

### Trang có nhiều trạng thái / tab / step

> Một ROUTE có thể ứng với NHIỀU frame Figma (mỗi frame = 1 state).
> - Các state CÙNG một URL (tab/step đổi nội dung tại chỗ) → vẫn là **1 dòng** ở bảng
>   trên, liệt kê các frame ở đây. Claude build **1 trang** với cơ chế tab/step,
>   KHÔNG tách thành nhiều trang.
> - Mỗi state là URL KHÁC nhau (vd `/checkout/cart`, `/checkout/payment`) → tách
>   thành **nhiều dòng riêng** ở bảng trên, không dùng mục này.

**`/pricing`** — tab, cùng URL (state ở client):
- Tab "Monthly": <node-url>
- Tab "Yearly": <node-url>

**`/checkout`** — step, cùng URL (state nội bộ):
- Step 1 — Giỏ hàng: <node-url>
- Step 2 — Thanh toán: <node-url>
- Step 3 — Hoàn tất: <node-url>

## Tiến độ giai đoạn

- [ ] A — Setup
- [ ] B — Tokens
- [ ] C — Inventory
- [ ] D — Shared components
- [ ] E — Golden page
- [ ] F — Fan-out các trang
- [ ] G — SEO
- [ ] H — Responsive
- [ ] I — QA
- [ ] J — Build & deploy

## Quyết định scope (chốt qua họp — những gì Figma không thể hiện)

- **Mobile:** làm theo Reflow Rules trong CLAUDE.md. Ngoại lệ: ____
- **States** (hover/focus/empty/error/loading): ____
- **Form** (gửi đi đâu / validation): ____
- **Nội dung** (tĩnh/CMS, ai cấp): ____
- **i18n / dark mode / analytics:** ____
- **Nghiệm thu:** sai số desktop <TOLERANCE_PX>px; Lighthouse SEO ≥ __.

## Cần hỏi / chờ quyết

- [ ] (vd) màu lẻ ở trang X gộp vào token nào?
- [ ] ...
