# Playbook: Figma → Code với Claude Code

> Quy trình tái sử dụng để chuyển một thiết kế Figma (không có design system) thành
> một website Next.js chuẩn SEO + responsive, dùng Claude Code làm đòn bẩy.
> Áp dụng được cho nhiều dự án; điều chỉnh quy mô theo số trang & deadline.

---

## 0. Triết lý cốt lõi (đọc trước, quyết định mọi thứ sau)

1. **Phân vai đúng:** *Người* làm kiến trúc + design system + QA. *Claude* cày phần
   lặp lại và làm song song. Đừng giao máy quyết định "gốc", đừng tự tay làm phần "cày".
2. **Design là Ý ĐỒ THỊ GIÁC, không phải toạ độ.** Luôn tái dựng layout bằng
   flex/grid; toạ độ x/y của Figma chỉ là tham khảo.
3. **Nguồn chân lý sống trên ĐĨA, không trong hội thoại.** Mọi quyết định quan trọng
   ghi ra file để bất kỳ session/agent nào cũng đọc lại được.
4. **Golden page trước, fan-out sau.** Làm 1 trang hoàn chỉnh, khoá "khuôn", rồi mới
   nhân bản. Fan-out sớm = 20 trang lệch chuẩn.
5. **Tin số liệu, không tin cảm giác.** Verify bằng screenshot diff, không bằng
   "trông có vẻ đúng".
6. **Đơn giản hóa markup.** Không copy nguyên cấu trúc div-lồng-div của Figma.

---

## 1. Pre-flight checklist (TRƯỚC khi gõ dòng code đầu tiên)

### 1.1 Khảo sát file Figma
- [ ] Bao nhiêu trang / frame thật sự cần làm?
- [ ] Có những breakpoint nào? (chỉ desktop? có mobile/tablet không?)
- [ ] Tỷ lệ frame dùng auto-layout vs không? (quyết định độ khó tái dựng)
- [ ] Chất lượng đặt tên layer (có dùng được để định danh component không?)
- [ ] Font dùng gì? Có license/quyền dùng không? Có trên Google Fonts không?
- [ ] Có Variables / Styles không? (nếu có thì rút token nhanh hơn nhiều)
- [ ] Assets: icon (map sang lucide hay phải export?), ảnh, logo, illustration.
- [ ] Có dark mode / theme / nhiều màu không?

### 1.2 Chốt SCOPE với khách (đây là nơi dễ "vỡ hợp đồng" nhất)
- [ ] **Mobile**: có thiết kế mobile không? Nếu không → ai chịu trách nhiệm layout
      mobile? (mặc định: dev tự thiết kế theo reflow rules, chốt qua họp)
- [ ] **Các state không được vẽ**: hover, focus, active, disabled, empty, error,
      loading, success — Figma thường thiếu. Chốt cách xử lý.
- [ ] **Nội dung**: tĩnh hay động? Có CMS không? Text dài/ngắn thực tế (overflow)?
- [ ] **Tương tác**: form gửi đi đâu? validation? animation/motion?
- [ ] **i18n / đa ngôn ngữ?**
- [ ] **Tiêu chí nghiệm thu "đúng thiết kế"**: verify ở breakpoint nào, sai số (px)
      chấp nhận được là bao nhiêu.

### 1.3 Chuẩn bị kỹ thuật
- [ ] Mở Figma **desktop app** (MCP cần app chạy để đọc node).
- [ ] Thu thập **node URL chính xác** của từng trang + **2–4 frame nguồn token** (phủ nhiều kiểu: home/nội dung/form).
- [ ] Cài & test Figma MCP (`get_metadata`, `get_screenshot`, `get_design_context`).

---

## 2. Quyết định kiến trúc (chốt một lần, ghi vào CLAUDE.md)

| Hạng mục | Lựa chọn mặc định | Ghi chú |
|---|---|---|
| Framework | Next.js LTS, App Router | RSC tốt cho SEO |
| Package manager | pnpm + pin version | commit lockfile |
| Ngôn ngữ | TypeScript strict | |
| Styling | Tailwind + CSS variables cho token | cấm hardcode |
| Icon | lucide-react | icon đặc thù mới export từ Figma |
| Font | next/font | tự host, tránh CLS |
| Ảnh | next/image | export @2x, SVG cho vector |
| Nội dung | tĩnh / MDX / CMS | theo scope 1.2 |
| Component | `ui/` (primitive), `layout/`, `sections/` (khối trang) + barrel `index.ts` | danh mục = chính thư mục này |
| Deploy | Vercel / tùy khách | |

