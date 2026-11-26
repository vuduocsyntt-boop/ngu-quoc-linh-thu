# BÁO CÁO TIẾN ĐỘ – Ngũ Quốc Linh Thú (goPet mod)

Cập nhật: 26/11/2026. Claude khác đọc file này và CONTEXT.md trước khi làm tiếp. Giao tiếp với người dùng bằng tiếng Việt.

## 0. Nguyên tắc bắt buộc (người dùng yêu cầu)
- Khi usage còn khoảng 90%: PHẢI trả file BAOCAO_TIENDO.md đầy đủ, cụ thể (đã làm gì, đang làm gì, việc tiếp theo, file nào tạo/sửa, quyết định đã chốt, lỗi/giới hạn) để Claude khác làm tiếp không gián đoạn.
- Giữ nguyên tên file và kích thước ảnh gốc. Không sửa phần không được nhắc. Giao jar test từng hệ thống một.

## 1. Bối cảnh
- Repo (công khai, chỉ đọc): https://github.com/vuduocsyntt-boop/ngu-quoc-linh-thu (CONTEXT.md, yeu-cau/…, jar). Người dùng tự cập nhật file lên GitHub.
- Jar gốc làm việc: goPetdevdao_v1_8_gametuoitho_v4.jar. Người dùng làm trên điện thoại, test từng jar.
- Thứ tự: 1) Maps (đang làm) 2) Ngũ đại cường quốc (yeu-cau/ngu-dai-cuong-quoc.md) 3) 10 pet Akatsuki (yeu-cau/pet-moi.md). Đã bỏ tiến hóa pet.
- Năm nước: Hỏa Quốc/Mộc Diệp/Hokage, Lôi Quốc/Vân Ẩn/Raikage, Thủy Quốc/Vũ Ẩn/Mizukage, Phong Quốc/Sa Ẩn/Kazekage, Thổ Quốc/Thạch Ẩn/Tsuchikage.
- Giới hạn đã nói với người dùng: không vẽ lại nhân vật/logo có sẵn của truyện (pet Akatsuki, logo làng); ảnh tileset do người dùng cung cấp nên dùng để cắt ghép.

