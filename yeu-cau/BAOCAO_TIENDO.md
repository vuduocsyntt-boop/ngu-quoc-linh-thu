BÁO CÁO TIẾN ĐỘ - Sửa cơ chế sinh quái (jar Sasuke)

Cập nhật: 2026-11-26 15:20. Dành cho Claude khác làm tiếp. Người dùng nói tiếng Việt, trả lời tiếng Việt, giao jar test từng bước.

1. Yêu cầu

Jar gốc: goPetdevdao_v1_8_gametuoitho_v4_sasuke_v1.jar. Người dùng auto quét quái; quét hết thì không sinh lại, phải đổi map rồi quay lại mới có quái. (Yêu cầu "mở lv100 pet" ở lượt đầu chưa làm, người dùng chuyển sang việc này.)

2. Kiến thức kỹ thuật đã xác minh
Sinh quái: offline.cz (e = quái đang sống mỗi map; d = mẫu vị trí; c = loại quái). cz.l(map) sinh các vị trí chưa có quái (mẫu không có quái sống trong 120px), chỉ chạy khi MobRespawn.lim >= số quái sống. Ngoài MobRespawn thì lim = -1 nên l() luôn rỗng (nên thắng trận không sinh ngay).
offline.MobRespawn.check(du) được MobTick.b gọi mỗi ~1 giây từ OfflineBridge.run; gửi gói 81/22 (dp.a(List)) cho client. Loại map <=0, 11, 12, 20 (20 = Lôi đài). Đợi >=3s từ lúc vào map và từ trận cuối (offline.p.b(du.o) != null = đang đánh).
Gói 81/22 cũng là gói danh sách quái khi vào map (ao.a(int,du,int): cz.g(map) rồi cz.h(map)).
Giải mã data/.json: magic GPENC2\x01\x02, rồi byte[i] ^ key[i%28] ^ ((i31+7)&255), key = GoPet2026_Secure_Key_VHD_#99.
Bộ test chạy headless: scratchpad/t/T3.java (gọi offline.dn.a(null); offline.cz.a(null) rồi cz.g/h/c/l) chạy được ngoài game: java -cp t:<thư mục jar giải nén> T3 <mapId>.
3. Các lỗi tìm ra và bản vá
v2 - cz.h() tự nạp lại toàn bộ quái vào server (gọi g) mỗi khi danh sách rỗng, mà offline.o tick gọi h() liên tục. Quét sạch -> server lặng lẽ có 12 quái, client không biết, MobRespawn thấy 12 > 2 nên không gửi. Vá: NOP lệnh iload_0; invokestatic cz.g trong cz.h (offset byte pattern 1ab80082b200561ab80187).
v3 - viết lại offline/MobRespawn (nguồn: scratchpad/src/offline/MobRespawn.java, biên dịch --release 8 rồi sửa version class về 50): (a) trận treo >15s coi như hết trận; (b) lim=100000 -> luôn bù đủ vị trí trống, không còn điều kiện "còn <=2 quái".
v4 - người dùng báo: quái mới hiện ra nhưng biến mất khi hoạt ảnh chết của quái cũ kết thúc => quái mới TRÙNG ID quái vừa chết (id = map1000+chỉ số+1), client xoá theo id. Vá: thêm lớp offline/MobId (nguồn scratchpad/src2/MobId.java), vá cz.a(int,cy,int,List,List,boolean) thay công thức id bằng MobId.id(map*1000, idx) (thêm 6 mục vào constant pool của cz.class, thay byte 122..139 của method). Mỗi lần sinh lại cùng vị trí dùng id khác: +100n với n chạy 1..9 (quái lúc vào map giữ id gốc n=0). Headless: các vòng sinh lại có id 13101.., 13201.., ...
4. Trạng thái

