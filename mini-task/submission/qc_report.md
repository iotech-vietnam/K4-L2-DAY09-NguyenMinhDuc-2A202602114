# QC report

Họ tên: Nguyễn Minh Đức · Chế độ: cá nhân
Guideline dùng: `GUIDE.md` + 4 card, bản phát ngày học.

Viết ở phút 205–225. Cold QC: Đánh giá lại độc lập toàn bộ các task đã thực hiện, đối chiếu chéo các lỗi geometry, attribute, temporal và ca mơ hồ với góc nhìn khách quan.

## 1. Sample plan

Không đủ thời gian xem hết. Chọn **6 sample** và nói vì sao chọn. Lấy theo lát dễ lỗi (ngã tư, crosswalk, đêm/mưa, lóa, biển nhỏ, điểm chuyển state), không lấy ngẫu nhiên.

| # | Task | Sample (ảnh / frame) | Lát (vì sao chọn) |
|---|---|---|---|
| 1 | lane | c1589305-200e315b.jpg | Lát ngã tư (intersection) phức tạp: có vạch đi bộ qua đường (crosswalk), vạch dừng (stop line), và vạch rẽ bị xe che một phần |
| 2 | lane | c95fecc3-41401a5f.jpg | Lát kính lái lóa sáng (windshield glare): vạch sơn phía xa mờ nhạt, độ tương phản thấp dễ vẽ sai điểm dừng |
| 3 | drivable | c068a67b-03b6e200.jpg | Lát phố đô thị có xe đỗ hàng dài bên phải: nguy cơ cao polygon lấn sát sườn hoặc chui vào khe giữa 2 bánh xe đỗ |
| 4 | drivable | c723ad21-efed33e5.jpg | Lát hầm chui (tunnel) tối: ánh sáng tương phản mạnh giữa cửa hầm và trong hầm, biên vùng chạy được mờ mịt |
| 5 | traffic_sign | 00206.png | Lát biển báo kích thước nhỏ (< 25px) ở xa: biển tròn xanh mờ nhòe dễ bị annotator đoán mò class theo bối cảnh |
| 6 | traffic_light | dayClip5--01620.jpg (frame 14–15) | Lát điểm chuyển tiếp trạng thái đèn (state transition phase): ranh giới chuyển pha giữa đỏ và xanh dễ lệch frame |

## 2. Lỗi tìm thấy

Ít nhất 1 lỗi geometry, 1 lỗi attribute và 1 ca cần vào decision log.

- `error_type`: `geometry`, `missing`, `class`, `attribute`, `temporal`, `guideline_gap` (taxonomy của buổi học).
- `severity`:
  `critical` = đổi quyết định của ego (state/relevance sai, drivable lấn sang làn ngược chiều, mất lane ngay trước xe);
  `major` = sai attribute hoặc geometry mà model sẽ học theo; `minor` = lệch nhỏ, không đổi nghĩa.
- `action`: `accept`, `rework`, `escalate`.

| Task | Sample | Object | Mô tả lỗi | error_type | severity | action | Downstream sai gì nếu bỏ qua |
|---|---|---|---|---|---|---|---|
| drivable | c068a67b-03b6e200.jpg | alternative | Polygon lấn sát lốp và khoảng hở bánh xe đỗ, không chừa đệm an toàn 0.3m | geometry | major | rework | Model dự đoán vùng đi được quá sát xe đỗ, planner lập quỹ đạo sát sườn gây nguy cơ va chạm khi xe đỗ mở cửa đột ngột |
| lane | b75f355e-b3f098b9.jpg | R2/B2 | Gán gờ bó vỉa (road curb) thành single white | attribute | major | rework | Model học sai kết cấu ngăn cách vật lý thành vạch kẻ sơn phẳng, hệ thống có thể điều khiển xe leo lên vỉa hè |
| traffic_light | dayClip5--01620.jpg | R#0/B#0 | Chuyển trạng thái sang green sớm 1 frame tại frame 14 khi đèn đỏ chưa ngắt hẳn | temporal | critical | rework | Model học hành vi xuất phát non, xe tự hành có thể lao vào giao lộ khi luồng giao thông cắt ngang chưa thoát hết |
| traffic_sign | 00206.png | R3/B3 | Biển báo 20x22 px quá nhỏ bị suy đoán class 38 thay vì gán unknown | class | minor | escalate | Đưa ca biên vào DEC-004: cấm suy đoán class dưới 30px, bảo vệ tập huấn luyện khỏi nhãn nhiễu |

## 3. Kết luận cho batch

- Accept / rework / escalate cả batch, và lý do: **Accept có điều kiện (Rework cục bộ các ca biên rồi Accept toàn batch)**. Toàn bộ các lỗi phát hiện đã được khắc phục, đối chiếu kỹ với reference và bổ sung quy tắc rõ ràng vào `decision_log.csv`.
- Note cho người label (viết cho chính mình): Luôn giữ vững tư duy bằng chứng nhìn thấy: không bao giờ vẽ nối vạch qua chướng ngại vật; không ép class cho biển nhỏ mờ dưới 30px; và luôn kiểm tra tua chậm frame-by-frame tại các điểm chuyển pha đèn giao thông theo nguyên tắc an toàn cao nhất.
- Known limitation phải ghi khi handoff: Vùng drivable trong bóng tối sâu dưới hầm chui cần cảm biến Lidar hỗ trợ; các biển báo phụ trợ hình chữ nhật ngoài chuẩn 43 class GTSDB chưa có nhãn chi tiết trong ontology hiện tại.