## 2. Kỹ thuật jar (đã xác minh)
- Ảnh trong assets/{ui,pets,items,chars,maps}.dat, định dạng GPDT không mã hóa: "GPDT" + uint16 số mục + mỗi mục [uint16 độ dài tên][tên UTF-8][uint32 offset][uint32 size] (big-endian), dữ liệu ngay sau chỉ mục. gpdt.py đọc/ghi, đã kiểm tra roundtrip byte-identical.
- assets/maps.dat: 260 mục newMapData/*; 251 png + 9 file không đuôi (107_b, 108_b, 190_b, 194_b, 217_b, 218_b, 2_b, 3_b, 72_b = dữ liệu hoạt ảnh, KHÔNG đụng vào).
- Ảnh "_a" (190_a, 194_a, 218_a) là atlas: giữ nguyên bố cục, chỉ đổi màu pixel.
- Bố cục bản đồ: jar/maps/NN.dat (không mã hóa). Map làng trong ảnh chụp của người dùng là maps/11.dat (có chuỗi "Sông Băng", "Đường lên núi", "Chợ trời" ở cuối file). Cấu trúc: byte đầu, danh sách (uint16 id ảnh, byte cờ), lưới ô nền, danh sách đối tượng (tọa độ), tên điểm đến UTF-8. ĐÃ giải mã đầy đủ (mục 7).
- data/*.json mã hóa (GPENC2D, AES, khóa trong offline/ci.class). CHƯA giải mã. Chứa tên Hệ/Lớp/Cõi/pet, cần cho hệ thống pet.
- Môi trường: không có mạng, có Java (JRE, không có javap), Python + PIL/numpy/scipy/cv2.

## 3. Maps – đã làm (jar mới nhất: goPetdevdao_v1_8_gametuoitho_v4_maps_v5.jar)
Chỉ assets/maps.dat khác jar gốc; mọi ảnh giữ nguyên tên và kích thước (đã assert).
- ~96 ảnh chi tiết thay bằng tileset làng lá: cây, bụi, đá, rào, đèn, biển, tượng, nhà, thùng, bậc thang (bảng ánh xạ trong build.py: MAP; vị trí cắt trong build.py).
- Nhà đa dạng: 177→nhà cam(29), 189→nhà đỏ(22), 192→nhà xanh lá(23), 193→nhà xanh dương(21), 216→mái nâu(76), 301→nhà đỏ lớn(27), 302→29, 303→chợ(128), 304→mái(56).
- 191 (147x44, núi nền) → vách đá Hokage 4 mặt.
- Biển hiệu vòm 190_a/194_a/218_a: đổi màu dải sang tông làng lá (đỏ, navy, nâu gỗ, vàng gỗ), giữ chữ và icon (cắt cột 57/56 để tách dải khỏi chữ).
- Nền: ô xám (3.png, 151.png, 162.png) → đá lát màu be (từ ảnh sàn đá be người dùng chỉ định); ô cỏ đặc 161.png (x=240..264, sân cỏ giữa map) → đá lát xám lấy từ tấm nền có biểu tượng lá; 246.png ô 0 = đá lát, ô 1 = đá có xoáy lá.
- Tượng: 259 và 300 (hai tượng trắng xám giữa sân, 57x68) → tượng đá hộ vệ (136). Các id 232, 258, 348-353, 384, 385, 399, 400 cũng → 136.
- Cổng: id 18 (43x27) → torii nhỏ.

## 4. Maps – chưa làm / đang chờ
- (ĐÃ LÀM ở v6, xem mục 7) hướng "cách 2": phân tích maps/*.dat (viết công cụ đọc mã/ phân tích bản ghi) để (a) đặt cổng torii lớn ở đúng tọa độ từng lối chuyển khu, (b) đặt biểu tượng lá giữa sân, (c) ba nhà shop hàng trên (cùng một id) thành ba kiểu khác nhau. Rủi ro: ghi sai byte làm hỏng map → làm trên bản sao, giao jar test riêng.
- Người dùng đã gửi tileset thứ hai (1536x1024, ảnh làng lá có torii lớn bậc thang, nhà hai tầng, chợ, bia lá, tháp nước, cầu, tượng xanh…) làm lựa chọn thêm; chưa dùng.
- Chưa xử lý: dải ngang (179, 180, 219-222, 241-245, 295, 299, 334, 337, 371 …), sàn gỗ 274-288, ô cát/kính 246/371.
- Lối chuyển khu trong game là mũi tên cam nhỏ + tên điểm đến, không phải ảnh cổng riêng; vị trí nằm trong maps/*.dat.
- Rủi ro đã biết: ô 161 có thể dùng ở map khác; ảnh nguồn JPEG nên chi tiết nhỏ hơi mờ; ô nền lặp lại thấy rõ.

## 5. Cách tái tạo (thư mục làm việc mất giữa các phiên)
Cần người dùng gửi lại: jar gốc, tileset JPEG 1536x1024 (B3698933-…jpeg), tùy chọn ảnh nhãn IMG_3209/3210. Script trong outputs: gpdt.py, seg.py, labels.py, build.py, apply2.py (chạy trước, đọc jar gốc, ghi out/maps.dat), apply3.py (chạy sau, sửa out/maps.dat: nền đá, ô 161, 246). apply.py là bản cũ (v1), bỏ qua. Đóng gói: zipfile sao chép toàn bộ jar gốc, chỉ thay assets/maps.dat.

## 6. Việc tiếp theo
1. Chờ người dùng xác nhận cách 2 và chọn chi tiết (cổng, nhà, bia lá) trong hai tileset; sau đó phân tích maps/11.dat, giao jar test.
2. Xử lý dải ngang và sàn gỗ nếu người dùng muốn.
3. Hệ thống ngũ đại cường quốc, sau đó pet Akatsuki (cần giải mã data/*.json).

## 7. CẬP NHẬT v6 – giải mã maps/*.dat và đặt cổng/nhà/biểu tượng (jar: goPetdevdao_v1_8_gametuoitho_v4_maps_v6.jar)
Định dạng maps/NN.dat (suy ra bằng cách đọc bytecode lớp ek.a(DataInputStream,dz), el.a, id; mapfmt.py đọc đúng hết cả 26 map, mapwrite.py ghi lại byte-identical):
- byte nA, byte nB; (nA+nB) mục kiểu: [short id ảnh newMapData][byte cờ]; nA kiểu đầu là ảnh nền (dải ô 24px), còn lại là đối tượng. Cờ 1 = hoạt ảnh (_a/_b, thêm 5 byte/đối tượng), cờ 0/2 = ảnh tĩnh.
- byte W, H (số ô, ô 24px), byte số lớp; lớp[H][W] byte (nibble cao = chỉ số ảnh nền, nibble thấp-1 = số thứ tự ô 24px trong dải); mảng q[H][W].
- int N đối tượng: [byte kiểu][short x][short y][byte j] (+5 byte nếu cờ 1). Ảnh tĩnh vẽ với đáy-giữa tại (x,y). Danh sách PHẢI sắp tăng dần theo y (game vẽ xen kẽ với nhân vật theo y): luôn chèn đúng thứ tự y. Thêm kiểu mới = nối cuối danh sách kiểu (chỉ số cũ không đổi).
- int K bản ghi el: [byte loại][byte b][short x][short y][5 byte hộp] và nếu loại≠0: [byte đích][byte][UTF tên][byte]. Loại 1 = lối chuyển khu (hộp va chạm = x+f1, y+f2, rộng f3, cao f4).
- byte C, rồi C mục [byte, short x, short y] (điểm xuất hiện).
Đã làm: (a) thêm ảnh mới newMapData/2001-2009.png (4 kiểu nhà 2001-2004, dải nền plaza 2008-2009, cổng torii 2005 lớn 56x43 và 2007 nhỏ 40x31) vào maps.dat; (b) map 11 (làng): 4 căn nhà id 189 thành 4 kiểu khác nhau (đỏ, xanh dương, cam, mái nâu, sắp theo x), sân giữa (ô 5..11 x 8..11) được lát bằng nguyên tấm nền đá có bia xoáy lá (168x96, cắt thành 28 ô nền 24px trong 2 dải ảnh nền mới 2008/2009, nA tăng 2→4, các chỉ số kiểu đối tượng đã dịch +2), bảng đỏ "2024" (id 238) đã xóa khỏi map 11, hai tượng 259/300 dời sang x=150 và x=258 để đứng hai bên bia; (c) MỌI map có lối ra (loại 1, 49 lối ở 25 map) đặt một cổng torii tại giữa-đáy hộp lối ra. Scripts: apply2.py → apply3.py → apply4.py (đọc out/maps.dat, ghi out/maps_final.dat + jar v6). render.py dựng ảnh xem trước map (không vẽ hoạt ảnh).
Chưa kiểm chứng trong game: tọa độ gần mép map có thể che mất lối; cổng ở map hang/nội thất có thể lạc phong cách; map 35 danh sách đối tượng vốn không sắp theo y.

Cập nhật tiếp (v6): map 11 chỉ còn MỘT núi Hokage (ảnh mới 2010, 190x57, đáy-giữa tại (330,50)), bỏ 4 bản id 191 và 2 cây thông 158 ở y<70, 250<x<330 để lộ mặt núi. Núi phải có y nhỏ hơn cổng Chợ trời (đáy y=57) vì game vẽ theo thứ tự y. Nhà cam (x=242) và cổng che một phần núi; có thể dời nhà/cổng nếu người dùng muốn thấy rõ cả 4 mặt.

## 8. CẬP NHẬT v7 – map Linh Lâm (maps/13.dat) thành sa mạc (jar: goPetdevdao_v1_8_gametuoitho_v4_maps_v7.jar, gồm toàn bộ v6)
- Tên map ↔ file: tên điểm đến trong bản ghi el là map id: 13=Linh Lâm, 15=Đại Linh Cảnh, 16=Đường lên núi, 19=Đấu trường, 22=Chợ trời, 24=Sông Băng, 11=làng (TP Linh Thú).
- Người dùng gửi tileset sa mạc/Phong quốc thứ ba (7F4923A7-…png, 1536x1024). seg2_comps.py tách chi tiết, chỉ số trong apply5.py (SP) là chỉ số của bảng này.
- Cách làm riêng cho từng map (ảnh dùng chung theo id nên KHÔNG sửa ảnh gốc): thêm id ảnh mới (3001-3002 nền, 3101-3125 chi tiết), đổi kiểu của map 13 sang id mới; mỗi loại đối tượng xoay vòng nhiều chi tiết (CYC) cho đa dạng.
- Nền: dải 161/162 GỐC được tô lại (trắng tuyết→cát sáng, xanh cỏ→cát đậm, viền xanh→viền cát, xám→đá cuội) thành 3001/3002, map 13 dùng chúng ở chỗ 161/162.
- Cây thông 158→cọ/cây khô/xương rồng, anh đào 157/159→cọ lớn/đồi đá/cây lá, đá 164/165/186→đá sa mạc, bụi 170-172→bụi nhỏ, biển 168, bụi 40, hàng rào 185→bụi, cổng torii→cổng đá có cờ 風.
- LƯU Ý: ở các map khác ô 161 idx10 (cỏ đặc) đã bị thay bằng đá lát xám ở v3-v6 (gồm cả map khác ngoài map 11); người dùng chưa phản hồi. Nếu thấy lạ, hạn chế phạm vi chỉ còn map 11 bằng cách tạo ảnh nền mới cho map 11 thay vì sửa ảnh 161.
- Chưa kiểm chứng trong game. Cổng ở lối Linh Mộc (đông) bị cây che.
Trình tự chạy lại: apply2 → apply3 → apply4 (tạo v6) → apply5 (tạo v7).
