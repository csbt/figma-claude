# Giai đoạn E — Golden page · GUIDE (cho người)

> **Mục tiêu:** build 1 trang HOÀN CHỈNH (thường là Home) end-to-end làm "khuôn" cho 19 trang sau.
> **Output:** 1 trang khớp desktop + responsive + SEO; "page recipe" trong `CLAUDE.md` được xác nhận/tinh chỉnh.
> **Thời lượng:** ~1.5–2 giờ · **Session:** 1 (vòng lặp build → fix diff).

---

## Điều kiện vào
- A→D xong: token + component dùng chung + layout đã sẵn.
- Playwright MCP đã cài (để self-verify).

## Việc của BẠN — CỔNG CHẤT LƯỢNG QUAN TRỌNG NHẤT
- **Review cực kỹ** trang này: spacing/màu/font/responsive/markup. Đây là khuôn — đẹp ở đây thì 19 trang sau mới đều.
- Xác nhận "page recipe" trong `CLAUDE.md` phản ánh đúng cách trang này được dựng (nếu cần, tinh chỉnh recipe).
- **Đừng vội** sang F khi chưa thật ưng E.

## Việc giao CLAUDE
Dựng trang theo PAGE RECIPE → self-verify bằng Playwright (chụp site so Figma) → tự sửa cho khớp.

## Mô hình prompt (vòng lặp, có cổng Plan mode)
0. **Plan mode** — Claude đề xuất cấu trúc trang + component tái dùng + cách layout, CHỜ bạn duyệt (chưa sửa file).
1. **Prompt dựng** — build trang theo recipe + tự verify.
2. **Prompt sửa theo diff** — lặp lại đến khi khớp tolerance ("sửa các điểm lệch sau: …").

## Component page-specific — tách ở đâu, yêu cầu thế nào

> Section phức tạp nhưng **chỉ 1 trang dùng** → đừng đưa vào `components/`, dùng thư mục co-located.

**Quy tắc 2 tầng:**

| Điều kiện | Lưu ở đâu |
|---|---|
| Chỉ trang này dùng | `app/(pages)/<slug>/_components/TênSection.tsx` |
| ≥ 2 trang dùng | `components/ui/` hoặc `components/features/` |

`_components/` (dấu gạch dưới) là convention Next.js — thư mục này bị exclude khỏi router tự động.

**Ngưỡng nên tách:** JSX > 60 dòng **hoặc** section có local state riêng.

**Yêu cầu Claude tách tại Prompt 0 (Plan mode):**
```
Với mỗi section có JSX > 60 dòng hoặc có local state riêng, đề xuất tách thành
component riêng tại app/(pages)/<slug>/_components/. Chỉ đưa vào components/
nếu section đó tái dùng ≥ 2 trang.
```

**Yêu cầu tách khi đang sửa diff:**
```
Section <TênSection> quá lớn / có state riêng. Tách thành
app/(pages)/<slug>/_components/<TênSection>.tsx.
KHÔNG tạo file mới trong components/ vì chỉ trang này dùng.
```

**Promote lên shared:** Khi trang thứ 2 cần dùng cùng component → di chuyển vào `components/` và cập nhật barrel.

---

## Sửa lỗi theo section (KHÔNG chạy lại cả page)

> Claude gần như không dựng đúng cả trang trong 1 lần — đó là bình thường. Vá theo **section**, đừng dựng lại trang.

- **Vì sao không chạy lại cả page:** vứt phần đã đúng + đốt lại Figma call + dễ làm hỏng section đang đúng (regression).
  Bước self-verify đã cho bạn **danh sách lệch theo từng section** → đó chính là danh sách việc cần sửa.
- **Context còn sống → nói tiếp, không chạy lại.** Cùng session, agent vẫn nhớ JSX vừa viết. Chỉ cần **pin scope**:
  *"Header/Hero ổn. Riêng Features: card 2 cột, Figma là 3 cột, gap hẹp hơn. CHỈ sửa Features, đừng đụng phần còn lại."*
- **Sửa Ở ĐÂU tuỳ tầng lỗi** (nhầm tầng là lỗi đắt nhất):
  - sai **shared component** (Button/Card/Header) → sửa 1 lần ở `components/`, fix mọi nơi — đừng vá trong từng trang.
  - sai **section riêng của trang** → sửa thẳng trong file trang.
  - sai **token** (màu/spacing/font) → sửa token trong `globals.css`, lan toả.
- **Có trần:** quá **2–3 vòng** vẫn chưa khớp → DỪNG & HỎI (Hard Rule "leo thang"). Thường là token sai / thiết kế mơ hồ
  cần quyết định của bạn — đừng để vá vô tận.
- **Componentize tốt = iterate rẻ.** Vá theo section chỉ dễ khi markup đã chia section/component rõ. E trả "học phí"
  sửa lỗi **một lần** để khoá khuôn → F lệch ít hẳn. Nếu sau này trang nào ở F cũng lệch nhiều → tín hiệu khuôn / shared
  component chưa chặt, quay lại sửa tầng dùng chung thay vì vá từng trang.

## Điểm REVIEW trước khi qua F
- [ ] Khớp desktop trong ngưỡng `<TOLERANCE_PX>`px.
- [ ] Reflow đúng ở tablet/mobile.
- [ ] Tái dùng component dùng chung, không đẻ trùng; chỉ dùng token.
- [ ] `generateMetadata` đầy đủ; heading/alt/aria ổn.
- [ ] PAGE RECIPE trong `CLAUDE.md` đã khớp thực tế → KHOÁ KHUÔN.

## Cạm bẫy (mục 10 playbook)
- **#1** fan-out trước khi khoá khuôn · **#16** đòi pixel-perfect tuyệt đối · **#17** markup div-lồng-div.

## Commit gợi ý
```
feat(page): home (golden template)
```

→ **Tiếp theo:** `guides/F-fanout/`
