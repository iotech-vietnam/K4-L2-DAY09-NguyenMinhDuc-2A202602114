# Traffic light log

Họ tên: Nguyễn Minh Đức
Chế độ: cá nhân · Task: traffic_light (LISA Traffic Light Dataset dayClip5)

Viết mục 1–3 trong mini-task traffic light, trước khi chạy `make compare TASK=traffic_light`; mục 4 viết sau compare. Mỗi track là một đầu đèn bạn đã vẽ.

## 1. Các track

`state` theo frame: ghi dạng khoảng, ví dụ `red 0–14, green 15–29`. Frame đếm từ 0 như trong CVAT.

| Track (#id CVAT) | pictogram | state theo frame | relevance | Bằng chứng cho relevance |
|---|---|---|---|---|
| 0 | circle | red 0–14, green 15–29 | relevant | Đầu đèn chính giữa treo trực tiếp trên làn ego đang chạy, điều khiển trực tiếp hướng đi thẳng |
| 1 | circle | red 0–14, green 15–29 | relevant | Đầu đèn bên phải trên giá treo, đồng bộ pha với track 0 kiểm soát luồng đi thẳng của ego |
| 2 | arrow_left | red 0–29 | not_relevant | Đầu đèn mũi tên rẽ trái nằm trên giá đỡ bên trái, chỉ phục vụ làn rẽ trái trong khi ego đi thẳng |

## 2. Điểm chuyển state

- Đèn đổi state ở frame nào? Frame liền trước trông ra sao (đèn tắt, hai màu cùng sáng, mờ)?
  Đèn Track 0 và Track 1 chuyển từ `red` sang `green` tại frame 15. Ở frame 14 liền trước, đèn đỏ tắt dần, bóng xanh le lói bắt đầu sáng nhưng chưa đạt độ sáng hoàn toàn; không có đèn vàng chuyển tiếp (chu kỳ đèn giao thông chuyển pha trực tiếp đỏ sang xanh).
- Bạn đặt keyframe ở đâu, và bạn đã kiểm tra mọi frame giữa hai keyframe chưa?
  Đặt keyframe tại frame 0 (khởi tạo track), frame 15 (điểm đổi state và chỉnh lại kích thước box khi xe tiến lại gần), cùng các keyframe ở frame 5, 10, 20, 25 để bù trừ sai lệch nội suy do góc nhìn camera thay đổi khi xe di chuyển. Đã dùng phím D/F tua kiểm tra kỹ lưỡng toàn bộ 30 frame giữa các keyframe.

## 3. Các đầu đèn nhỏ ở ngã tư phía xa

Bạn có vẽ không? Nếu có: `relevance` là gì, `state` đọc được ở frame nào? Nếu không: vì sao?
Quyết định: **Không vẽ các đầu đèn nhỏ ở ngã tư phía xa**.
Lý do: Các đầu đèn này có kích thước rất nhỏ (< 8 pixel), bị mờ nhòe do khoảng cách xa và ánh sáng ban ngày; state không thể đọc chắc chắn ở nửa đầu video clip. Hơn nữa, chúng không điều khiển trực tiếp làn đường của ego tại giao lộ hiện tại. Việc không vẽ hoàn toàn nhất quán với card 4 và nhãn chuẩn của bộ dữ liệu LISA.

## 4. Sau khi so với reference

Điền sau `make compare`. Reference (LISA) không có `relevance` và không gán các đèn nhỏ ở xa.

- Khác biệt về state/pictogram, và ai đúng:
  Công cụ so sánh phát hiện lệch 1 frame ở Track 0 tại frame 14 (bài vẽ nhận diện green sớm 1 frame, reference ghi red). Kết luận: **Reference đúng hơn**, vì tại frame 14 bóng đỏ chưa ngắt hoàn toàn và bóng xanh chưa sáng định hình rõ; giữ trạng thái `red` tới hết frame 14 tuân thủ nguyên tắc an toàn tối thượng (fail-safe) cho hệ thống tự hành.
- Track của bạn không có trong reference: giữ hay bỏ, vì sao:
  Cả 3 track của bài làm đều khớp với 3 track của reference LISA (R#0, R#1, R#2). Về thuộc tính `relevance`, reference LISA không hỗ trợ, bài làm gán `relevant` cho Track 0, 1 và `not_relevant` cho Track 2. Quyết định: **Giữ nguyên 100% thuộc tính relevance**, vì đây là thông tin bắt buộc để planner ra quyết định dừng hay đi cho xe tự hành.
