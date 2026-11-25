# CONTEXT – ngu-quoc-linh-thu

## 1. Dự án là gì
- Tên dự án: Ngũ Quốc Linh Thú
- Game: GoPet, sửa (mod) để chơi riêng
- Jar đang làm việc chính: goPetdevdao_v1_8_gametuoitho_v4.jar (có trong repo)
- Ngôn ngữ: Java
- Ngôn ngữ giao tiếp: tiếng Việt

## 2. Việc cần làm (làm từng hệ thống một, mỗi hệ thống giao một jar test)
1. Maps: thay ảnh chi tiết bối cảnh bằng tileset làng lá người dùng cung cấp
2. Thay hình ảnh "ngũ đại cường quốc" (xem yeu-cau/ngu-dai-cuong-quoc.md)
3. Thêm pet mới vào game (xem yeu-cau/pet-moi.md)
- ĐÃ BỎ: hệ thống tiến hóa pet (không làm)

## 3. Quy ước
- Không sửa các phần không được nhắc trong yêu cầu
- Giữ nguyên tên file và kích thước ảnh gốc khi thay ảnh
- Trả file sửa đã đóng gói để tải về điện thoại

## 4. NGUYÊN TẮC BẮT BUỘC: báo cáo tiến trình
- Khi usage của Claude còn khoảng 90%, PHẢI trả file báo cáo tiến trình đầy đủ (file BAOCAO_TIENDO.md)
- Nội dung: đã làm gì, đang làm gì, việc tiếp theo, file nào đã tạo/sửa, quyết định đã chốt, lỗi/giới hạn đã gặp
- Mục đích: để Claude khác đọc và làm tiếp mà không bị gián đoạn
- Claude khác khi bắt đầu phải đọc BAOCAO_TIENDO.md trước

## 5. Năm cường quốc (đã chốt)
Hỏa Quốc (Mộc Diệp, Hokage), Lôi Quốc (Vân Ẩn, Raikage), Thủy Quốc (Vũ Ẩn, Mizukage), Phong Quốc (Sa Ẩn, Kazekage), Thổ Quốc (Thạch Ẩn, Tsuchikage)

## 6. Tiến độ
Xem BAOCAO_TIENDO.md (bản mới nhất gửi trong chat).

## 7. Ghi chú
- Làm trên điện thoại, không chạy lệnh trên máy tính
- Dữ liệu data/*.json trong jar bị mã hóa (GPENC2D), ảnh trong assets/*.dat không mã hóa (định dạng GPDT)
- Repo chỉ cho Claude quyền đọc: người dùng tự cập nhật file lên GitHub