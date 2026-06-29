# Figma → Code Kit (Next.js + Claude Code)

Bộ **tài liệu + template tái sử dụng** để chuyển một thiết kế Figma (không có design system)
thành website **Next.js chuẩn SEO + responsive**, dùng Claude Code làm đòn bẩy — tối ưu cho
mô hình **"mỗi giai đoạn = 1 session mới"**.

Stack mặc định: Next.js LTS (App Router) · pnpm · TypeScript · **Tailwind v4** (token ở `globals.css @theme`) · lucide-react · Playwright MCP để verify.

---

## Cấu trúc bộ kit

```
figma-claude/
├── README.md                       # ← file này (điểm vào)
├── FIGMA-TO-CODE-PLAYBOOK.md       # CHIẾN LƯỢC: triết lý, quy trình A→J, 20 cạm bẫy
├── guides/                         # HƯỚNG DẪN THỰC THI chi tiết từng giai đoạn
│   ├── README.md                   #   cách dùng bộ guides
│   ├── A-setup/ … J-deploy/        #   mỗi folder: guide.md · prompt.md · instructions.md
└── templates/                      # FILE COPY VÀO DỰ ÁN
    ├── CLAUDE.md                   #   "hiến pháp" dự án (luật + reflow + page recipe)
    ├── PROJECT.md                  #   trạng thái: trang + tiến độ + scope
    ├── TROUBLESHOOTING.md          #   failure modes + fallback (Claude tra khi kẹt)
    └── .claude/commands/build-one-page.md   # slash command /build-one-page
```

**3 file trong mỗi folder `guides/<phase>/`:**
- `guide.md` — cho **NGƯỜI**: mục tiêu, việc bạn làm, điểm review, cạm bẫy.
- `prompt.md` — block **dán vào session** (có giai đoạn nhiều prompt: review-gated / vòng lặp / fan-out).
- `instructions.md` — cho **CLAUDE**: các bước mệnh lệnh + self-check + "KHÔNG làm".

---

## File nào đi đâu

| Nhóm | File | Vai trò | Đưa vào dự án? |
|---|---|---|---|
| Chiến lược | `FIGMA-TO-CODE-PLAYBOOK.md` | Đọc để nắm tổng thể + danh sách cạm bẫy (§10) | Tuỳ chọn (để cũng tốt) |
| Hướng dẫn | `guides/**` | Prompt cần đọc `instructions.md` khi chạy | **CÓ** — copy cả thư mục |
| Vận hành | `templates/CLAUDE.md`, `PROJECT.md`, `TROUBLESHOOTING.md` | Claude đọc & tuân mỗi session | **CÓ** — ra **root** dự án |
| Vận hành | `templates/.claude/commands/build-one-page.md` | Slash command | **CÓ** — ra `.claude/` dự án |

> Quy ước: `templates/*` = "đổ ra **root** dự án". `guides/` = giữ nguyên thư mục trong dự án.

---

## Triển khai vào dự án thật

### Bước 1 — Tạo app Next.js (thư mục RỖNG)
```bash
pnpm dlx create-next-app@latest my-site \
  --typescript --eslint --app --tailwind --src-dir --import-alias "@/*" --use-pnpm
```

### Bước 2 — Copy kit vào dự án
Đặt `<KIT>` = đường dẫn tới thư mục `figma-claude` này.
```bash
cp <KIT>/templates/*.md my-site/            # CLAUDE.md + PROJECT.md + TROUBLESHOOTING.md → root
cp -r <KIT>/templates/.claude my-site/      # .claude/commands/build-one-page.md
cp -r <KIT>/guides my-site/                 # prompt cần đọc instructions
cp <KIT>/FIGMA-TO-CODE-PLAYBOOK.md my-site/ # tuỳ chọn
cd my-site
```
> Phải `create-next-app` **trước** rồi mới copy (create-next-app từ chối thư mục đã có file lạ).

### Bước 3 — Pre-flight (việc của bạn, làm tay)
1. `CLAUDE.md`: điền `<PROJECT_NAME>`, `<DESIGN_WIDTH>`, `<TOLERANCE_PX>`, `<FIGMA_FILE_URL>`, version.
2. `PROJECT.md`: điền **PHẦN 1 — Hồ sơ** (chi tiết ở mục "Chỉnh PROJECT.md" ngay dưới).
3. Mở Figma **desktop**; cài/kiểm Figma MCP + Playwright MCP.

#### Chỉnh PROJECT.md — giữ gì / sửa gì / xoá gì

`PROJECT.md` chia **2 phần**: PHẦN 1 bạn điền, PHẦN 2 để Claude tự cập nhật. Quy ước ký hiệu trong file:

