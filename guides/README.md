# Guides — Hướng dẫn chi tiết từng giai đoạn (A→J)

Bộ hướng dẫn thực thi cho `../FIGMA-TO-CODE-PLAYBOOK.md`. Mỗi giai đoạn là một folder.

## Cách dùng
Theo thứ tự **A→J**, với mỗi giai đoạn **mở 1 session Claude Code mới** và:
1. Đọc `<phase>/guide.md` — hiểu mục tiêu, việc bạn cần làm, điểm review.
2. Mở `<phase>/prompt.md`, điền `<…>`, dán vào session.
3. Claude thực thi `<phase>/instructions.md`.
4. Review theo "Điểm REVIEW" trong `guide.md` → commit → sang folder kế tiếp.

## 3 file trong mỗi folder
- **`guide.md`** — cho NGƯỜI: mục tiêu, điều kiện vào, việc bạn làm vs giao máy, điểm review, cạm bẫy, commit.
- **`prompt.md`** — block dán vào session (một số giai đoạn có **nhiều prompt**: review-gated / vòng lặp / fan-out).
- **`instructions.md`** — cho CLAUDE: các bước mệnh lệnh + self-check + "KHÔNG làm".

## Mô hình prompt theo giai đoạn
| Loại | Giai đoạn | Cách chạy |
|---|---|---|
| One-shot | A, G, J | 1 prompt (có thể dừng-HỎI giữa chừng) |
| Review-gated | B, C | 1 prompt build + (tuỳ) 1 prompt sửa sau khi bạn review |
| Vòng lặp | E, H, I | 1 prompt dựng + N prompt sửa theo diff |
| Fan-out | F | 1 prompt/batch (×~3) + 1 prompt dedup |

## Thứ tự & phụ thuộc
```
A Setup → B Tokens → C Inventory → D Shared components → E Golden page (KHOÁ KHUÔN)
       → F Fan-out → G SEO → H Responsive → I QA → J Build & deploy
```

## Folder
- `A-setup/` · `B-tokens/` · `C-inventory/` · `D-shared-components/` · `E-golden-page/`
- `F-fanout/` · `G-seo/` · `H-responsive/` · `I-qa/` · `J-deploy/`

> Tài liệu nền: `../FIGMA-TO-CODE-PLAYBOOK.md` (chiến lược; mục 10 = danh sách cạm bẫy được các guide trỏ tới).
> File xương sống trong dự án: `CLAUDE.md` + `PROJECT.md`. Token ở `globals.css @theme` (Tailwind v4).
> Khi gặp trục trặc lúc thực thi: xem **`TROUBLESHOOTING.md`** (ở root dự án, cạnh `CLAUDE.md` — failure modes + fallback) và luật leo thang trong `CLAUDE.md`.
