# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202604050
- Ngày / CVAT local: 11/11/2026 – 12/11/2026 / CVAT local (localhost:8080)
- Công cụ đã dùng: Brush (Draw new mask / Polygon-plus), Polygon (Draw new polygon)

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `submissions/easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `submissions/medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `submissions/hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg` (ảnh 1 của `medium_instance`), người phụ nữ mặc áo dài trắng đang đi bộ sang đường trên vạch kẻ ở chính giữa tiền cảnh.
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Tự vẽ thủ công bằng công cụ `Polygon` trước khi dùng bất kỳ gợi ý nào: zoom lớn để bám sát viền tóc, quai túi đeo vai, hai ống tay áo, tà áo dài và ống quần; dừng biên ngay tại mũi dép tiếp xúc với mặt đường để không lấy phần bóng đổ mờ dưới chân và không vẽ lẹm vào chiếc xe máy cắt ngang phía sau.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Không dùng.
- Nếu không dùng gợi ý: Không dùng; tự vẽ bằng `Polygon` cho các vật thể liền khối và dùng `Draw new mask` (Brush / Polygon-plus) cho các vật bị che khuất tách làm nhiều mảng rời nhau (như chiếc `motorcycle` bị người mặc áo dài che ở ảnh 1 và chiếc `car` taxi vàng bị người đi bộ che ở ảnh 2) để giữ đúng 1 instance ID duy nhất theo phần nhìn thấy.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: Task `medium_instance` / ảnh 1 (`000000181542.jpg`) / vùng chiếc xe máy ở tiền cảnh bên trái người phụ nữ mặc áo dài và vùng gầm chiếc xe bán tải màu bạc ở trung cảnh.
- Lỗi thuộc loại: Gộp-tách instance (vật bị che khuất) và lỗi biên ăn vào bóng đổ dưới gầm xe.
- Bằng chứng tôi nhìn thấy: Chiếc xe máy của người đàn ông bên trái bị tà áo dài trắng che khuất phần giữa, chia phần nhìn thấy thành 2 mảng rời nhau (thân/bánh sau bên trái và bánh trước/chắn bùn trước bên phải tà áo dài). Nếu vẽ bằng Polygon thông thường sẽ bị tách nhầm thành 2 object riêng; đồng thời vùng dưới gầm xe bán tải màu bạc ở giữa có mảng bóng đổ đen đậm dễ bị tô lẹm vào mask của xe.
- Quy tắc và hành động sửa: Áp dụng quy tắc chỉ vẽ phần nhìn thấy và giữ nguyên 1 instance cho vật bị che khuất. Sử dụng công cụ `Draw new mask` tô mảng thân xe máy bên trái và mảng bánh trước bên phải trong cùng một lần tạo object (không đè lên tà áo dài), bật chế độ `Color by: Instance` để kiểm tra cả hai mảng mang cùng một màu ID; đồng thời cắt sát biên mask tại mép lốp và bậc gầm xe bán tải để loại bỏ phần bóng đen trên mặt đường.
- Sau sửa đã Save và export lại chưa? Đã bấm Save trên CVAT, kiểm tra lại danh sách Objects và export ra file `submissions/medium_instance.zip` theo định dạng `COCO 1.0`.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `medium_instance` / ảnh 1 (`000000181542.jpg`) — Chiếc xe màu bạc ở trung cảnh trước cửa hàng CHANEL và hình cô gái trên poster CHANEL | (a) Xe màu bạc có thể hiểu là `car` hoặc `truck` (xe bán tải); (b) Hình cô gái trong khung cửa CHANEL có gán `person` hay không. | Xe có kết cấu thùng chở hàng hở phía sau tách biệt với cabin; hình cô gái ở cửa hàng CHANEL là ảnh in phẳng 2D trên biển quảng cáo. | Gán chiếc xe bán tải màu bạc là `truck` (giữ nguyên kính cửa sổ trong mask) và bỏ qua không gán nhãn `person` cho poster quảng cáo. |
| 2. `medium_instance` / ảnh 3 — Người trượt ván giữa đường và nhóm người đẩy xe trẻ em qua đường ở xa | Có gộp tấm ván trượt (`skateboard`) dưới chân người trượt ván và chiếc xe đẩy trẻ em (`stroller`) vào mask của `person` hay không. | Danh sách 6 lớp của `medium_instance` chỉ gồm `person`, `bicycle`, `car`, `motorcycle`, `bus`, `truck`; không có lớp cho ván trượt hay xe đẩy trẻ em. | Chỉ vẽ mask `person` dừng sát tại đế giày của người trượt ván và cơ thể của những người đi bộ; để trống phần ván trượt và xe đẩy trẻ em theo quy tắc không tô ép vùng ngoài schema. |
| 3. `hard_panoptic` / ảnh 2 — Xe lồng chở 4 chiếc ô tô con ở làn đường bên trái | Cách 1: Gộp nguyên khối cả xe lồng và 4 chiếc ô tô nằm trên sàn chở hàng thành 1 instance `truck` duy nhất. Cách 2: Tách riêng từng chiếc ô tô con là `car` và phần khung giàn chở hàng là `truck`. | Theo quy tắc Panoptic, mỗi vật thể đếm được (`thing`) thuộc đúng class nhìn thấy rõ cần được tách thành instance riêng và không chồng lấn. | Tách riêng 4 chiếc ô tô nằm trên thùng xe thành 4 instance `car` độc lập, và dùng công cụ Mask gom phần khung giàn màu vàng + gầm xe chở hàng thành 1 instance `truck` không chồng lấn. |