| Ký hiệu | Nghĩa | Bạn làm gì |
|---|---|---|
| `<...>` (ngoặc nhọn) | chỗ giữ chỗ | **THAY** bằng giá trị thật, xoá ngoặc |
| `<!-- ... -->` | ghi chú hướng dẫn (ẩn khi render) | đọc để biết cách điền → **xoá được** sau khi xong |
| dòng ví dụ (home/about/pricing…) | minh hoạ | **THAY** bằng trang thật, **xoá** dòng thừa |

**PHẦN 1 — bạn điền 1 lần:**
- `# <PROJECT_NAME>` → tên dự án · `<FIGMA_FILE_URL>` → link file Figma.
- **Trang** → mỗi trang 1 dòng: slug · route · link node `[↗ slug](url)` · để Trạng thái = `Chưa làm`.
- **Frame nguồn token** → 2–4 link frame phủ nhiều kiểu (home / nội dung nhiều chữ / form).
- **Trang nhiều trạng thái** → chỉ giữ nếu có tab/step; không có thì **xoá cả mục**.
- **Quyết định scope** → điền sau buổi họp (mỗi `____`).
- **Cần hỏi** → để trống lúc đầu.

**PHẦN 2 — KHÔNG đụng tay:** "Tiến độ giai đoạn" + cột "Trạng thái" ở bảng Trang do Claude cập nhật mỗi session.

> **Tóm tắt:** GIỮ heading + bảng + checklist · SỬA mọi `<...>` và dòng ví dụ · XOÁ các `<!-- -->` (và mục "nhiều trạng thái" nếu không dùng).

### Bước 4 — Chạy lần lượt A → J (mỗi giai đoạn 1 session mới)
Với mỗi folder theo thứ tự:
1. Đọc `guides/<phase>/guide.md`.
2. Mở `guides/<phase>/prompt.md`, điền `<…>`, **dán vào session Claude Code mới**.
3. Claude đọc `guides/<phase>/instructions.md` và thực thi.
4. Review theo "Điểm REVIEW" trong `guide.md` → **commit** → sang folder kế.

> Giai đoạn **A**: bạn đã tự `create-next-app` + copy kit ở Bước 1–2 (chính là A bước 1 & 5).
> Khi dán prompt A, thêm: *"App đã init và kit đã copy, làm tiếp các bước còn lại của A."*

### Layout cuối trong dự án
```
my-site/
├── CLAUDE.md   ├── PROJECT.md   ├── TROUBLESHOOTING.md     # cùng root
├── .claude/commands/build-one-page.md
├── guides/ …                                              # tham chiếu khi chạy
├── src/app/globals.css   (@theme = token)
├── src/components/{ui,layout,sections}/index.ts           # danh mục component
└── … (Next.js)
```

---

## Thứ tự giai đoạn & mô hình prompt

```
A Setup → B Tokens → C Inventory → D Shared components → E Golden page (KHOÁ KHUÔN)
       → F Fan-out → G SEO → H Responsive → I QA → J Build & deploy
```

| Loại | Giai đoạn | Cách chạy |
|---|---|---|
| One-shot | A, G, J | 1 prompt (có thể dừng-HỎI) |
| Review-gated | B, C | 1 prompt build + (tuỳ) 1 prompt sửa sau khi bạn review |
| Vòng lặp | E, H, I | 1 prompt dựng + N prompt sửa theo diff |
| Fan-out | F | Plan (1 lần) → 1 prompt/batch (×~3) → 1 prompt dedup |

E và F có thêm **Plan mode** (duyệt cách làm trước khi khoá khuôn / nhân bản trang).

---

## Khi cần tra cứu
- **Kẹt lúc thực thi** → `TROUBLESHOOTING.md` (triệu chứng → fallback) + luật leo thang trong `CLAUDE.md`.
- **Tổng thể & cạm bẫy** → `FIGMA-TO-CODE-PLAYBOOK.md` (đặc biệt §10).
- **Cách dùng guides** → `guides/README.md`.

---

## Nguyên tắc cốt lõi (rút gọn)
1. Người làm kiến trúc + design system + QA; Claude cày phần lặp + làm song song.
2. Design là **ý đồ thị giác**, không phải toạ độ → tái dựng bằng flex/grid.
3. Nguồn chân lý sống trên **đĩa** (`CLAUDE.md`, `PROJECT.md`, code), không trong hội thoại.
4. **Golden page trước, fan-out sau** — khoá khuôn rồi mới nhân bản.
5. Verify bằng **screenshot diff**, không tin "trông có vẻ đúng".
6. Kẹt/mơ hồ → **DỪNG & HỎI**, không đoán bừa (luật leo thang).
