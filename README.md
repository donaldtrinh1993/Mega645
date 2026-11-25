# Mega 6/45 — iPhone / iOS / PWA

## Mục đích

Ứng dụng độc lập, không tài khoản, không truy cập dữ liệu Vietlott. Bản PWA được thiết kế để đưa lên GitHub Pages và thêm vào màn hình chính iPhone.

## Gói file

- `index.html`: toàn bộ giao diện và thuật toán.
- `sw.js`: Service Worker để sử dụng offline, sau một lần truy cập HTTPS thành công.
- `manifest.webmanifest`: cấu hình ứng dụng web.
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`: icon iOS/PWA.
- `README.md`: tài liệu này.

## Đưa lên GitHub Pages (tương tự Power655)

1. Tạo repository **Public** mới, ví dụ `mega645-ios`. Không ghi đè repository Power655 đang hoạt động.
2. Giải nén `Mega645_iOS_PWA.zip`; upload **7 file** trong thư mục giải nén vào **thư mục gốc** repo (không upload cả ZIP hoặc thư mục lồng).
3. Vào **Settings > Pages > Build and deployment**: Source **Deploy from a branch**, branch **main**, folder **/(root)**; Save.
4. URL sẽ có dạng `https://TEN-USERNAME.github.io/mega645-ios/`.
5. Trên iPhone, mở URL HTTPS bằng **Safari** > Chia sẻ > **Thêm vào Màn hình chính**; bật *Open as Web App* nếu được hỏi.
6. Mở ứng dụng một lần khi có Wi‑Fi/mạng, sau đó bật Chế độ máy bay để kiểm tra offline.

> Offline cần Service Worker + các file được cache. iOS có thể dọn cache khi thiếu bộ nhớ. Nếu mất offline, hãy mở lại online. Lịch sử bằng `localStorage`, **không tự đồng bộ iCloud**: sử dụng Sao lưu/Khôi phục JSON nếu cần.

## Phiên bản HTML đơn

`Mega645_iOS.html` là file HTML tự chứa. File này không cần internet nếu mở bằng ứng dụng duyệt HTML có JavaScript. **Mở trực tiếp từ Files/Quick Look trên iOS có thể không chạy script.** Khuyên dùng PWA qua HTTPS.

## Thuật toán

Chế độ **Lọc theo 4 điều kiện** giữ nguyên tiêu chí gốc:

1. Tổng sáu số từ **135 đến 141** (bao gồm hai đầu).
2. **3 chẵn + 3 lẻ**.
3. **3 thấp (01–22) + 3 cao (23–45)**.
4. Ít nhất **một cặp liên tiếp**.

Cho phép chọn tối đa 4 số cốt lõi. Hệ thống liệt kê chính xác các bộ số đáp ứng yêu cầu bằng cách ghép bộ ba thấp/cao có cùng điều kiện tổng và chẵn/lẻ, sau đó lấy ngẫu nhiên các bộ **không trùng nhau**. Nếu không đủ số bộ yêu cầu, chỉ tạo tối đa số bộ hợp lệ và thông báo. Nếu không có tổ hợp hợp lệ, không báo nhầm do hết số lần thử.

- Tổng số tổ hợp Mega 6/45: **8.145.060**.
- Số tổ hợp phù hợp cả bốn điều kiện khi không nhập core: **83.863** (~1,03%).
- Chế độ **Ngẫu nhiên 6/45** không áp dụng 4 điều kiện nhưng luôn giữ core nếu có.
- Ưu tiên `crypto.getRandomValues()` bằng cách loại mẫu lệch, fallback `Math.random()` nếu không hỗ trợ.

**Không có lợi thế xác suất Jackpot:** bất kỳ một bộ số riêng lẻ hợp lệ nào đều có xác suất như nhau trong kỳ quay công bằng; lọc chỉ làm hẹp phạm vi chọn số.

## Dùng lâu dài / cập nhật

- Mặc định 10 bộ; có thể chọn 1, 5, 10, 20, 50 bộ.
- Nút Sao chép, Chia sẻ, Xuất TXT; Lưu lịch sử tối đa 30 lần; Xuất/nhập JSON.
- Dữ liệu nằm trên thiết bị, không gửi lên server.
- Khi nâng cấp, cập nhật `index.html` và chỉnh phiên bản cache trong `sw.js` (ví dụ `mega645-ios-v1.0.1`) rồi upload lại; tránh kẹt bản cache cũ.
