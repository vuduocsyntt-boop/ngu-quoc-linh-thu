# Yêu cầu: thêm pet mới vào game (chủ đề Akatsuki)

## Quy tắc chung
- Số lượng pet cần thêm: 10 (các thành viên Akatsuki)
- Pet Akatsuki dạng linh thú chibi: thân nhỏ, mặc áo choàng mây đỏ, giữ nét đặc trưng của từng nhân vật
- Mỗi pet cần: ảnh thân, icon, ảnh hiệu ứng kỹ năng, và dữ liệu khai báo
- Tên file ảnh đặt theo kiểu: ten-pet-than.png, ten-pet-icon.png, ten-pet-skill1.png (không dấu, không khoảng trắng)
- Chỉ số bên dưới là tương đối, Claude quy đổi theo thang chỉ số thật của game
- Hệ, Lớp và Cõi bên dưới là đề xuất: Claude đối chiếu với danh sách có sẵn trong jar và đổi sang tên đúng của game; nếu game thiếu hệ nào (ví dụ hệ Phong) thì dùng hệ gần nhất và báo lại
- Chỉ dùng cho bản chơi riêng

## Cách chia Hệ, Lớp và Cõi
- Hệ và Cõi theo Ngũ Đại Cường Quốc (khớp với file ngu-dai-cuong-quoc.md): Hỏa = Hỏa Quốc, Phong = Phong Quốc, Thủy = Thủy Quốc, Lôi = Lôi Quốc, Thổ = Thổ Quốc
- Lớp: Chiến Binh, Pháp Sư, Hộ Vệ, Hỗ Trợ, Sát Thủ

## Định dạng ảnh
- Ảnh thân: một dải ngang gồm các khung hình bằng nhau, [số] khung, mỗi khung [rộng x cao] px (Claude lấy theo pet gốc)
- Icon: một ảnh nhỏ, [rộng x cao] px (theo icon pet gốc)
- Hiệu ứng kỹ năng: dải khung hình giống ảnh thân, [số] khung

## Pet 1: Pain
- Tên hiển thị: Pain
- Hệ: Thủy
- Lớp: Hộ Vệ
- Cõi: Thủy Quốc
- Chỉ số: HP 1200 | Tấn công 120 | Phòng thủ 110 | Tốc độ 85
- Kỹ năng:
  1. Đẩy Lùi – đẩy cả nhóm đối thủ ra xa, giảm tốc độ
  2. Triệu Hồi – gọi một pet hỗ trợ trong vài lượt
- Tên file ảnh: pain-than.png, pain-icon.png, pain-skill1.png
- Mô tả ngoại hình: chibi tóc cam nhọn, mắt vòng tròn tím, nhiều khuyên đen trên mặt, áo choàng mây đỏ

## Pet 2: Konan
- Tên hiển thị: Konan
- Hệ: Phong
- Lớp: Hỗ Trợ
- Cõi: Phong Quốc
- Chỉ số: HP 900 | Tấn công 115 | Phòng thủ 75 | Tốc độ 125
- Kỹ năng:
  1. Mưa Giấy – mưa giấy sắc gây sát thương nhiều mục tiêu
  2. Cánh Giấy – tăng tốc độ bản thân trong vài lượt
- Tên file ảnh: konan-than.png, konan-icon.png, konan-skill1.png
- Mô tả ngoại hình: chibi tóc xanh, hoa giấy trên đầu, cánh giấy trắng, áo choàng mây đỏ

## Pet 3: Itachi
- Tên hiển thị: Itachi
- Hệ: Hỏa
- Lớp: Pháp Sư
- Cõi: Hỏa Quốc
- Chỉ số: HP 950 | Tấn công 145 | Phòng thủ 75 | Tốc độ 130
- Kỹ năng:
  1. Hỏa Độn – quả cầu lửa lớn, sát thương phép, có xác suất gây bỏng
  2. Ảo Thuật – làm một đối thủ mất lượt trong thời gian ngắn
- Tên file ảnh: itachi-than.png, itachi-icon.png, itachi-skill1.png
- Mô tả ngoại hình: chibi tóc đen buộc đuôi thấp, mắt đỏ ba chấm, quạ đen đậu vai, áo choàng mây đỏ

## Pet 4: Kisame
- Tên hiển thị: Kisame
- Hệ: Thủy
- Lớp: Chiến Binh
- Cõi: Thủy Quốc
- Chỉ số: HP 1300 | Tấn công 125 | Phòng thủ 100 | Tốc độ 80
- Kỹ năng:
  1. Hút Chakra – gây sát thương và hồi năng lượng cho bản thân
  2. Sóng Lớn – sóng tràn gây sát thương cả nhóm đối thủ
- Tên file ảnh: kisame-than.png, kisame-icon.png, kisame-skill1.png
- Mô tả ngoại hình: chibi da xanh xám, mang cá hai bên má, răng nhọn, kiếm lớn quấn băng, áo choàng mây đỏ

