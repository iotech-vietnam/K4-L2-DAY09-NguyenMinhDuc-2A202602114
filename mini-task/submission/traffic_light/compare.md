# So sánh traffic_light

> Các số dưới đây là số của công cụ so sánh, không phải ngưỡng chấm.

Track LISA dayClip5, 30 frame. LISA không gán nhãn đèn nhỏ ở ngã tư phía xa và không có relevance — box không khớp GT chưa chắc là thừa.

Trùng từng đỉnh với reference: 0/90 shape (<= 0,5 px).

## dayClip5--01606.jpg

- R#0/B#0: frame đổi state reference 15; bạn 14
- B#0: relevance=relevant (không so với GT)
- R#1/B#1: frame đổi state reference 15; bạn 15
- B#1: relevance=relevant (không so với GT)
- R#2/B#2: cả reference và bạn đều không đổi state
- B#2: relevance=not_relevant (không so với GT)

## dayClip5--01620.jpg

- R#0/B#0 state, frame 14: bạn green, reference red — gợi ý `temporal`

## Dòng gợi ý cho comparison_log.csv

```csv
task,sample,object,difference,error_type,who_is_right,action,note
traffic_light,dayClip5--01620.jpg,R#0/B#0,"R#0/B#0 state, frame 14: bạn green, reference red",temporal,,,
```

Hãy điền `who_is_right`, `action`, `note`; loại lỗi chỉ là gợi ý.