---

## 3. Các file "xương sống" (bộ nhớ bền vững giữa các session)

> Triết lý: giữ TỐI THIỂU. Token là CSS (`globals.css` `@theme`, Tailwind v4), danh mục component
> là chính thư mục `components/` + barrel `index.ts`. Đừng dựng file markdown rỗng
> rồi ép điền — chỉ cần **2 file** thật sự:

| File | Vai trò | Ai sở hữu |
|---|---|---|
| `CLAUDE.md` | Quy ước + **luật cứng** + reflow rules + page recipe | Người viết, máy tuân |
| `PROJECT.md` | Trang + node URL + tiến độ + quyết định scope (gộp pages/progress/assumptions) | Người điền trang & scope; mọi session cập nhật tiến độ |

Nguồn chân lý khác (KHÔNG phải file markdown riêng):
- **Token** → `globals.css` khối `@theme` (Tailwind v4; máy sinh ở giai đoạn B, **người duyệt**).
- **Danh mục component** → thư mục `components/` + barrel `components/**/index.ts`
  (đọc 1 file `index.ts` là thấy hết component đang có; không bao giờ lệch với code).

> **Luật vàng cho mô hình "mỗi bước = 1 session":** mở đầu mọi session, bắt Claude
> đọc `CLAUDE.md` + `PROJECT.md`, xem token trong `globals.css` (`@theme`) và liệt kê
> `components/` trước khi làm bất cứ gì.

---

## 4. Quy trình theo giai đoạn

| # | Giai đoạn | Việc giao Claude | Output | Exit criteria (xong khi…) |
|---|---|---|---|---|
| A | **Setup** | Init project, viết CLAUDE.md + PROJECT.md, settings.json (allowlist + hook format), git init, pin version | Project chạy + CLAUDE.md + PROJECT.md | `pnpm dev` chạy, commit đầu tiên |
| B | **Design tokens** | Đọc 2–4 frame nguồn token, trích & gom màu/typo/spacing/radius/shadow thành scale | globals.css (@theme, Tailwind v4) | **Người duyệt token bằng mắt** |
| C | **Component inventory** | Screenshot các trang, cluster theo HÌNH DẠNG (bỏ qua tên Figma), đặt tên chuẩn, tạo stub + barrel | `components/**/index.ts` (danh mục component) | **Người chốt tên & gộp trùng** |
| D | **Shared layout + primitives** | Build Header/Footer/Nav/Button/Card/Container/Section (+ index.ts), layout.tsx, font, metadata gốc | components/ui, components/layout | Render thử ổn ở trang demo |
| E | **Golden page** | 1 trang end-to-end: match desktop, tự dựng responsive, SEO metadata | 1 trang + "page recipe" thêm vào CLAUDE.md | **Người review cực kỹ — khoá khuôn** |
| F | **Fan-out** | Subagent + git worktree, mỗi agent 1 trang theo recipe; chia ~3 batch | Các trang còn lại | Mỗi batch: build pass, cập nhật PROJECT.md, commit |
| G | **SEO toàn cục** | sitemap.ts, robots.ts, JSON-LD, audit metadata/heading/alt | File SEO + báo cáo | Lighthouse SEO ≥ 95 |
| H | **Responsive pass** | Áp & kiểm reflow mobile/tablet theo rules (không đụng desktop) | Trang co giãn đúng | Xem thủ công 3 viewport |
| I | **QA visual diff** | Playwright chụp desktop, so screenshot Figma, liệt kê lệch & fix | Báo cáo diff + fix | Khớp desktop trong ngưỡng tolerance |
| J | **Build & deploy** | pnpm build, fix SSR/TS, Lighthouse perf/a11y, deploy | Bản build + URL | Build sạch, các chỉ số đạt |

---

## 5. Luật cứng đưa vào CLAUDE.md (áp cho mọi prompt sinh code)

