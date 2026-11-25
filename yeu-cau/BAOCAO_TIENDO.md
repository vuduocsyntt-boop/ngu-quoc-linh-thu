# BÁO CÁO TIẾN ĐỘ – Ngũ Quốc Linh Thú (goPet mod)

Cập nhật: 26/11/2026 ~03:00 (giờ VN). Claude khác đọc file này trước khi làm tiếp. Giao tiếp với người dùng bằng tiếng Việt.

## 1. Bối cảnh
- Repo (công khai, chỉ đọc): https://github.com/vuduocsyntt-boop/ngu-quoc-linh-thu (CONTEXT.md, yeu-cau/ngu-dai-cuong-quoc.md, yeu-cau/pet-moi.md, jar)
- Jar làm việc: goPetdevdao_v1_8_gametuoitho_v4.jar (có trong repo)
- Người dùng làm trên điện thoại, muốn nhận jar test từng hệ thống một.
- Đã bỏ hệ thống tiến hóa pet.

## 2. Quyết định đã chốt
- Năm nước: Hỏa Quốc/Mộc Diệp/Hokage, Lôi Quốc/Vân Ẩn/Raikage, Thủy Quốc/Vũ Ẩn/Mizukage, Phong Quốc/Sa Ẩn/Kazekage, Thổ Quốc/Thạch Ẩn/Tsuchikage.
- Việc hiện tại: hệ thống MAPS. Người dùng gửi 1 ảnh tileset làng lá pixel-art (1536x1024) và yêu cầu gắn từng chi tiết của ảnh vào đúng chi tiết bối cảnh tương ứng trong maps (không dán nguyên ảnh).
- Quy ước: giữ nguyên tên file và kích thước ảnh gốc của game.
- Giới hạn Claude đã nói với người dùng: không vẽ lại nhân vật có sẵn của truyện (pet Akatsuki) và không vẽ lại logo làng có sẵn; làm huy hiệu/linh thú nguyên bản. Ảnh tileset là do người dùng cung cấp nên dùng để cắt ghép.

## 3. Phát hiện kỹ thuật về jar
- Ảnh nằm trong assets/{ui,pets,items,chars,maps}.dat, định dạng GPDT không mã hóa: "GPDT" + uint16 số mục + mỗi mục [uint16 độ dài tên][tên UTF-8][uint32 offset][uint32 size], dữ liệu ngay sau chỉ mục (offset đầu = hết chỉ mục). Số lớn big-endian.
- assets/maps.dat: 260 mục dưới newMapData/ (251 png + 9 file không đuôi là dữ liệu hoạt ảnh, vd 107_b, 108_b: KHÔNG đụng vào).
- ui.dat có ui/maps/ (44 ảnh: nhà bang hội 158x126, 117x115, 116x81, 124x76; các frame hoạt ảnh).
- data/*.json bị mã hóa (header GPENC2D), giải mã bằng offline/ci.class (AES, khóa là mảng int XOR 90). CHƯA giải mã. Đây là nơi chứa tên Hệ/Lớp/Cõi/pet thật, cần cho hệ thống pet.
- Tên cường quốc/làng/Kage không có sẵn trong jar.

## 4. Tiến độ maps
Đã xong:
- Tải repo, giải nén jar, trích toàn bộ ảnh GPDT ra thư mục làm việc.
- Tách tileset thành ~131 chi tiết có nền trong suốt (script dùng ngưỡng sáng + độ bão hòa để xóa nền ca-rô, gán nhãn thành phần liên thông). Một số chi tiết dính nhau (cây+đèn, cây anh đào+bụi, bậc thang gỗ/đá nằm trong cụm lớn) nên cắt tay theo tọa độ trên ảnh gốc:
  - anh đào trái ~(350,655)-(480,790), anh đào phải ~(600,650)-(730,760)
  - bậc thang gỗ ~(835,548)-(930,660), bậc thang đá ~(930,548)-(1015,660)
- Đã lập bảng đối chiếu chi tiết game ↔ tileset (mục 5).
Đang làm: viết script thay ảnh (resize theo kích thước gốc, neo đáy-giữa), ghi lại maps.dat, nén jar.
Việc tiếp theo: xem ảnh so sánh trước/sau, kiểm tra kích thước từng file, đóng gói jar test, gửi người dùng; sau đó mới sang hệ thống ngũ đại cường quốc rồi pet.

## 5. Bảng đối chiếu (id game newMapData → chi tiết tileset)
Mã tileset theo nhãn trong bảng tách (cat_a/cat_b): số = id thành phần.
- Cây: 153→198, 157/159→anh đào (cắt tay), 158→174 (thông), 107/389/402→172/120/214, 403→123, 41/175/266→162, 265→177, 261→172, 262→198, 240→147
- Bụi/hoa: 37/38/40→109/241/246/247/211, 201→149, 202→148, 184→247, 170-173/205/206/207/208/264→bụi nhỏ (211/246/247/241/84), 263→109, 267→245, 268-270→149/110
- Đá: 212/213→219, 215→222, 203→66, 186/187/188/226/227/223/225→66/222/219, 164→222, 165→220
- Rào/đèn/biển: 12→74, 160→86, 247→238, 10→141, 166→232, 255→143/166, 168→199, 260/441→212, 224→212
- Tượng: 232→136, 258→136, 348-353/384/385/399/400→136
- Nhà: 177→29, 193→21, 192→23, 189→76, 216→56, 301→27, 302→29, 303→128, 304→56, 18→167 (torii)
- Thùng/đồ: 209→240, 210→244, 211→216, 214→245, 391→218, 401→216
- Bậc thang 289/291/296/298 và 290/297/293: cắt từ bậc thang gỗ/đá của tileset
- Sàn gỗ 274-288: dùng kết cấu ván gỗ tileset, phần viền/lan can giữ hình dạng gốc và đổi sang tông nâu gỗ
- GIỮ NGUYÊN (sự kiện/hiệu ứng/UI): 1, 2-7, 13-17, 72_a, 107_a, 108_a, 194_a, 190_a, 218_a, 237-239, 256, 257, 359/360, các hiệu ứng lửa/vết nứt 361-398, 406-418, 421, 436, 438-440, 442, 443, 1027, các khung 422-435
- Dải ngang (151, 161-163, 179, 180, 219-221, 242-246, 295, 299, 334, 337, 371...): chưa xử lý, quyết định sau khi xem kết quả.

## 6. Chưa xử lý / rủi ro
- Chưa có bản jar test nào cho maps.
- Cảnh lớn phía trên tileset (vách đá Hokage + dãy nhà) dính thành một khối, chưa tách; có thể dùng cho nhà lớn nếu người dùng muốn.
- Ảnh gốc là JPEG nên mép hơi mờ; phóng nhỏ về kích thước game có thể mất nét.
- Cần người dùng test jar trên điện thoại rồi phản hồi.

## 7. Thư mục làm việc (chỉ tồn tại trong phiên cũ, tạo lại nếu mất)
scratchpad/jar (jar giải nén), scratchpad/x (ảnh GPDT đã tách), scratchpad/ts (tileset đã tách: ts/c/<id>.png, ts/boxes.json).
