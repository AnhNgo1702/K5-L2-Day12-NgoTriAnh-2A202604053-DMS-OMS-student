# Tự đánh giá — Ngô Trí Anh · G06

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S02_rgb.png | child_seat (rear_right) | seat_flip | Peer ghi rear_left — nhầm hướng ghế theo xe vs theo ảnh; đã kiểm tra và xác nhận rear_right là đúng (ghế bên trái ảnh = rear_right xe) | Không cần sửa (đúng rồi) |
| S06_rgb.png | face | box | IoU thấp với peer (0.49) — box của mình bao chặt hơn theo guideline "bao khớp mặt thực" | Không cần sửa |
| D05.png | face.eye_right | guess | Ghi unknown thay vì open (peer ghi open) — mắt bị khuất một phần nên mình an toàn ghi unknown | Không cần sửa (unknown đúng hơn) |
| D06.png | face.eye_left | guess | Peer ghi open, mình ghi closed — vùng mắt trái tối do ánh sáng bên; mình ghi closed có thể là đoán, nên đổi sang unknown | Đã sửa → unknown |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Nhầm hướng trái/phải của ghế (seat_flip) — đặc biệt với S02 khi ghế em bé và ghế trẻ nằm ngược chiều nhau trong ảnh.

Vì sao mình mắc: Nhìn theo ảnh (trái/phải ảnh) thay vì theo xe (rear_left = phía sau bên trái xe = bên phải ảnh khi nhìn từ sau ra trước).

Lần sau tránh bằng cách: Luôn tự hỏi "camera nhìn từ đâu?" — camera OMS nhìn từ phía sau xe ra phía trước, nên trái ảnh = phải xe. Dùng quy tắc: rear_right = góc phải của xe = **trái ảnh**.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai: Dòng D06.png / face / eye_left — peer ghi open, mình ghi closed. Nhìn lại ảnh D06, vùng mắt trái thực sự không rõ ràng do ánh sáng bên cửa sổ; "closed" của mình là đoán không có căn cứ. Mình học được: khi không chắc chắn 80%+, luôn ghi unknown thay vì chọn trạng thái cụ thể.