```
- KHÔNG hardcode màu/spacing/font-size — chỉ dùng token trong globals.css @theme (Tailwind v4).
- KHÔNG position:absolute, trừ phần tử chồng lớp/trang trí thực sự.
  Frame không auto-layout: tái dựng bằng flex/grid từ ý đồ thị giác, KHÔNG lấy toạ độ.
- KHÔNG tự tạo lại component đã có: kiểm tra components/ + barrel index.ts trước khi
  viết JSX; cần khác đi → thêm variant/prop, KHÔNG copy thành component mới.
- KHÔNG tự cài thư viện ngoài whitelist (Next, Tailwind, lucide-react).
- Author Tailwind mobile-first; điểm neo đúng/sai là desktop (bản thiết kế gốc).
- Semantic HTML + heading hierarchy + alt + aria + đảm bảo contrast & keyboard nav.
- Mỗi page export generateMetadata (title, description, OG).
- Đơn giản hóa markup — không copy div-lồng-div của Figma.
- Xử lý nội dung dài/overflow, không chỉ nội dung mẫu trong Figma.
- TypeScript strict, không any.
```

---

## 6. Dùng Claude Code hiệu quả

- **Plan mode** trước **E (golden page)** và **F (fan-out)** — chốt cách làm trước khi khoá khuôn / nhân bản 19 trang. (B, C đã có cổng review qua mô hình 2-prompt nên không cần plan mode riêng.)
- **Subagent + `isolation: worktree`** cho fan-out nhiều trang song song, tránh xung đột file.
- **Workflow** khi cần orchestrate vòng lặp xác định (build N trang → verify từng trang).
- **Custom slash command / skill** "build-one-page" để chuẩn hóa recipe, gọi lại nhiều lần.
- **Hooks**: chạy prettier/eslint --fix sau mỗi edit → code luôn sạch, không tốn turn.
- **settings.json allowlist** (pnpm, git, MCP read-only) → giảm permission prompt khi fan-out.
- **`/code-review`** sau mỗi batch; **`/security-review`** trước khi deploy.
- **Context hygiene:** 1 session = 1 mục tiêu; `/clear` khi đổi việc; chia batch để không cạn context.
- **Commit checkpoint** sau mỗi bước = mạng an toàn duy nhất giữa các session.

### Prompt mở đầu mọi session
```
Đọc CLAUDE.md + PROJECT.md trước. Xem token trong globals.css @theme và liệt kê components/ (đọc barrel index.ts).
Tuân thủ tuyệt đối luật cứng trong CLAUDE.md.
Nhiệm vụ session này: <…>
Xong thì cập nhật PROJECT.md và đề xuất commit.
```

### Prompt fan-out (giai đoạn F)
```
Với mỗi trang trong batch [danh sách], spawn 1 subagent trong git worktree riêng. Mỗi agent:
1. get_metadata + get_design_context + get_screenshot (desktop) theo node URL trong PROJECT.md.
2. Frame KHÔNG auto-layout: tái dựng bằng flex/grid từ ý đồ thị giác, KHÔNG lấy toạ độ.
3. Tái dùng component sẵn có (đọc components/ + barrel index.ts), không tạo trùng; cần khác → thêm variant/prop.
4. Thêm generateMetadata; author mobile-first, neo desktop.
5. Tự screenshot & so Figma desktop, tự sửa cho khớp.
Gộp worktree xong cập nhật PROJECT.md.
```

---

## 7. SEO checklist
- [ ] `generateMetadata` mỗi trang (title, description, canonical, OG, Twitter).
- [ ] `app/sitemap.ts` + `app/robots.ts`.
- [ ] JSON-LD: Organization, WebSite, BreadcrumbList (+ phù hợp loại trang).
- [ ] Semantic HTML + 1 `<h1>`/trang + heading đúng thứ bậc.
- [ ] `next/image` (alt đầy đủ) + `next/font` (tránh CLS).
- [ ] Internal linking, slug sạch, lang attribute.
- [ ] Lighthouse SEO ≥ 95; test với JS tắt (RSC render được).

## 8. Responsive checklist
- [ ] Reflow rules đã chốt: grid collapse, nav→hamburger, hero stack, table→card.
- [ ] Typography/spacing co giãn bằng `clamp()` thay vì nhảy bậc thô.
- [ ] Test 360 / 768 / 1024 / 1440. Không scroll ngang ngoài ý muốn.
- [ ] Touch target ≥ 44px; ảnh responsive (`sizes`).

## 9. QA / verification checklist
- [ ] Visual diff desktop: screenshot thật vs Figma, từng trang.
- [ ] Kiểm token: màu/spacing/font có khớp globals.css @theme không (không lệch ngầm).
- [ ] State: hover/focus/empty/error đã làm theo scope.
- [ ] a11y: contrast, keyboard, focus ring, aria.
- [ ] Cross-browser cơ bản; build SSR không lỗi hydration.

