# Troubleshooting — Khi thực thi gặp trục trặc

> Bảng "triệu chứng → xử lý / fallback". Mọi guide trỏ về đây qua **luật leo thang** trong `CLAUDE.md`.
> Nguyên tắc bao trùm: gặp mơ hồ / tool rỗng / sửa 2–3 vòng không đạt → **DỪNG & HỎI**, không đoán bừa.

---

## 1. Đọc Figma không ra như mong đợi
| Triệu chứng | Xử lý / fallback |
|---|---|
| MCP trả rỗng/thiếu | Kiểm Figma desktop đang mở, đúng file active, node URL đúng. Thử `get_metadata` trước. Vẫn rỗng → dùng `get_screenshot` + dựng tay từ ảnh; báo người. |
| Frame **không auto-layout** → output absolute lộn xộn | Bỏ toạ độ, tái dựng từ screenshot bằng flex/grid. Cấu trúc mơ hồ (không rõ hàng/cột) → HỎI người ý đồ. |
| Effect không map CSS (blend/shader/shadow nhiều lớp/gradient phức tạp) | Xấp xỉ gần nhất bằng CSS, ghi "khác biệt chấp nhận" vào `PROJECT.md`. Nếu là điểm nhấn quan trọng → HỎI. |
| **Font không free / không có trên Google Fonts** | HỎI người: mua / thay font thay thế / dùng fallback. **KHÔNG tự chọn font khác** (sai vertical rhythm). |
| Asset không export / icon thiếu trong lucide | `download_assets` dạng SVG. Không được → xin người cấp asset. Icon → lucide gần nhất hoặc xin SVG. |

## 2. Token khi KHÔNG có design system
| Triệu chứng | Xử lý / fallback |
|---|---|
| Quá nhiều màu gần nhau | Gom theo ngưỡng (chênh nhỏ → 1 token). Đề xuất scale + liệt kê cái đã gộp vào `PROJECT.md` để người duyệt. Mơ hồ → HỎI. |
| Cùng phần tử, spacing khác nhau giữa các trang | Lấy giá trị **phổ biến nhất** làm token, ghi ngoại lệ. Lệch lớn → HỎI. |
| Màu alpha / gradient | Tạo token riêng (vd `--color-overlay`, gradient token), không rải giá trị lẻ. |
| Dark mode lộ muộn | DỪNG: đưa ngay vào biến CSS (đừng hardcode), báo người (ảnh hưởng scope + giờ). |

## 3. Fan-out song song (F)
| Triệu chứng | Xử lý / fallback |
|---|---|
| Subagent lệch recipe / bịa component / code hỏng | Loại kết quả, làm lại trang đó **tuần tự** (không worktree) với prompt chặt hơn; đánh dấu "cần review". |
| Merge worktree xung đột ở file dùng chung (`globals.css`/`layout`/`index.ts`) | File dùng chung phải **khoá ở giai đoạn D** trước fan-out. Buộc đổi → làm tuần tự trên nhánh chính, không trong worktree. |
| Trang phức tạp hơn golden (bảng dữ liệu / nhiều state / tương tác) | Tách khỏi batch, làm riêng tuần tự; có thể cần recipe bổ sung. Đừng nhồi vào fan-out. |
| Subagent cạn context giữa trang lớn | Chia trang theo section thành nhiều bước; giảm số trang/batch. |
| Build vỡ sau merge, khó truy nguyên | Build từng route / `git bisect` revert từng trang. Trang lỗi → cô lập sửa riêng. Commit theo batch giúp revert. |

## 4. Cross-session "mất trí nhớ"
| Triệu chứng | Xử lý / fallback |
|---|---|
| Session mới quên đọc file nền → trôi | Luôn mở đầu bằng prompt chuẩn (đọc `CLAUDE.md` + `PROJECT.md` + token + `components/`). |
| `PROJECT.md` không cập nhật → mất dấu | Cuối mỗi session **bắt buộc** cập nhật + commit. Mất dấu → dựng lại trạng thái từ `git log` + `ls`. |
| Quyết định mâu thuẫn (đổi tên token/component đã dùng) | **Cấm đổi nguồn chân lý sau khi khoá.** Buộc đổi → 1 session refactor toàn cục (grep thay thế), không sửa lẻ từng chỗ. |

## 5. Verify bằng Playwright (E/H/I)
| Triệu chứng | Xử lý / fallback |
|---|---|
| MCP hỏng / site chưa chạy / sai port | Kiểm `pnpm dev` + đúng port; cài lại Playwright MCP. Fallback: người tự chụp & so sánh. |
| "Ước lượng bằng mắt" sai mà tưởng đúng | Đặt 2 ảnh cùng kích thước cạnh nhau, **đo px** các phần chính thay vì cảm tính. Phần quan trọng → người xác nhận. |
| Diff **không hội tụ** sau N vòng | DỪNG (luật leo thang): liệt kê điểm còn lệch + nguyên nhân nghi ngờ, HỎI người chấp nhận hay đổi cách. |
| Animation / nội dung động làm screenshot chập chờn | Tắt animation khi chụp (prefers-reduced-motion) / chờ trạng thái ổn định. |

## 6. Build / SSR & ràng buộc 1 ngày
| Triệu chứng | Xử lý / fallback |
|---|---|
| Hydration mismatch | Tìm nguyên nhân (Date/random/window, HTML sai cấu trúc); đơn giản hóa; phần client-only → `'use client'` đúng chỗ. |
| Build vỡ ở phút chót | Lẽ ra **build sau mỗi batch**. Đã vỡ → bisect; checkpoint commit để revert nhanh. |
| **Tụt tiến độ** | Triage theo thứ tự cắt giảm (xem mục triage trong playbook): hoãn animation/state phụ → gộp trang giống nhau → hạ mục tiêu Lighthouse phụ → cắt trang ít ưu tiên. Báo người. |

## 7. Claude tự gây lỗi
| Triệu chứng | Xử lý / fallback |
|---|---|
| Hallucinate component/token/API | Luật leo thang + verify; bắt **chỉ ra file/dòng thật**. |
| Over-engineer / thêm thư viện | Whitelist trong luật cứng; phải HỎI trước khi cài. |
| Lén lệch recipe | Self-check bắt buộc + người review golden/mỗi batch. |
| Báo "xong" khi chưa verify | Definition of Done + ảnh screenshot làm bằng chứng. |
| **Lặp lại cùng lỗi nhiều lần** | Cắt context (`/clear` hoặc session mới); ghi luật vào `CLAUDE.md` (sticky); diễn đạt lại "sai gì → vì sao → đúng là gì + ví dụ"; bắt tự chẩn đoán trước khi sửa. |
