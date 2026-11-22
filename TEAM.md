# TEAM — Ngày 12

Mã bộ ảnh: **G06** · Hình thức: cá nhân (được ghép cặp)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Ngô Trí Anh | 2A202604053 | có | Nguyễn Quốc Vương (2A202604052) | 51 dòng (xử lý tất cả) |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- S01_rgb.png / face / box — peer không có box face ở S01_rgb; mình có (IoU=0.00). Cần xác nhận ảnh này có mặt người không.
- S06_rgb.png / face / box — hai box face khác nhau đáng kể (IoU=0.49); cần xem trực tiếp trên CVAT.

## Cá nhân

Mỗi người một mục, 1–2 dòng mỗi ý.

### NgoTriAnh-2A202604053
- Phần mình làm: gán nhãn đủ 22 ảnh (S01–S08 × RGB+IR, D01–D06), chạy cabin_check checkpoint A, B và final, diff export với peer Nguyễn Quốc Vương, điền disagreement_log.
- Điều mình học được hôm nay: Ánh xạ ghế theo xe (rear_left/right) dễ nhầm khi nhìn theo ảnh; ảnh Drive&Act bị xoay ~90° nên trái/phải mặt tài xế phải hiểu theo người ngồi, không theo ảnh; IR-sim của SVIRO là ảnh xám mô phỏng (không phải IR thật), nên seatbelt thường là unknown vì không thấy dây trên nền tối.
