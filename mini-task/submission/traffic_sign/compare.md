# So sánh traffic_sign

> Các số dưới đây là số của công cụ so sánh, không phải ngưỡng chấm.

Box GTSDB (43 class Đức). GT không có readable, truncated, relevant_to_ego — các attribute này tự đối chiếu bằng decision log.

Trùng từng đỉnh với reference: 0/21 shape (<= 0,5 px).

## 00073.png

- R4/B4: IoU 0.837
- R1/B1: IoU 0.804
- R2/B2: IoU 0.799
- R3/B3: IoU 0.758
- R5/B5: IoU 0.755
- R6/B6: IoU 0.751
- readable, truncated, relevant_to_ego: GT không có, không so — tự đối chiếu bằng decision log

## 00206.png

- R4/B4: IoU 0.956
- R5/B5: IoU 0.938
- R2/B2: IoU 0.930
- R1/B1: IoU 0.920
- R3/B3: IoU 0.755
- R3/B3: sign_class: bạn unknown, reference 38 keep right — gợi ý `class`
- readable, truncated, relevant_to_ego: GT không có, không so — tự đối chiếu bằng decision log

## 00054.png

- R1/B1: IoU 0.842
- R2/B2: IoU 0.822
- R3/B3: IoU 0.813
- R4/B4: IoU 0.769
- readable, truncated, relevant_to_ego: GT không có, không so — tự đối chiếu bằng decision log

## 00088.png

- R2/B2: IoU 0.826
- R3/B3: IoU 0.803
- R1/B1: IoU 0.781
- R4/B4: IoU 0.775
- readable, truncated, relevant_to_ego: GT không có, không so — tự đối chiếu bằng decision log

## 00026.png

- R1/B1: IoU 0.870
- readable, truncated, relevant_to_ego: GT không có, không so — tự đối chiếu bằng decision log

## 00223.png

- R1/B1: IoU 0.855
- readable, truncated, relevant_to_ego: GT không có, không so — tự đối chiếu bằng decision log

## Dòng gợi ý cho comparison_log.csv

```csv
task,sample,object,difference,error_type,who_is_right,action,note
traffic_sign,00206.png,R3/B3,"R3/B3: sign_class: bạn unknown, reference 38 keep right",class,,,
```

Hãy điền `who_is_right`, `action`, `note`; loại lỗi chỉ là gợi ý.
