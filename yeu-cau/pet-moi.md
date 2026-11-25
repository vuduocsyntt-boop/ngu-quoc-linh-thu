# Yêu cầu: thêm pet mới vào game (chủ đề Akatsuki)

## Quy tắc chung
- Số lượng pet cần thêm: 11 (Lôi Long Baby + 10 pet Akatsuki)
- Pet Akatsuki dạng linh thú chibi: thân nhỏ, mặc áo choàng mây đỏ, giữ nét đặc trưng của từng nhân vật
- Mỗi pet cần: ảnh thân, icon, ảnh hiệu ứng kỹ năng, và dữ liệu khai báo
- Tên file ảnh đặt theo kiểu: ten-pet-than.png, ten-pet-icon.png, ten-pet-skill1.png (không dấu, không khoảng trắng)
- Chỉ số bên dưới là tương đối, Claude quy đổi theo thang chỉ số thật của game
- Lớp và Cõi bên dưới là đề xuất: Claude đối chiếu với danh sách Lớp/Cõi có sẵn trong jar và đổi sang tên đúng của game; nếu game thiếu loại nào thì báo lại
- Chỉ dùng cho bản chơi riêng

## Cách chia Lớp và Cõi (đề xuất)
- Lớp: Chiến Binh, Pháp Sư, Hộ Vệ, Hỗ Trợ, Sát Thủ
- Cõi theo hệ: Kim = Thiên Kim Đế Quốc, Mộc = Thanh Mộc Vương Quốc, Thủy = Hải Lam Thủy Quốc, Hỏa = Xích Viêm Đế Quốc, Thổ = Hoàng Thổ Sơn Quốc; Lôi Long Baby ở cõi riêng Thiên Lôi

## Định dạng ảnh
- Ảnh thân: một dải ngang gồm các khung hình bằng nhau, [số] khung, mỗi khung [rộng x cao] px (Claude lấy theo pet gốc)
- Icon: một ảnh nhỏ, [rộng x cao] px (theo icon pet gốc)
- Hiệu ứng kỹ năng: dải khung hình giống ảnh thân, [số] khung

## Pet 1: Lôi Long Baby
- Tên hiển thị: Lôi Long Baby
- Hệ: Lôi (nếu game không có hệ Lôi thì dùng hệ gần nhất)
- Lớp: Sát Thủ
- Cõi: Thiên Lôi
- Chỉ số: HP 1000 | Tấn công 130 | Phòng thủ 70 | Tốc độ 120
- Kỹ năng:
  1. Lôi Trảo – cào bằng móng điện, sát thương vật lý, có xác suất gây choáng ngắn
  2. Sấm Rền – tia sét đánh một mục tiêu, sát thương phép cao
- Tên file ảnh: loi-long-baby-than.png, loi-long-baby-icon.png, loi-long-baby-skill1.png
- Mô tả ngoại hình: rồng con thân xanh dương đậm, sừng và đuôi phát tia điện vàng

## Pet 2: Pain (Hệ Thổ)
- Tên hiển thị: Pain
- Hệ: Thổ
- Lớp: Hộ Vệ
- Cõi: Hoàng Thổ Sơn Quốc
- Chỉ số: HP 1200 | Tấn công 120 | Phòng thủ 110 | Tốc độ 85
- Kỹ năng:
  1. Đẩy Lùi – đẩy cả nhóm đối thủ ra xa, giảm tốc độ
  2. Triệu Hồi – gọi một pet hỗ trợ trong vài lượt
- Tên file ảnh: pain-than.png, pain-icon.png, pain-skill1.png
- Mô tả ngoại hình: chibi tóc cam nhọn, mắt vòng tròn tím, nhiều khuyên đen trên mặt, áo choàng mây đỏ

## Pet 3: Konan (Hệ Mộc)
- Tên hiển thị: Konan
- Hệ: Mộc
- Lớp: Hỗ Trợ
- Cõi: Thanh Mộc Vương Quốc
- Chỉ số: HP 900 | Tấn công 115 | Phòng thủ 75 | Tốc độ 125
- Kỹ năng:
  1. Mưa Giấy – mưa giấy sắc gây sát thương nhiều mục tiêu
  2. Cánh Giấy – tăng tốc độ bản thân trong vài lượt
- Tên file ảnh: konan-than.png, konan-icon.png, konan-skill1.png
- Mô tả ngoại hình: chibi tóc xanh, hoa giấy trên đầu, cánh giấy trắng, áo choàng mây đỏ

