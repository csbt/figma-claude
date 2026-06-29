# Prompt — Giai đoạn A (dán vào session mới)

> Điền các <…> rồi dán nguyên khối dưới đây vào một session Claude Code mới.

```
Đọc guides/A-setup/instructions.md và (nếu đã có) CLAUDE.md + PROJECT.md, rồi thực thi tuần tự.

Bối cảnh dự án:
- Tên: <PROJECT_NAME>
- Stack: Next.js LTS (App Router, RSC), pnpm, TypeScript strict, Tailwind v4, lucide-react
- Figma: <FIGMA_FILE_URL> — chỉ có desktop, rộng <DESIGN_WIDTH>px (desktop là SPEC)
- Tolerance nghiệm thu desktop: <TOLERANCE_PX>px

Yêu cầu:
- Làm đúng từng bước trong instructions.md.
- DỪNG LẠI HỎI tôi ở các điểm cần quyết: version pin cụ thể, nội dung placeholder,
  và trước khi chạy lệnh cài Playwright MCP.
- Xong thì chạy self-check, cập nhật PROJECT.md (tick A), và đề xuất commit.
```