## Pet 5: Deidara
- Tên hiển thị: Deidara
- Hệ: Thổ
- Lớp: Pháp Sư
- Cõi: Thổ Quốc
- Chỉ số: HP 850 | Tấn công 155 | Phòng thủ 60 | Tốc độ 110
- Kỹ năng:
  1. Bom Đất Sét – nổ gây sát thương rất lớn, bản thân mất một phần HP
  2. Chim Đất Sét – chim đất sét bay tấn công nhiều mục tiêu
- Tên file ảnh: deidara-than.png, deidara-icon.png, deidara-skill1.png
- Mô tả ngoại hình: chibi tóc vàng buộc cao, mái che một mắt, tay có miệng nhỏ, áo choàng mây đỏ

## Pet 6: Sasori
- Tên hiển thị: Sasori
- Hệ: Phong
- Lớp: Sát Thủ
- Cõi: Phong Quốc
- Chỉ số: HP 1000 | Tấn công 130 | Phòng thủ 95 | Tốc độ 100
- Kỹ năng:
  1. Kiếm Độc – gây sát thương và độc theo lượt
  2. Rối Gỗ – dùng rối đỡ một đòn thay cho bản thân
- Tên file ảnh: sasori-than.png, sasori-icon.png, sasori-skill1.png
- Mô tả ngoại hình: chibi tóc đỏ ngắn, thân gỗ, khớp nối lộ ra, đuôi rối có lưỡi sắc, áo choàng mây đỏ

## Pet 7: Hidan
- Tên hiển thị: Hidan
- Hệ: Hỏa
- Lớp: Chiến Binh
- Cõi: Hỏa Quốc
- Chỉ số: HP 1150 | Tấn công 135 | Phòng thủ 85 | Tốc độ 90
- Kỹ năng:
  1. Lưỡi Hái – chém bằng lưỡi hái ba lưỡi, sát thương vật lý
  2. Nguyền Rủa – chia một phần sát thương nhận vào sang đối thủ
- Tên file ảnh: hidan-than.png, hidan-icon.png, hidan-skill1.png
- Mô tả ngoại hình: chibi tóc bạc vuốt ngược, vòng cổ ký hiệu tròn, lưỡi hái ba lưỡi đỏ, áo choàng mây đỏ

## Pet 8: Kakuzu
- Tên hiển thị: Kakuzu
- Hệ: Lôi
- Lớp: Hộ Vệ
- Cõi: Lôi Quốc
- Chỉ số: HP 1250 | Tấn công 120 | Phòng thủ 105 | Tốc độ 75
- Kỹ năng:
  1. Chỉ Đen – sợi chỉ đen trói đối thủ, giảm tốc độ và phòng thủ
  2. Nhiều Mạng – hồi sinh một lần mỗi trận với một phần HP
- Tên file ảnh: kakuzu-than.png, kakuzu-icon.png, kakuzu-skill1.png
- Mô tả ngoại hình: chibi mặt nạ che miệng, mắt xanh lá trên nền đỏ, sợi chỉ đen, áo choàng mây đỏ

## Pet 9: Tobi
- Tên hiển thị: Tobi
- Hệ: Hỏa
- Lớp: Hỗ Trợ
- Cõi: Hỏa Quốc
- Chỉ số: HP 1000 | Tấn công 125 | Phòng thủ 80 | Tốc độ 135
- Kỹ năng:
  1. Xuyên Không – né đòn tiếp theo hoàn toàn
  2. Hút Vào – hút đối thủ vào xoáy, mất lượt ngắn
- Tên file ảnh: tobi-than.png, tobi-icon.png, tobi-skill1.png
- Mô tả ngoại hình: chibi mặt nạ cam xoáy một lỗ mắt, tóc đen dựng, áo choàng mây đỏ

## Pet 10: Zetsu
- Tên hiển thị: Zetsu
- Hệ: Thổ
- Lớp: Hỗ Trợ
- Cõi: Thổ Quốc
- Chỉ số: HP 1050 | Tấn công 105 | Phòng thủ 90 | Tốc độ 115
- Kỹ năng:
  1. Hút Chakra – hút năng lượng đối thủ, hồi HP cho bản thân
  2. Ẩn Mình – giảm khả năng bị chọn làm mục tiêu trong vài lượt
- Tên file ảnh: zetsu-than.png, zetsu-icon.png, zetsu-skill1.png
- Mô tả ngoại hình: chibi nửa đen nửa trắng, bẫy cây ăn thịt bao quanh đầu, da xanh lá

## Ghi chú thêm
- Tên pet, hệ, lớp, cõi và kỹ năng chỉ là đề xuất, có thể đổi sau
- Tên kỹ năng có thể đổi nhẹ để tránh trùng tên với kỹ năng có sẵn trong game
- Nếu game đã có sẵn pet trùng tên, Claude sẽ báo để đổi
- Chỉ dùng cho bản chơi riêng