Jar test mới nhất: out/goPetdevdao_v1_8_gametuoitho_v4_sasuke_v4_respawn.jar (so với v1: đổi offline/cz.class, offline/MobRespawn.class, thêm offline/MobId.class). Đã kiểm tra: zip hợp lệ, java -Xverify:all chạy được, mô phỏng 8 vòng quét sạch/sinh lại đúng số quái. CHƯA chạy trong game thật. Cần người dùng báo: quái mới còn biến mất không, có dòng "[He Thong] Quai moi da xuat hien (n)" không.

5. Rủi ro / việc tiếp
Nếu quái vẫn biến mất: client có thể xoá theo VỊ TRÍ hoặc theo danh sách khác; xem client do.class (hàm đọc gói 81/22 chưa xác định được) và cách client xử lý quái chết.
Id có thể chạm id của map khác khi n lớn (chỉ tới +900, trong cùng map nên không sao); cz.f() (map 20, 35) dùng công thức id riêng, không qua MobId.
Quái "đặc biệt" (cờ u, 30% xuất hiện) có logic riêng, chưa kiểm thử riêng.
6. Cập nhật 15:35 - v5: thêm item "bình x4 exp 5 tiếng"
Jar: out/goPetdevdao_v1_8_gametuoitho_v4_sasuke_v5_binhx4_5h.jar (gồm toàn bộ bản vá respawn v4).
Thêm item id 6015 (type 10, sao chép từ 6014 "bình x4 exp", icon items/61036.png) vào data/item.json; thêm vào shop id 39 (entry id 137, tiền loại 10, giá 1000 = 10 lần giá 6014 là 100) trong data/shop.json. Dữ liệu mã hóa lại bằng cùng thuật toán (key ở mục 2).
Cơ chế dùng bình: ao.class có 2 chỗ gọi offline.z.a(du, itemId, 30) (30 phút cố định) khi item type==10. Đã vá: thay bằng offline/ExpPotion.a(du,id): id 6015 -> z.a(du, 6014, 300) (x4, 300 phút), còn lại 30 phút như cũ; thay phép so sánh id==6014/id==200 (chọn x4 cho thông báo) bằng ExpPotion.m(id); sửa thông báo thành "...trong thời gian hiệu lực của bình!" (trước ghi cứng 30 phút). Nguồn scratchpad/src3/ExpPotion.java.
z.a cộng dồn thời gian nếu buff đang chạy (ag > now), hệ số tối đa 4.0 (có chống cheat cắt về 4).
Đã kiểm tra: ao.class qua -Xverify:all, item 6015 đọc được (type 10). CHƯA chạy trong game; chưa thử trong game việc dùng bình và hiển thị tên/giá ở shop.
7. Cập nhật 15:50 - v6: sửa chỗ bán bình x4 5 tiếng
Lỗi v5: mình thêm item vào shop.json ShopId 39 nhưng shop người dùng mở (shop bình/thức ăn, nơi bán bình x4 30 phút giá 60 loại tiền 1) là shop 19001 lập trình cứng trong offline.eu.b(int) (không đọc shop.json). Bảng 19001 gồm 14 dòng [itemId, loạiTiền, giá]: 6001, 6002, 6009, 6010, 6011, 6014(1,60), 7003, 7054, 7055, 8007, 7056, 7063, 8020, 7005.
Vá v6 (không chèn bytecode): thêm lớp offline/ShopX (nguồn scratchpad/src4/ShopX.java) có b(int) gọi eu.b(int) rồi nếu shop==19001 thì thêm dòng et(maxId+1, 19001, 6015, 0, 1, {1}, {600}, true) (loại tiền 1, giá 600 = 10 lần bình 30 phút). Đổi Methodref eu.b:(I)List (#2185 của ao.class) sang lớp ShopX nên cả 14 chỗ gọi trong ao đều đi qua ShopX.
Đã test headless: shop 19001 ra 15 dòng, dòng cuối item 6015 giá 600, gọi 2 lần không bị nhân đôi. CHƯA chạy trong game.
Jar: out/goPetdevdao_v1_8_gametuoitho_v4_sasuke_v6_binhx4_5h.jar.