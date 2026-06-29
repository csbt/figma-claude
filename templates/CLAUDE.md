<!--
  TEMPLATE — copy file này vào ROOT của dự án thật rồi điền các <PLACEHOLDER>.
  Đây là "hiến pháp" của dự án: mọi session/agent phải đọc & tuân thủ.
-->

# <PROJECT_NAME> — Hướng dẫn dự án (Figma → Code)

- **Khách hàng:** <CLIENT> · **Deadline:** <DEADLINE>
- **Figma:** <FIGMA_FILE_URL> — bản gốc chỉ có **desktop** (rộng <DESIGN_WIDTH>px) → desktop là SPEC.
  Mobile/tablet là layout dev tự thiết kế theo *Reflow Rules* (mục 4). Trạng thái &
  quyết định scope: xem `PROJECT.md`.

---

## 0. Giao thức mở đầu MỌI session (làm trước tiên)

1. Đọc `CLAUDE.md` (file này) và `PROJECT.md`.
2. Xem token hiện có trong `globals.css` (khối `@theme`, Tailwind v4); xem component hiện có bằng cách liệt
   kê `components/` và đọc barrel `components/**/index.ts`.
3. Làm đúng một mục tiêu được giao, không "tiện tay" làm thêm.
4. Xong → cập nhật `PROJECT.md` (tiến độ/trang) và đề xuất commit.

---

## 1. Tech stack (đã pin — KHÔNG đổi version)

| | |
|---|---|
| Framework | Next.js <NEXT_VERSION> (App Router, RSC) |
| Package manager | pnpm <PNPM_VERSION> (commit lockfile) · Node <NODE_VERSION> |
| Ngôn ngữ | TypeScript strict, không `any` |
| Styling | Tailwind + CSS variables (token) |
| Icon | lucide-react (icon đặc thù mới export từ Figma) |
| Font / Ảnh | next/font (tự host) · next/image |
| Whitelist thư viện | Next, Tailwind, lucide-react. **Cài thêm gì phải hỏi.** |

---

## 2. Cấu trúc thư mục

```
app/                # routes; mỗi page có generateMetadata; sitemap.ts; robots.ts
components/
  ui/               # primitive: Button, Card, Container, Section…  + index.ts (barrel)
  layout/           # Header, Footer, Nav                            + index.ts
  sections/         # khối trang dùng lại: Hero, Features, CTA…       + index.ts
lib/                # utils, jsonld, metadata helpers
```
> Mỗi thư mục component có `index.ts` re-export tất cả → đây chính là "danh mục
> component" sống. Đọc file này là biết có sẵn gì, khỏi cần file inventory riêng.

---

## 3. ⚠️ HARD RULES (áp cho mọi dòng code sinh ra)

- **Luật leo thang — DỪNG & HỎI.** Khi (a) yêu cầu/thiết kế mơ hồ, (b) MCP/tool trả rỗng
  hoặc mâu thuẫn, (c) đã sửa **2–3 vòng** vẫn chưa đạt, hoặc (d) phải đoán một quyết định
  "nguồn chân lý" (token / tên component / scope) → **DỪNG, nêu rõ cái đang chặn + phương án,
  rồi HỎI người**. TUYỆT ĐỐI không bịa component/token/nội dung để "cho xong". Kẹt kỹ thuật →
  xem `TROUBLESHOOTING.md` (cùng thư mục với file này).
- **Token-only.** KHÔNG hardcode màu/spacing/font-size/radius — chỉ dùng token trong
  `globals.css` (khối `@theme`, Tailwind v4). Gặp giá trị lẻ chưa có → ghi vào `PROJECT.md` mục "cần hỏi", không tự thêm.
- **Tái dùng component, không tạo trùng.** Trước khi viết JSX: liệt kê `components/`
  và đọc barrel `index.ts`. Nếu đã có component phù hợp → import dùng lại. Cần khác đi
  → **thêm prop/variant**, KHÔNG copy thành component mới. Chỉ tạo mới khi thật sự chưa có,
  và phải export qua `index.ts` + ghi vào `PROJECT.md`.
