# BÁO CÁO TIẾN ĐỘ – Ngũ Quốc Linh Thú (goPet mod)

Cập nhật: 26/11/2026. Claude khác đọc file này và CONTEXT.md trước khi làm tiếp. Giao tiếp với người dùng bằng tiếng Việt.

## 0. Nguyên tắc bắt buộc (người dùng yêu cầu)
- Khi usage còn khoảng 90%: PHẢI trả file BAOCAO_TIENDO.md đầy đủ, cụ thể (đã làm gì, đang làm gì, việc tiếp theo, file nào tạo/sửa, quyết định đã chốt, lỗi/giới hạn) để Claude khác làm tiếp không gián đoạn.
- Giữ nguyên tên file và kích thước ảnh gốc. Không sửa phần không được nhắc. Giao jar test từng hệ thống một.
- Người dùng làm trên điện thoại, không chạy lệnh trên máy tính. Repo GitHub chỉ cho Claude quyền đọc.

## 1. Bối cảnh
- Repo (công khai, chỉ đọc): https://github.com/vuduocsyntt-boop/ngu-quoc-linh-thu (CONTEXT.md, yeu-cau/…, jar).
- Jar gốc: goPetdevdao_v1_8_gametuoitho_v4.jar. Jar làm việc mới nhất người dùng gửi: goPetdevdao_v1_8_gametuoitho_v4_maps_v7_2.jar (đã gồm maps v7).
- Thứ tự: 1) Maps (đã làm tới v7, chờ người dùng phản hồi) 2) Ngũ đại cường quốc (yeu-cau/ngu-dai-cuong-quoc.md, chưa làm) 3) 10 pet Akatsuki (yeu-cau/pet-moi.md, ĐANG LÀM: đã thay hình cho 2 pet). Đã bỏ tiến hóa pet.
- Năm nước: Hỏa Quốc/Mộc Diệp/Hokage, Lôi Quốc/Vân Ẩn/Raikage, Thủy Quốc/Vũ Ẩn/Mizukage, Phong Quốc/Sa Ẩn/Kazekage, Thổ Quốc/Thạch Ẩn/Tsuchikage.
- Môi trường Claude: không có mạng, có Java 21 (chạy được `java -cp JAR File.java`, không có javac/javap riêng), Python + PIL/numpy/scipy/cv2/cryptography. Thư mục làm việc MẤT giữa các phiên.

