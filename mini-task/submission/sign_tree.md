# Traffic sign tree

Họ tên: Nguyễn Minh Đức · Chế độ: cá nhân
Guideline dùng: `GUIDE.md` + Card 3 (Traffic sign).

Viết sau mini-task traffic sign (phút ~155). Dựa vào những biển **bạn đã vẽ** trong 7 ảnh core, không chép danh sách 43 class.

## 1. Cây của bạn

Chỉ liệt kê class có trong ảnh core. Đếm số box của từng class. Class có 1 box trong cả batch là ứng viên "hiếm".

| family | class (`sign_class`) | Số box trong core | Phổ biến / hiếm | Ảnh ví dụ |
|---|---|---|---|---|
| prohibitory | 00 speed limit 20 | 1 | hiếm | 00054.png |
| prohibitory | 01 speed limit 30 | 2 | phổ biến | 00026.png, 00223.png |
| prohibitory | 02 speed limit 50 | 2 | phổ biến | 00073.png |
| prohibitory | 08 speed limit 120 | 2 | phổ biến | 00088.png |
| prohibitory | 09 no overtaking | 2 | phổ biến | 00073.png |
| prohibitory | 10 no overtaking (trucks) | 2 | phổ biến | 00088.png |
| mandatory | 33 go right | 1 | hiếm | 00206.png |
| mandatory | 34 go left | 1 | hiếm | 00206.png |
| mandatory | 38 keep right | 2 | phổ biến | 00206.png, 00054.png |
| danger | 23 slippery road | 2 | phổ biến | 00073.png |
| danger | 27 pedestrian crossing | 1 | hiếm | 00054.png |
| other | 12 priority road | 1 | hiếm | 00054.png |
| other | 13 give way | 2 | phổ biến | 00206.png |

## 2. Hai quyết định merge/split

Mỗi quyết định: giữ tách hay gộp, vì sao, và cái giá nếu chọn sai (model downstream nhầm gì).

**Quyết định 1 — `09 no overtaking` và `10 no overtaking (trucks)`: tách hay gộp?**
Quyết định: **Tách riêng**.
Lý do: Ý nghĩa điều khiển xe tự hành cho hai biển này hoàn toàn khác nhau. Biển `09` cấm mọi phương tiện cơ giới vượt nhau (áp dụng trực tiếp cho xe ego là xe con), trong khi biển `10` chỉ áp dụng đối với xe tải có khối lượng chuyên chở vượt quy định (xe ego là ô tô con vẫn được phép vượt nếu đủ an toàn).
Cái giá nếu chọn sai: Nếu gộp thành một class `no_overtaking` chung, planner của xe ego sẽ xử lý sai lầm: xe con sẽ tưởng nhầm mình bị cấm vượt khi gặp biển cấm xe tải, dẫn tới hành vi lái xe quá thận trọng, rụt rè bất hợp lý và gây cản trở lưu thông trên cao tốc. Ngược lại nếu gộp lơ là, xe con có thể vượt sai luật ở đoạn đường cấm toàn bộ phương tiện vượt.

**Quyết định 2 — nhóm `other` của GTSDB khi dùng ở Việt Nam.**
GTSDB xếp biển hết hạn chế (`06`, `32`, `41`, `42`) vào `other`. QCVN 41:2024/BGTVT xếp biển hết hiệu lực (`DP.133`–`DP.135`) vào nhóm **biển báo cấm**. Cây của bạn theo cách nào, và cần rule gì để hai người label giống nhau?
Cây phân loại theo cách: **Tuân thủ QCVN 41:2024/BGTVT khi triển khai hệ thống tại Việt Nam**, đưa các biển hết hạn chế cấm vào nhóm `prohibitory` thay vì `other`.
Rule thống nhất để hai người label giống nhau: "Tất cả các biển tròn viền trắng/xám có vạch chéo đen xóa bỏ lệnh cấm (tương đương nhóm DP.133, DP.134, DP.135 theo QCVN 41) đều thuộc family `prohibitory`. Chỉ những biển chỉ dẫn thông tin hình vuông/chữ nhật hoặc biển cảnh báo đặc thù mới xếp vào `other`."

## 3. Chính sách cho class hiếm và biển không đọc được

Khi gặp biển không có trong 43 class, hoặc quá nhỏ để đọc: bạn chọn `sign_family`, `sign_class`, `readable` thế nào? Dẫn một box cụ thể (ảnh + vị trí) làm bằng chứng.
Chính sách:
- Phân định rõ 2 cấp độ: nếu nhận diện được hình dạng ngoài và màu sắc cơ bản (ví dụ hình tròn xanh, tam giác viền đỏ), người gán nhãn chọn đúng `sign_family` (`mandatory`, `danger`, `prohibitory`), nhưng nếu ký hiệu biểu tượng bên trong bị vỡ hạt mờ nhòe thì bắt buộc chọn `sign_class = unknown`.
- Đặt `readable = uncertain` hoặc `no`.
- Tuyệt đối không phóng to ảnh rồi suy đoán class theo trí tưởng tượng hoặc bối cảnh.
Bằng chứng cụ thể: Trên ảnh `00206.png`, box R3/B3 tại tọa độ `[827, 471, 847, 493]` có kích thước chỉ 20×22 pixel. Mắt thường nhìn thấy rõ khung tròn nền xanh nên xác định chắc chắn `sign_family = mandatory`, nhưng mũi tên hướng đi bên trong bị nhòe pixel không rõ rẽ phải hay đi thẳng, nên chọn `sign_class = unknown`, `readable = uncertain`.

## 4. Dòng decision log tương ứng

Id của dòng trong `decision_log.csv` ghi rule ở mục 2 hoặc 3: **`DEC-004`** (và liên hệ DEC-001/002 cho quy tắc gán nhãn).