## Pet 4: Itachi (Hệ Hỏa)
- Tên hiển thị: Itachi
- Hệ: Hỏa
- Lớp: Pháp Sư
- Cõi: Xích Viêm Đế Quốc
- Chỉ số: HP 950 | Tấn công 145 | Phòng thủ 75 | Tốc độ 130
- Kỹ năng:
  1. Hỏa Độn – quả cầu lửa lớn, sát thương phép, có xác suất gây bỏng
  2. Ảo Thuật – làm một đối thủ mất lượt trong thời gian ngắn
- Tên file ảnh: itachi-than.png, itachi-icon.png, itachi-skill1.png
- Mô tả ngoại hình: chibi tóc đen buộc đuôi thấp, mắt đỏ ba chấm, quạ đen đậu vai, áo choàng mây đỏ

## Pet 5: Kisame (Hệ Thủy)
- Tên hiển thị: Kisame
- Hệ: Thủy
- Lớp: Chiến Binh
- Cõi: Hải Lam Thủy Quốc
- Chỉ số: HP 1300 | Tấn công 125 | Phòng thủ 100 | Tốc độ 80
- Kỹ năng:
  1. Hút Chakra – gây sát thương và hồi năng lượng cho bản thân
  2. Sóng Lớn – sóng tràn gây sát thương cả nhóm đối thủ
- Tên file ảnh: kisame-than.png, kisame-icon.png, kisame-skill1.png
- Mô tả ngoại hình: chibi da xanh xám, mang cá hai bên má, răng nhọn, kiếm lớn quấn băng, áo choàng mây đỏ

## Pet 6: Deidara (Hệ Thổ)
- Tên hiển thị: Deidara
- Hệ: Thổ
- Lớp: Pháp Sư
- Cõi: Hoàng Thổ Sơn Quốc
- Chỉ số: HP 850 | Tấn công 155 | Phòng thủ 60 | Tốc độ 110
- Kỹ năng:
  1. Bom Đất Sét – nổ gây sát thương rất lớn, bản thân mất một phần HP
  2. Chim Đất Sét – chim đất sét bay tấn công nhiều mục tiêu
- Tên file ảnh: deidara-than.png, deidara-icon.png, deidara-skill1.png
- Mô tả ngoại hình: chibi tóc vàng buộc cao, mái che một mắt, tay có miệng nhỏ, áo choàng mây đỏ

## Pet 7: Sasori (Hệ Kim)
- Tên hiển thị: Sasori
- Hệ: Kim
- Lớp: Sát Thủ
- Cõi: Thiên Kim Đế Quốc
- Chỉ số: HP 1000 | Tấn công 130 | Phòng thủ 95 | Tốc độ 100
- Kỹ năng:
  1. Kiếm Độc – gây sát thương và độc theo lượt
  2. Rối Gỗ – dùng rối đỡ một đòn thay cho bản thân
- Tên file ảnh: sasori-than.png, sasori-icon.png, sasori-skill1.png
- Mô tả ngoại hình: chibi tóc đỏ ngắn, thân gỗ, khớp nối lộ ra, đuôi rối có lưỡi sắc, áo choàng mây đỏ

## Pet 8: Hidan (Hệ Kim)
- Tên hiển thị: Hidan
- Hệ: Kim
- Lớp: Chiến Binh
- Cõi: Thiên Kim Đế Quốc
- Chỉ số: HP 1150 | Tấn công 135 | Phòng thủ 85 | Tốc độ 90
- Kỹ năng:
  1. Lưỡi Hái – chém bằng lưỡi hái ba lưỡi, sát thương vật lý
  2. Nguyền Rủa – chia một phần sát thương nhận vào sang đối thủ
- Tên file ảnh: hidan-than.png, hidan-icon.png, hidan-skill1.png
- Mô tả ngoại hình: chibi tóc bạc vuốt ngược, vòng cổ ký hiệu tròn, lưỡi hái ba lưỡi đỏ, áo choàng mây đỏ

## Pet 9: Kakuzu (Hệ Mộc)
- Tên hiển thị: Kakuzu
- Hệ: Mộc
- Lớp: Hộ Vệ
- Cõi: Thanh Mộc Vương Quốc
- Chỉ số: HP 1250 | Tấn công 120 | Phòng thủ 105 | Tốc độ 75
- Kỹ năng:
  1. Chỉ Đen – sợi chỉ đen trói đối thủ, giảm tốc độ và phòng thủ
  2. Nhiều Mạng – hồi sinh một lần mỗi trận với một phần HP
- Tên file ảnh: kakuzu-than.png, kakuzu-icon.png, kakuzu-skill1.png
- Mô tả ngoại hình: chibi mặt nạ che miệng, mắt xanh lá trên nền đỏ, sợi