---

## 10. ⚠️ Các sai lầm thường gặp (anti-patterns) — phần quan trọng nhất

1. **Fan-out trước khi có tokens + golden page** → 20 trang mỗi trang một kiểu.
   *Tránh:* khoá B, C, E trước khi sang F.
2. **Dịch thô absolute positioning** từ frame không auto-layout → vỡ responsive, markup rác.
   *Tránh:* luật cấm absolute; tái dựng từ ý đồ.
3. **Tin tên layer Figma** để định danh/gộp component (frame1, trùng tên) → gộp nhầm/tạo trùng.
   *Tránh:* inventory bằng mắt, tên do người đặt.
4. **Hardcode px/màu** thay vì token → không sửa nổi, không nhất quán.
   *Tránh:* luật cứng + review token kỹ.
5. **Giả định mobile-first khi chỉ có desktop** → làm sai kỳ vọng, hoặc bỏ ngỏ scope mobile.
   *Tránh:* desktop là spec; mobile là deliverable tự thiết kế, chốt scope qua họp (ghi PROJECT.md).
6. **Nhồi cả dự án vào 1 session** → cạn context, chất lượng tụt, hallucination.
   *Tránh:* 1 session 1 mục tiêu, chia batch, subagent gánh nặng.
7. **Không lưu state ra file** → session mới mất trí nhớ, làm lại/làm lệch.
   *Tránh:* lưu state vào CLAUDE.md (luật) + PROJECT.md (trang/tiến độ/scope).
8. **Không verify bằng screenshot** → "trông giống" nhưng lệch spacing/màu/font.
   *Tránh:* giai đoạn I bắt buộc.
9. **Để Claude tự cài thư viện** (UI kit, animation lib) → bloat, lệch design.
   *Tránh:* whitelist trong luật cứng.
10. **Dùng font gần giống thay font thật** → sai toàn bộ vertical rhythm & spacing.
    *Tránh:* xác nhận font + license ở pre-flight.
11. **Export asset sai** (PNG cho thứ nên là SVG, thiếu @2x) → mờ/nặng.
    *Tránh:* SVG cho vector, raster @2x, tối ưu trước khi import.
12. **Quên các state không vẽ** (hover/empty/error/loading) → thiếu, đoán bừa.
    *Tránh:* chốt ở scope, ghi PROJECT.md.
13. **Bỏ accessibility** khi mải copy pixel → contrast/alt/keyboard hỏng.
    *Tránh:* a11y nằm trong luật cứng + checklist QA.
14. **Làm SEO ở bước cuối** → phải sửa lại metadata cho 20 trang.
    *Tránh:* generateMetadata ngay từ golden page (E).
15. **Không pin version** (Node/pnpm/Next) → build trôi giữa các session.
    *Tránh:* pin + commit lockfile ở giai đoạn A.
16. **Đòi pixel-perfect tuyệt đối** → tốn thời gian vô ích vì lệch 1px.
    *Tránh:* đặt ngưỡng tolerance; ưu tiên đúng scale hơn đúng từng pixel.
17. **Copy nguyên DOM lồng sâu của Figma** → markup khó bảo trì.
    *Tránh:* luật "đơn giản hóa markup".
18. **Bỏ qua dark mode/theme** nếu design có → làm lại từ đầu.
    *Tránh:* phát hiện ở pre-flight, đưa vào token (CSS variables) từ giai đoạn B.
19. **Chỉ test với nội dung mẫu** → text dài thật làm vỡ layout.
    *Tránh:* test overflow trong QA.
20. **Không có checkpoint git** → một bước hỏng đầu độc cả chuỗi.
    *Tránh:* commit sau mỗi giai đoạn.

---

## 11. Definition of Done
- [ ] Tất cả trang khớp desktop trong ngưỡng tolerance đã chốt.
- [ ] Responsive đạt ở mọi breakpoint mục tiêu.
- [ ] Lighthouse: SEO ≥ 95, Performance & A11y đạt ngưỡng.
- [ ] `pnpm build` sạch, không lỗi hydration.
- [ ] PROJECT.md: tất cả trang = Xong; quyết định scope đã chốt.
- [ ] `/code-review` & `/security-review` đã chạy và xử lý.
