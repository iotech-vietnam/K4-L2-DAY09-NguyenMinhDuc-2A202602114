# So sánh lane

> Các số dưới đây là số của công cụ so sánh, không phải ngưỡng chấm.

Reference lane do người thiết kế lab vẽ theo card 1 trên 6 ảnh core, KHÔNG phải GT chính thức BDD100K. Khác reference chưa chắc là bạn sai: ghi who_is_right và lý do.

Trùng từng đỉnh với reference: 0/25 shape (<= 0,5 px).

## bb890202-d9d48310.jpg

- R2/B2: khoảng cách 0.7 px
- R4/B4: khoảng cách 1.2 px
- R3/B3: khoảng cách 1.4 px
- R6/B6: khoảng cách 1.4 px
- R5/B5: khoảng cách 1.6 px
- R1/B1: khoảng cách 2.1 px

## c1589305-200e315b.jpg

- R2/B2: khoảng cách 0.5 px
- R3/B3: khoảng cách 1.4 px
- R6/B6: khoảng cách 1.5 px
- R4/B4: khoảng cách 1.9 px
- R1/B1: khoảng cách 2.0 px
- R5/B5: khoảng cách 2.7 px

## c3cd6c82-b5d52beb.jpg

- R2/B2: khoảng cách 1.2 px
- R4/B4: khoảng cách 1.3 px
- R1/B1: khoảng cách 1.7 px
- R3/B3: khoảng cách 1.7 px

## b75f355e-b3f098b9.jpg

- R1/B1: khoảng cách 1.9 px
- R2/B2: khoảng cách 2.6 px
- R2/B2: laneTypes: bạn single white, reference road curb — gợi ý `attribute`

## c0f739d8-6ff93525.jpg

- R1/B1: khoảng cách 0.9 px
- R2/B2: khoảng cách 1.2 px

## c95fecc3-41401a5f.jpg

- R2/B2: khoảng cách 1.1 px
- R3/B3: khoảng cách 1.3 px
- R4/B4: khoảng cách 1.8 px
- R5/B5: khoảng cách 1.8 px
- R1/B1: khoảng cách 2.4 px

## Dòng gợi ý cho comparison_log.csv

```csv
task,sample,object,difference,error_type,who_is_right,action,note
lane,b75f355e-b3f098b9.jpg,R2/B2,"R2/B2: laneTypes: bạn single white, reference road curb",attribute,,,
```

Hãy điền `who_is_right`, `action`, `note`; loại lỗi chỉ là gợi ý.
