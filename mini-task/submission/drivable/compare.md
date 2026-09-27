# So sánh drivable

> Các số dưới đây là số của công cụ so sánh, không phải ngưỡng chấm.

Polygon BDD100K đổi từ toạ độ normalized sang pixel 1280×720. BDD không có tag needs_review.

Trùng từng đỉnh với reference: 0/10 shape (<= 0,5 px).

## c3cd6c82-b5d52beb.jpg

- direct: IoU 0.961; chỉ reference 410 px; chỉ bạn 3009 px
- direct: vùng hình học khác reference (410 px thiếu, 3009 px thừa) — gợi ý `geometry`
- alternative: IoU 0.951; chỉ reference 2335 px; chỉ bạn 2 px
- alternative: vùng hình học khác reference (2335 px thiếu, 2 px thừa) — gợi ý `geometry`
- mọi vùng: IoU 0.957; chỉ reference 2745 px; chỉ bạn 3011 px
- mọi vùng: vùng hình học khác reference (2745 px thiếu, 3011 px thừa) — gợi ý `geometry`

## c068a67b-03b6e200.jpg

- direct: IoU 0.977; chỉ reference 445 px; chỉ bạn 1991 px
- direct: vùng hình học khác reference (445 px thiếu, 1991 px thừa) — gợi ý `geometry`
- alternative: IoU 0.960; chỉ reference 2145 px; chỉ bạn 255 px
- alternative: vùng hình học khác reference (2145 px thiếu, 255 px thừa) — gợi ý `geometry`
- mọi vùng: IoU 0.971; chỉ reference 2590 px; chỉ bạn 2246 px
- mọi vùng: vùng hình học khác reference (2590 px thiếu, 2246 px thừa) — gợi ý `geometry`

## c723ad21-efed33e5.jpg

- direct: IoU 0.970; chỉ reference 3134 px; chỉ bạn 1012 px
- direct: vùng hình học khác reference (3134 px thiếu, 1012 px thừa) — gợi ý `geometry`
- alternative: IoU 0.890; chỉ reference 530 px; chỉ bạn 2647 px
- alternative: vùng hình học khác reference (530 px thiếu, 2647 px thừa) — gợi ý `geometry`
- mọi vùng: IoU 0.958; chỉ reference 3659 px; chỉ bạn 3208 px
- mọi vùng: vùng hình học khác reference (3659 px thiếu, 3208 px thừa) — gợi ý `geometry`

## bb5cc516-c98d1fbe.jpg

- direct: IoU 0.954; chỉ reference 345 px; chỉ bạn 1610 px
- direct: vùng hình học khác reference (345 px thiếu, 1610 px thừa) — gợi ý `geometry`
- alternative: IoU 0.941; chỉ reference 2124 px; chỉ bạn 820 px
- alternative: vùng hình học khác reference (2124 px thiếu, 820 px thừa) — gợi ý `geometry`
- mọi vùng: IoU 0.947; chỉ reference 2469 px; chỉ bạn 2430 px
- mọi vùng: vùng hình học khác reference (2469 px thiếu, 2430 px thừa) — gợi ý `geometry`

## be860305-899a96c3.jpg

- direct: IoU 0.952; chỉ reference 1873 px; chỉ bạn 1262 px
- direct: vùng hình học khác reference (1873 px thiếu, 1262 px thừa) — gợi ý `geometry`
- alternative: IoU 0.942; chỉ reference 2230 px; chỉ bạn 49 px
- alternative: vùng hình học khác reference (2230 px thiếu, 49 px thừa) — gợi ý `geometry`
- mọi vùng: IoU 0.949; chỉ reference 4103 px; chỉ bạn 1311 px
- mọi vùng: vùng hình học khác reference (4103 px thiếu, 1311 px thừa) — gợi ý `geometry`

## Dòng gợi ý cho comparison_log.csv

```csv
task,sample,object,difference,error_type,who_is_right,action,note
drivable,c3cd6c82-b5d52beb.jpg,direct,"direct: vùng hình học khác reference (410 px thiếu, 3009 px thừa)",geometry,,,
drivable,c3cd6c82-b5d52beb.jpg,alternative,"alternative: vùng hình học khác reference (2335 px thiếu, 2 px thừa)",geometry,,,
drivable,c3cd6c82-b5d52beb.jpg,mọi vùng,"mọi vùng: vùng hình học khác reference (2745 px thiếu, 3011 px thừa)",geometry,,,
drivable,c068a67b-03b6e200.jpg,direct,"direct: vùng hình học khác reference (445 px thiếu, 1991 px thừa)",geometry,,,
drivable,c068a67b-03b6e200.jpg,alternative,"alternative: vùng hình học khác reference (2145 px thiếu, 255 px thừa)",geometry,,,
drivable,c068a67b-03b6e200.jpg,mọi vùng,"mọi vùng: vùng hình học khác reference (2590 px thiếu, 2246 px thừa)",geometry,,,
drivable,c723ad21-efed33e5.jpg,direct,"direct: vùng hình học khác reference (3134 px thiếu, 1012 px thừa)",geometry,,,
drivable,c723ad21-efed33e5.jpg,alternative,"alternative: vùng hình học khác reference (530 px thiếu, 2647 px thừa)",geometry,,,
drivable,c723ad21-efed33e5.jpg,mọi vùng,"mọi vùng: vùng hình học khác reference (3659 px thiếu, 3208 px thừa)",geometry,,,
drivable,bb5cc516-c98d1fbe.jpg,direct,"direct: vùng hình học khác reference (345 px thiếu, 1610 px thừa)",geometry,,,
drivable,bb5cc516-c98d1fbe.jpg,alternative,"alternative: vùng hình học khác reference (2124 px thiếu, 820 px thừa)",geometry,,,
drivable,bb5cc516-c98d1fbe.jpg,mọi vùng,"mọi vùng: vùng hình học khác reference (2469 px thiếu, 2430 px thừa)",geometry,,,
drivable,be860305-899a96c3.jpg,direct,"direct: vùng hình học khác reference (1873 px thiếu, 1262 px thừa)",geometry,,,
drivable,be860305-899a96c3.jpg,alternative,"alternative: vùng hình học khác reference (2230 px thiếu, 49 px thừa)",geometry,,,
drivable,be860305-899a96c3.jpg,mọi vùng,"mọi vùng: vùng hình học khác reference (4103 px thiếu, 1311 px thừa)",geometry,,,
```

Hãy điền `who_is_right`, `action`, `note`; loại lỗi chỉ là gợi ý.
