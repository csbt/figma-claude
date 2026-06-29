<!--
  ══════════════════════════════════════════════════════════════════════════
  PROJECT.md — FILE TRẠNG THÁI DUY NHẤT CỦA DỰ ÁN
  ══════════════════════════════════════════════════════════════════════════
  Cách điền (chi tiết: README → "Chỉnh PROJECT.md khi vào dự án thật"):
    • <...>            = chỗ giữ chỗ → THAY bằng giá trị thật rồi xoá ngoặc nhọn.
    • <!-- ... --\>     = ghi chú hướng dẫn, KHÔNG hiện khi render → xoá được sau khi điền.
    • dòng ví dụ (home/about/pricing/checkout) = minh hoạ → thay bằng trang thật, xoá dòng thừa.

  File gồm 2 PHẦN:
    PHẦN 1 — HỒ SƠ : bạn điền 1 lần từ Figma + buổi họp (đầu vào, ít đổi).
    PHẦN 2 — TIẾN ĐỘ: Claude tự cập nhật sau mỗi session (bạn chỉ đọc).

  Token KHÔNG ghi ở đây (nằm trong globals.css @theme). Component KHÔNG liệt kê ở đây
  (xem thư mục components/ + barrel index.ts).
-->

# <PROJECT_NAME>

- Figma: [mở file](<FIGMA_FILE_URL>) — **bản gốc chỉ có desktop → desktop là SPEC**.
- Cập nhật lần cuối: ____

---

## 📋 PHẦN 1 — HỒ SƠ DỰ ÁN (bạn điền)

### Trang

<!--
  Cột "Figma": dán dạng [↗ slug](URL-node) — render chỉ hiện "↗ slug" bấm được, URL nằm
  cùng dòng nên Claude lấy đúng. Lấy URL: chuột phải node trong Figma → Copy link to selection.
  Cột "Trạng thái": để "Chưa làm"; CLAUDE sẽ tự đổi sang Đang làm/Xong (đừng sửa tay).
-->

| Slug | Route | Figma | Trạng thái |
|---|---|---|---|
| home | `/` | [↗ home](<node-url>) | Chưa làm |
| about | `/about` | [↗ about](<node-url>) | Chưa làm |
| ... | | | Chưa làm |

> Trạng thái: `Chưa làm` · `Đang làm` · `Xong`

### Frame nguồn token

<!--
  2–4 frame phủ nhiều KIỂU khác nhau (chọn theo độ đa dạng, KHÔNG phải trang to nhất) để
  giai đoạn B trích token màu/chữ/spacing. Không cần hoàn hảo: B review-gated, token được phép
  lớn dần (gặp giá trị lẻ ở E/F → Claude ghi vào "Cần hỏi", không bịa).
  LƯU Ý: danh mục COMPONENT không lấy từ đây — giai đoạn C quét TOÀN BỘ trang ở bảng trên.
-->

- Home — header · footer · hero · nav · button · card: [↗](<node-url>)
- Trang nhiều chữ (About/Blog) — typography, heading nhiều bậc, list: [↗](<node-url>)
- Trang có form (Contact/Signup) — input · select · checkbox · textarea: [↗](<node-url>)
- (nếu có) Trang đặc thù (pricing table, dashboard…): [↗](<node-url>)

### Trang nhiều trạng thái / tab / step

<!--
  CHỈ dùng khi 1 ROUTE ứng với NHIỀU frame Figma. Nếu dự án không có → xoá cả mục này.
  • Các state CÙNG một URL (tab/step đổi nội dung tại chỗ) → giữ 1 DÒNG ở bảng Trang,
    liệt kê các frame ở đây. Claude build 1 trang + cơ chế state, KHÔNG tách nhiều trang.
  • Mỗi state là URL KHÁC (vd /checkout/cart, /checkout/payment) → tách thành nhiều
    DÒNG RIÊNG ở bảng Trang, không dùng mục này.
-->

**`/pricing`** — tab, cùng URL (state ở client):
- Tab "Monthly": [↗](<node-url>)
- Tab "Yearly": [↗](<node-url>)

**`/checkout`** — step, cùng URL (state nội bộ):
- Step 1 — Giỏ hàng: [↗](<node-url>)
- Step 2 — Thanh toán: [↗](<node-url>)
- Step 3 — Hoàn tất: [↗](<node-url>)

### Quyết định scope

<!-- Những gì Figma KHÔNG thể hiện — chốt qua họp với khách. Điền vào sau mỗi ____ -->

- **Mobile:** làm theo Reflow Rules trong CLAUDE.md. Ngoại lệ: ____
- **States** (hover/focus/empty/error/loading): ____
- **Form** (gửi đi đâu / validation): ____
- **Nội dung** (tĩnh/CMS, ai cấp): ____
- **i18n / dark mode / analytics:** ____
- **Nghiệm thu:** sai số desktop <TOLERANCE_PX>px; Lighthouse SEO ≥ ____

### Cần hỏi / chờ quyết

<!-- Để trống lúc đầu. Claude cũng ghi vào đây khi gặp giá trị lẻ / quyết định mơ hồ. -->

- [ ] (vd) màu lẻ ở trang X gộp vào token nào?
- [ ] ...

---

## 📊 PHẦN 2 — TIẾN ĐỘ (Claude cập nhật mỗi session — bạn chỉ đọc)

### Tiến độ giai đoạn

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