## 2. Kỹ thuật jar (đã xác minh)
### 2.1 Ảnh (.dat, định dạng GPDT, không mã hóa)
"GPDT" + uint16 số mục + mỗi mục [uint16 độ dài tên][tên UTF-8][uint32 offset][uint32 size] (big-endian), dữ liệu ngay sau chỉ mục. scripts/gpdt.py đọc/ghi, roundtrip byte-identical. Các file: assets/pets.dat (petFrame3/*), chars.dat (icons/*.icon.png, anim_characters, anim_wings), ui.dat (pet/battle/skills/*, skills/skill_N.png, npcs…), items.dat, maps.dat.
### 2.2 data/*.json – ĐÃ GIẢI MÃ (trước đây ghi là chưa)
- Định dạng "GPENC2". Giải mã bằng chính lớp của jar: `offline.ae` (public static): `b(byte[])` = GIẢI MÃ, `a(byte[])` = MÃ HÓA, `c(byte[])` = giải mã ra String. (`offline.ci` chỉ xử lý định dạng GPDAT khác, khóa AES "GoPetOffline2026", IV "GoPetInitVector!", KHÔNG dùng cho data/*.json.)
- Mã hóa lại bản giải mã cho ra file BYTE-IDENTICAL với bản gốc (đã kiểm tra với gopet_pet.json) nên sửa json an toàn.
- Cách dùng: scripts/E.java (giải mã mọi file trong thư mục enc/ ra dec/), scripts/RT.java (`enc in out` để mã hóa; chạy `java -cp JAR RT.java enc plain.json out.enc`). Chạy với classpath là jar.
- JSON giải mã có CRLF, thụt lề 2 dấu cách. Khi sửa, thay chuỗi theo byte để giữ định dạng.
### 2.3 Dữ liệu pet (data/gopet_pet.json, 149 pet)
Trường: petId, name, icon ("icons/N.icon.png" trong chars.dat), frameImg ("petFrame3/N.png" trong pets.dat), frameNum, vY, hp, mp, str, _int, agi, type, nclass, element, gymUpLevel, FusionScore, isOnSky, realm.
- 143 pet có frameNum 2: ảnh = 2 khung ngang bằng nhau (khung 1 = tư thế bước, khung 2 = đứng). Pet 10000, 10001, 10003, 10005, 10007, 10009 có 8-20 khung nhưng là hoạt ảnh hiệu ứng, KHÔNG phải trạng thái đứng/đi/đánh. Game không có khung chiến đấu riêng cho pet.
- Ảnh pet là RGBA nhị phân (alpha chỉ 0/255).
- Họ bạch hổ: id 12 (bạch hổ, 84x37, icon 42x37), 2012 (ảo ảnh, 124x52, icon 61x52), 3012 (tiến hoá, 124x52, icon 61x52), 3034 (thiên hổ). Các pet đều có 3 dạng: gốc (id N), ảo ảnh (2000+N), tiến hoá (3000+N).
- Các bảng khác trong data: pet_class, pet_element, pet_tier, pet_eff, skill, skilllv, gopet_map_moblvl… (chưa đọc kỹ; cần cho việc đổi Hệ/Lớp/Cõi/chỉ số).
### 2.4 Hoạt ảnh skill (ui.dat: pet/battle/skills/)
- Mỗi skill hiệu ứng = `<tên>.png` (atlas) + `<tên>` (không đuôi, dữ liệu hoạt ảnh .anu). Skill id 113 "thiên cân thủ" (skill.json, nclass 0) ↔ `thiencanthu`.
- Định dạng .anu (suy ra từ dữ liệu, chưa đọc bytecode): [byte 0][byte N mảnh] rồi N hình chữ nhật atlas, mỗi hình là 4 int (x,y,w,h); sau đó byte số khung, mỗi khung chứa chỉ số mảnh + (x,y) offset (int có dấu) tương đối tâm đích; cuối là danh sách thứ tự phát [byte số mục][(byte chỉ số khung, int -1)…]. thiencanthu: 3 mảnh (0,0,33,57), (33,0,36,41), (33,41,32,25), 5 khung.
- Cách sửa an toàn đã dùng: GIỮ NGUYÊN .anu, chỉ vẽ lại pixel atlas trong đúng các hình chữ nhật cũ.
- Hiệu ứng gắn theo skill id, nên mọi pet dùng skill đó đều đổi hình.
### 2.5 Maps (xem mục 8-10 cũ, giữ nguyên bên dưới)

## 3. PET AKATSUKI – tiến độ (jar mới nhất: goPetdevdao_v1_8_gametuoitho_v4_itachi_v4.jar, dựng trên maps_v7_2, gồm cả maps v7)
Itachi nằm ở pet 12 và 2012, dùng làm mẫu cho các pet Akatsuki còn lại.
| Thay đổi | Chi tiết |
|---|---|
| Pet id 12 (bạch hổ → Itachi) | ảnh petFrame3/12.png ĐỔI cỡ thành 80x56 (2 khung 40x56), icon icons/12.icon.png vẫn 42x37 (cắt đầu + thân trên). Tên đổi "Itachi". Nguồn ảnh: IMG_3202.png (lưới 4x4, mỗi ô 270x180, nền xanh (60,160,60), pixel art phóng ĐÚNG 3x, lấy lại pixel gốc bằng a[1::3,1::3]). Khung 1 = dòng 2 cột 2 (bước), khung 2 = dòng 1 cột 1 (đứng). |
| Skill 113 thiên cân thủ | ui.dat pet/battle/skills/thiencanthu.png (69x66) vẽ lại bằng quả cầu lửa cắt từ dòng 3 của IMG_3202 (vùng x66-120,y396-459): rect0 nổ lớn, rect1/rect2 quả bay nhỏ (xoay 90° để đuôi lửa hướng trái). .anu giữ nguyên. |
| Pet id 2012 (bạch hổ ảo ảnh → Itachi ảo ảnh) | petFrame3/2012.png 124x52 (2 khung 62x52), icons/2012.icon.png 61x52. Tên đổi "Itachi ảo ảnh". Nguồn: IMG_3226.png (1200x896, nền ô caro in sẵn, ảnh nhiễu không phải pixel art chuẩn). Dùng khung 1 và 3 của dòng 1. Tách nền: bg.py (khoảng cách tới 2 màu caro (192,193,175)/(142,144,131), độ bão hòa thấp). |
| Pet id 3012 (bạch hổ tiến hoá → Itachi tiến hoá) | petFrame3/3012.png 124x52, icons/3012.icon.png 61x52, tên "Itachi tiến hoá". Nguồn: IMG_3227.png dòng 1 (Itachi mặc giáp Susanoo đỏ), khung 1 và 3. Ảnh này có màu ô caro KHÁC IMG_3226: sáng (182,182,164), tối (117,117,105); chạy bg.py/itachi3.py với biến môi trường SRC=<đường dẫn ảnh> TONES=3227. |
CHƯA đổi (vẫn là bạch hổ): chỉ số, hệ, lớp, cõi của pet 12, 2012 và 3012.
Quyết định đã chốt: thay pet có sẵn (không thêm id mới); id 12 = Itachi thường, id 2012 = Itachi ảo ảnh theo yêu cầu người dùng.

## 4. Việc tiếp theo (Akatsuki / hệ thống khác)
1. Chờ người dùng xem jar itachi_v4 (hình, hiệu ứng lửa).
2. Pet 3012 ĐÃ làm (xem mục 3). Các dòng 2-4 của IMG_3226/3227 (Susanoo, kiếm, khiên, cung…) chưa dùng.
3. Đổi chỉ số/hệ/lớp cho Itachi theo yeu-cau/pet-moi.md (Hỏa, Pháp Sư, HP 950…): phải quy đổi theo thang game (pet gốc hp 150, str 35 ở cấp thấp), tra pet_element.json và pet_class.json để lấy số đúng. Hệ Phong/Lôi/… có trong game hay không chưa kiểm.
4. Với 9 pet Akatsuki còn lại: nên chọn pet có sẵn để thay (như Itachi) vì thêm id mới cần sửa nhiều bảng. Cần người dùng gửi sprite và chọn pet bị thay. Chú ý game chỉ dùng 2 khung/pet.
5. Hệ thống ngũ đại cường quốc (yeu-cau/ngu-dai-cuong-quoc.md): chưa xác định vị trí trong jar.
6. Maps (xem mục 5-6): chờ phản hồi từ người dùng, xử lý dải ngang/sàn gỗ nếu muốn.

## 5. Maps – jar maps_v5, v6, v7 (nội dung giữ từ báo cáo cũ)
### v5 (assets/maps.dat là file duy nhất khác jar gốc)
- ~96 ảnh chi tiết trong maps.dat thay bằng tileset làng lá người dùng cung cấp (cây, bụi, đá, rào, đèn, biển, tượng, nhà, thùng, bậc thang); ảnh giữ nguyên tên và kích thước.
- maps.dat: 260 mục newMapData/*; 251 png + 9 file không đuôi (107_b, 108_b, 190_b, 194_b, 217_b, 218_b, 2_b, 3_b, 72_b = dữ liệu hoạt ảnh, KHÔNG đụng).
- Ảnh "_a" (190_a, 194_a, 218_a) là atlas: giữ nguyên bố cục, chỉ đổi màu.
- Nhà: 177→nhà cam, 189→nhà đỏ, 192→xanh lá, 193→xanh dương, 216→mái nâu, 301→đỏ lớn, 302→cam, 303→chợ, 304→mái. 191 (147x44) → vách đá Hokage. Nền ô xám 3/151/162 → đá lát be; ô cỏ 161.png → đá lát xám; 246.png ô 0 đá lát, ô 1 đá xoáy lá. Tượng 259/300 và 232, 258, 348-353, 384, 385, 399, 400 → tượng đá hộ vệ. Cổng id 18 → torii nhỏ.
### v6 – giải mã maps/*.dat
Định dạng maps/NN.dat: byte nA, byte nB; (nA+nB) mục [short id ảnh newMapData][byte cờ]; nA mục đầu là ảnh nền (dải ô 24px), còn lại đối tượng. Cờ 1 = hoạt ảnh (thêm 5 byte/đối tượng), 0/2 = ảnh tĩnh. byte W, H (ô 24px), byte số lớp; lớp[H][W] (nibble cao = chỉ số ảnh nền, nibble thấp-1 = thứ tự ô trong dải); mảng q[H][W]. int N đối tượng: [byte kiểu][short x][short y][byte j] (+5 nếu cờ 1); ảnh tĩnh vẽ đáy-giữa tại (x,y); danh sách PHẢI sắp tăng dần theo y; thêm kiểu mới = nối cuối danh sách kiểu. int K bản ghi: [byte loại][byte b][short x][short y][5 byte hộp] và nếu loại≠0: [byte đích][byte][UTF tên][byte]. Loại 1 = lối chuyển khu. byte C, rồi C mục [byte, short x, short y] (điểm xuất hiện). mapfmt.py đọc đúng 26 map, mapwrite.py ghi lại byte-identical.
Đã làm: thêm ảnh newMapData/2001-2010 (4 kiểu nhà, nền plaza 2008-2009, torii 2005 lớn 56x43 và 2007 nhỏ 40x31, núi Hokage 2010 190x57); map 11 (làng): 4 nhà id 189 thành 4 kiểu, sân giữa lát tấm nền bia xoáy lá, xóa bảng "2024", hai tượng dời sang hai bên bia, một núi Hokage; MỌI lối ra (49 lối, 25 map) có cổng torii.
Tên map ↔ file: 13=Linh Lâm, 15=Đại Linh Cảnh, 16=Đường lên núi, 19=Đấu trường, 22=Chợ trời, 24=Sông Băng, 11=làng.
### v7 – map Linh Lâm (13.dat) thành sa mạc
Tileset sa mạc/Phong quốc thứ ba (7F4923A7-…png). Thêm id ảnh mới 3001-3002 (nền), 3101-3125 (chi tiết); map 13 đổi sang id mới, xoay vòng nhiều chi tiết. Cây, đá, bụi, biển, hàng rào, cổng → kiểu sa mạc (cổng đá có cờ 風).
### Rủi ro/chưa làm (maps)
- Ô 161 idx10 (cỏ đặc) bị thay bằng đá lát xám ở mọi map dùng ảnh 161 (không chỉ map 11); người dùng chưa phản hồi. Nếu lạ, tạo ảnh nền riêng cho map 11.
- Chưa kiểm chứng trong game: tọa độ cổng gần mép map, cổng ở hang/nội thất lạc phong cách, map 35 danh sách đối tượng không sắp theo y, cổng lối Linh Mộc (map 13) bị cây che.
- Chưa xử lý: dải ngang (179, 180, 219-222, 241-245, 295, 299, 334, 337, 371…), sàn gỗ 274-288, ô cát/kính 246/371. Người dùng còn tileset thứ hai (1536x1024 có torii lớn bậc thang, nhà hai tầng, chợ, bia lá…) chưa dùng hết.
- Cách tái tạo maps: cần jar gốc + tileset JPEG 1536x1024 (B3698933-…jpeg), tileset sa mạc (7F4923A7-…png), ảnh nhãn IMG_3209/3210. Script cũ: gpdt.py, seg.py, labels.py, build.py, apply2.py → apply3.py → apply4.py (v6) → apply5.py (v7), mapfmt.py, mapwrite.py, render.py (các script maps KHÔNG còn trong môi trường hiện tại; cần dựng lại nếu làm tiếp maps).

## 6. File đã tạo trong lượt này (thư mục outputs)
- goPetdevdao_v1_8_gametuoitho_v4_itachi_v4.jar (jar test mới nhất; đã xóa v1-v3)
- BAOCAO_TIENDO.md (file này)
- scripts/: gpdt.py (đọc/ghi GPDT), itachi.py (bản 42x37, cũ), itachi2.py (Itachi 80x56 + icon + atlas lửa), itachi3.py (Itachi ảo ảnh 124x52), bg.py (tách nền caro), E.java (giải mã data), RT.java (mã hóa/kiểm tra), K.java (đọc khóa ci). Đường dẫn nguồn trong script: /mnt/user-data/uploads/IMG_3202.png, IMG_3226.png (cần người dùng gửi lại).
- Trình tự đóng gói: đọc jar, thay đúng mục trong .dat bằng gpdt.write (kiểm tra roundtrip trước), thay data/gopet_pet.json bằng bản mã hóa lại, ghi zip mới giữ nguyên ZipInfo từng mục, kiểm tra chỉ các mục dự kiến khác jar gốc.

## 7. Lỗi / giới hạn đã gặp
- Chưa kiểm chứng trong game thật (người dùng test trên điện thoại).
- Pet chỉ có 2 khung nên không dùng được dòng chiến đấu của sprite; hoạt ảnh "phun lửa" làm bằng hiệu ứng skill.
- Hiệu ứng thiencanthu thay cho mọi pet dùng skill 113; muốn riêng Itachi phải sửa code game (chưa làm).
- IMG_3226.png chất lượng thấp (nền caro in sẵn, JPEG nhiễu); mép khói tím bị mất một phần.
- Giới hạn bản quyền đã nói với người dùng: không tự vẽ lại nhân vật/logo có sẵn; dùng ảnh người dùng cung cấp.