- **Cấm `position: absolute`**, trừ phần tử chồng lớp/trang trí. Frame Figma KHÔNG
  auto-layout → tái dựng bằng flex/grid từ ý đồ thị giác, KHÔNG bê toạ độ x/y.
- **Mobile-first kỹ thuật, neo desktop về độ đúng.** Base = mobile reflow; `lg:`/`xl:`
  cho bản desktop khớp thiết kế gốc.
- **Đơn giản hóa markup** (không copy div-lồng-div của Figma). Semantic HTML.
- **A11y:** 1 `<h1>`/trang, heading đúng bậc, `alt`, `aria-*`, contrast AA, focus rõ, keyboard.
- **SEO:** mỗi page export `generateMetadata` (title, description, canonical, OG).
- **Nội dung thực tế:** chịu được text dài/ngắn & overflow, không chỉ nội dung mẫu.
- **TypeScript strict.** Không `any`, không `@ts-ignore` nếu chưa hỏi.

---

## 4. Reflow Rules (mobile/tablet — vì Figma chỉ có desktop)

- Breakpoints: mobile `<768`, tablet `768–1023`, desktop `≥1024`. Điểm neo: <DESIGN_WIDTH>px.
- Grid nhiều cột: desktop N → tablet 2 → mobile 1 cột.
- Nav ngang → hamburger (drawer) ở `<lg`.
- Hero 2 cột (text | media) → stack dọc, text trước.
- Typography & spacing co giãn bằng `clamp()`, không nhảy bậc thô.
- Bảng → card stack ở mobile. Touch target ≥ 44px. Không scroll ngang ngoài ý muốn.
> Thay đổi so với quy ước này → ghi vào `PROJECT.md` mục "Quyết định scope".

---

## 5. 📋 PAGE RECIPE — quy trình build 1 trang (dùng lại cho mọi trang)

> `/build-one-page` gọi đúng quy trình này. Làm tuần tự, không bỏ bước.

1. **Khám phá trước khi build:** liệt kê `components/` + đọc barrel `index.ts` để biết
   component nào tái dùng được. Xem token trong `globals.css` (khối `@theme`).
2. **Lấy ngữ cảnh Figma** theo node URL của trang (trong `PROJECT.md`):
   `get_metadata` → `get_screenshot` (desktop) → `get_design_context`.
3. **Phân loại layout** (`layoutMode`): auto-layout → flex/gap; không auto-layout →
   tái dựng từ screenshot, KHÔNG lấy toạ độ.
4. **Lắp ráp:** tái dùng component sẵn có; cần biến thể → thêm variant/prop; chỉ tạo
   mới khi chưa có (export qua `index.ts`, ghi `PROJECT.md`). Chỉ dùng token.
5. **Áp Reflow Rules** cho tablet/mobile.
6. **SEO:** `generateMetadata`; semantic HTML; alt/aria.
7. **Tự verify:** screenshot trang (desktop) so với Figma; liệt kê điểm lệch & sửa
   trong ngưỡng tolerance <TOLERANCE_PX>px.
8. **Asset:** icon → lucide; vector → SVG; raster → @2x + next/image.
9. **Cập nhật `PROJECT.md`** (đánh dấu trang xong + component mới nếu có) → đề xuất commit.

---

## 6. Definition of Done (mỗi trang)
- [ ] Khớp desktop trong ngưỡng <TOLERANCE_PX>px; reflow đúng mobile & tablet.
- [ ] `generateMetadata` đầy đủ; heading/alt/aria ổn.
- [ ] Không hardcode token; không absolute (trừ overlay); **không component trùng**.
- [ ] `pnpm build` không lỗi; `PROJECT.md` đã cập nhật.
