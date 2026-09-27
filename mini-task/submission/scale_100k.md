# Nếu scale lên 100k frames

Họ tên: Nguyễn Minh Đức
Chế độ: cá nhân · Phân tích systematic defect cho 4 road elements

Mỗi mini-task trả lời một câu: **"Nếu scale lên 100k frames, lỗi nào sẽ trở thành systematic defect?"** Dựa vào các lỗi và ca mơ hồ gặp trong buổi lab.

## Lane

- **Lỗi & bằng chứng:** Lỗi tự ý kéo dài vạch kẻ đường xuyên qua chướng ngại vật hoặc dừng tùy tiện khi vạch bị xe che khuất / mờ dần. Bằng chứng cụ thể: task `lane`, ảnh `bb890202-d9d48310.jpg` (vạch tim làn tại góc cản sau xe SUV) và `c95fecc3-41401a5f.jpg` (lóa kính lái, vạch phía xa nhạt màu).
- **Vì sao lặp lại có hệ thống:** Khi thiếu quy tắc dừng dứt khoát định lượng, annotator thường mắc thiên kiến hoàn thiện (completion bias) — vì biết con đường phía trước vẫn có làn nên tự động nối polyline qua gầm xe hoặc xuyên qua thân xe. Trên quy mô 100.000 frame, model sẽ học đặc trưng giả (spurious correlation) rằng vạch kẻ đường luôn tồn tại liên tục ngay cả khi mặt đường bị che khuất hoàn toàn. Khi vận hành thực tế, xe tự hành sẽ dự đoán sai tim làn khi có xe lớn cắt đầu hoặc chuyển làn đột ngột.
- **Cách phát hiện sớm:** Oversample lát cắt cảnh kẹt xe (bumper-to-bumper), xe dừng ngã tư, và cảnh lóa nắng/mưa ướt; thiết lập script QC tự động kiểm tra giao cắt không gian giữa polyline vạch đường và 2D box xe cộ (nếu polyline cắt qua box xe mà không có cờ occluded/split thì gắn cờ cảnh báo yêu cầu kiểm tra lại).

## Drivable area

- **Lỗi & bằng chứng:** Lỗi polygon lấn sát sườn hoặc lách sâu vào khe bánh của dãy xe đỗ ven đường, và co cụm vùng chạy được trong bóng tối. Bằng chứng cụ thể: task `drivable`, ảnh `c068a67b-03b6e200.jpg` (dãy ô tô đỗ bên phải) và `c723ad21-efed33e5.jpg` (hầm chui tối).
- **Vì sao lặp lại có hệ thống:** Người gán nhãn có xu hướng phản xạ "tô màu nhựa đường" thay vì phân tích vùng chức năng lưu thông an toàn. Khi thấy mặt đường dưới gầm hoặc giữa 2 xe đỗ còn hở, annotator kéo polygon sát vào mép lốp xe. Khi scale lên 100k frame, model segmentation sẽ xuất ra vùng cho phép lái xe áp sát xe đỗ. Hậu quả downstream là hệ thống path planning sẽ vạch quỹ đạo không có khoảng đệm an toàn, gây tai nạn nghiêm trọng nếu xe đỗ bất ngờ mở cửa (dooring hazard) hoặc người đi bộ bước ra từ điểm mù giữa 2 xe đỗ.
- **Cách phát hiện sớm:** Oversample lát cắt đường đô thị chật hẹp có xe đỗ dày đặc hai bên, hầm chui và bóng râm tương phản gắt; áp dụng kiểm tra hình học tự động tính khoảng cách tối thiểu giữa biên drivable và bounding box của parked vehicles (cảnh báo vi phạm nếu khoảng cách < 0.3m).

## Traffic sign

- **Lỗi & bằng chứng:** Lỗi đoán mò class cụ thể cho các biển báo nhỏ ở xa (< 30px) hoặc bị mờ nhòe. Bằng chứng cụ thể: task `traffic_sign`, ảnh `00206.png` tại tọa độ `[827, 471, 847, 493]` (biển tròn xanh 20×22 px, bài làm gán `unknown` class thay vì đoán `38 keep right`).
- **Vì sao lặp lại có hệ thống:** Do áp lực phải gán đủ nhãn hoặc danh mục class dài (43 lớp GTSDB), annotator có thói quen phóng to cực đại và suy luận theo ngữ cảnh (ví dụ thấy bùng binh thì đoán là biển vòng xuyến, thấy ngã ba thì đoán là rẽ phải). Trên 100.000 frame, điều này bơm vào tập huấn luyện hàng ngàn nhãn sai ở vùng đuôi phân phối (long-tail error). Model học vẹt đặc trưng nhiễu và sẽ phân loại sai biển báo ở khoảng cách xa, dẫn đến việc xe tự hành nhận diện nhầm biển tốc độ hoặc biển cấm từ cự ly 50–70m, gây phanh gấp hoặc vi phạm luật giao thông.
- **Cách phát hiện sớm:** Phân tầng dữ liệu kiểm tra theo diện tích bounding box (các bin: <32px, 32–64px, >64px); oversample lát cắt biển nhỏ ở xa và biển lóa ngược sáng; chạy QC tự động: các box có kích thước < 30px mà gán class chi tiết (trừ các biển có hình học độc nhất như Stop/Give way) phải được chuyển sang hàng đợi kiểm duyệt chéo (double-blind verification).

## Traffic light

- **Lỗi & bằng chứng:** Lỗi lệch frame chuyển tiếp trạng thái đèn (temporal defect) và gán sai thuộc tính `relevance` đối với đèn rẽ nhánh. Bằng chứng cụ thể: task `traffic_light`, clip `dayClip5` frame 14–15 của Track 0 và gán `relevance` cho Track 2.
- **Vì sao lặp lại có hệ thống:** Trong video sequence, mắt người rất dễ bị đánh lừa bởi bóng đèn chuyển pha (đèn đỏ đang tắt và đèn xanh le lói), dẫn tới việc đặt keyframe chuyển state sớm 1–2 frame (khoảng 33–66ms). Ngoài ra, nếu annotator không hiểu lộ trình xe ego, họ sẽ mặc định gán tất cả đèn nhìn thấy là `relevant`. Trên 100.000 frame, nếu model học nhãn đèn rẽ trái đỏ là `relevant` cho xe đi thẳng, xe tự hành sẽ dừng đứng giữa ngã tư dù đèn đi thẳng đang xanh, gây tắc nghẽn và nguy cơ bị đâm từ phía sau (rear-end crash). Ngược lại, nếu state xanh bị dự đoán sớm, xe có thể khởi hành trước khi luồng giao thông cắt ngang giải tỏa hết.
- **Cách phát hiện sớm:** Lọc tự động các cửa sổ thời gian ±3 frame quanh điểm đổi state; kiểm tra tính liên tục thời gian (temporal consistency checks); oversample các giao lộ phức tạp có nhiều cụm đèn phân làn (đèn rẽ trái, rẽ phải, đi thẳng) và kiểm tra ma trận tương quan giữa làn đường của xe và cờ `relevance`.
