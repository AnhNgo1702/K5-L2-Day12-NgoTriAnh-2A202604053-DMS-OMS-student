# Danh sách lớp v0 — Ngô Trí Anh · G06

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_state | open / closed / unknown | Phát hiện buồn ngủ: mắt nhắm lâu (>2s) kích hoạt cảnh báo; nếu nhắm liên tục kích hoạt phanh khẩn | Có, trừ khi kính phản xạ LED hoặc góc nghiêng lớn |
| 2 | gaze_zone | road / mirror_c / mirror_l / mirror_r / lap / cluster / other | Phát hiện mất tập trung: lái không nhìn đường quá lâu kích hoạt cảnh báo âm thanh hoặc chấn rung vô lăng | Có (hướng mắt vẫn rõ trên IR); giảm chính xác khi ánh nắng bên |
| 3 | glasses | none / clear / sun / unknown | Điều chỉnh độ sáng LED chiếu lên mặt; kính râm giảm chất lượng eye_state → tăng ngưỡng cảnh báo | Có (khung kính rõ trên IR), nhưng kính phản xạ tạo đốm sáng |
| 4 | phone | yes / no / unknown | Phát hiện sử dụng điện thoại khi lái → cảnh báo ngay lập tức; kết hợp với gaze_zone để xác nhận | Có (hình dạng thiết bị), nhưng độ chắc chắn thấp hơn RGB |
| 5 | face_box | bounding box | Định vị khuôn mặt cho tất cả model DMS downstream (eye tracking, gaze, drowsiness) | Có (nhiệt độ da tạo tương phản tốt với nền xe) |
| 6 | face13_keypoints | 13 điểm: đuôi mắt, khóe mắt, mi mắt (trên/dưới)×2 mắt, mũi, 2 khóe miệng | Ước lượng head pose (yaw/pitch/roll) và eye openness chính xác hơn box đơn giản | Có, trừ điểm bị che hoặc mặt < 60px |
| 7 | on_wheel | yes / no / unknown | Phát hiện tay trên vô lăng → biết lái đang kiểm soát xe hay không (xe tự lái thường no) | Có (hình dạng bàn tay và vô lăng) |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person (hành khách) | bounding box + seat | Đếm số người trong xe → điều chỉnh túi khí phù hợp từng ghế; nhắc dây an toàn | Có (hình dạng người tạo tương phản nhiệt tốt) |
| 2 | infant_seat | bounding box + seat + occupied | Phát hiện ghế em bé → tắt túi khí ghế trước (trẻ < 1 tuổi nguy hiểm với túi khí); nhắc quên trẻ trong xe | Có (hình dạng ghế đặc trưng) |
| 3 | child_seat | bounding box + seat + occupied | Phát hiện ghế trẻ em → điều chỉnh lực căng dây an toàn; cảnh báo quên trẻ trong xe nóng | Có (hình dạng khung ghế) |
| 4 | seatbelt | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo chưa thắt dây an toàn theo từng ghế; phân biệt thắt sai (dưới nách) vẫn nguy hiểm | Hạn chế: dây mỏng, khó thấy trên IR-sim nền xám |
| 5 | seat (thuộc tính vị trí) | front_left / front_right / rear_left / rear_center / rear_right | Ánh xạ chiếm dụng ghế → kích hoạt đúng túi khí bên ghế đó; hệ thống OMS biết ghế nào có người | Không trực tiếp (cần biết từ vị trí box trong ảnh) |
| 6 | object | bounding box + seat + type (bag/box/cup/other) | Phát hiện vật nguy hiểm không cố định trong cabin → cảnh báo phanh gấp có thể văng vật; đồ vật trong ghế trẻ cần box riêng | Có (hình dạng vật cứng), khó với vải mềm |
| 7 | child_seat.occupied | yes / no / unknown | Phân biệt ghế trẻ trống hay có trẻ → tránh báo nhầm quên trẻ; không dùng "yes" (chỉ no/unknown) | Có nếu trẻ trong ghế tạo tương phản nhiệt |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Khi mặt người lái bị kính râm đậm che hoàn toàn vùng mắt (không thấy gì), `eye_state` nên ghi `unknown` hay bỏ hoàn toàn không gán thuộc tính? Guideline §4.6 nói `unknown` khi không thấy, nhưng §4.5 nói không gán điểm face13 khi mặt < 60px — có nguyên tắc tương tự cho thuộc tính bị kính che không?
   Khung ví dụ: D03.png (kính phản xạ LED che mắt phải)
   Bạn tạm gán thế nào và vì sao: Ghi `unknown` vì không thể quan sát — an toàn hơn là đoán; nhưng cần biết đây có phải trường hợp đặc biệt cần flag riêng không.

2. Câu hỏi: Khi camera OMS nhìn từ phía sau và một ghế vừa có người ngồi vừa có vật (ví dụ túi xách đặt trên đùi người), có cần vẽ box riêng cho vật đó không? Guideline §4.8 chỉ nói hai box cho "vật trong ghế trẻ em", nhưng không rõ người lớn + vật cùng ghế.
   Khung ví dụ: S03_rgb.png (người ngồi với vật trên đùi hoặc cạnh)
   Bạn tạm gán thế nào và vì sao: Chỉ vẽ box `person` cho người, không vẽ thêm vật — vì guideline §4.8 chỉ đặc tả trường hợp ghế trẻ em; cần xác nhận để không over-label.
