# Tự đánh giá — Le Quang Tuan · G06

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| D04 | face (eye_left, eye_right) | `flag` | Điểm mắt face13 để hiện, nhưng thuộc tính eye của face ghi `unknown`: hai chỗ không khớp. Mắt không thấy nên bật `occluded` cho các điểm mắt, giữ eye = `unknown` | Đã sửa |
| D03 | hand (side) | `lr_flip` | Gán cả hai tay đều là `right`. Một người chỉ có một tay phải và một tay trái; sửa lại tay còn lại thành `left` theo người | Đã sửa |
| S01 | child_seat | `wrong_class` | Nhầm `child_seat` thành `infant_seat`. Sửa lại đúng lớp theo hình dạng ghế | Đã sửa |
| S03 | object (type) | `missing` | Quên chọn `type` (cup, bag, ...) cho box `object`. Bổ sung type theo hình dạng vật | Đã sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Thuộc tính của box bị bỏ sót hoặc không khớp với phần còn lại của đối tượng (D04 eye `unknown` nhưng điểm mắt không có cờ, D03 `side` hai tay trùng nhau, S03 `object` thiếu `type`, D02 `seatbelt` ghi đoán thay vì `unknown`).

Vì sao mình mắc: Vẽ box xong là chuyển sang hình tiếp theo, không mở **DETAILS** để kiểm lại từng thuộc tính. CVAT điền sẵn giá trị đầu tiên của danh sách cho thuộc tính mới, nên dễ để nguyên giá trị mặc định. Một số chỗ mình đoán thay vì ghi `unknown` khi không thấy rõ.

Lần sau tránh bằng cách: Vẽ xong mỗi đối tượng thì mở DETAILS, đọc lại từng thuộc tính một lượt. Cuối mỗi ảnh đối chiếu các cặp liên quan (eye với cờ điểm mắt, hai tay với `side`). Không thấy rõ thì ghi `unknown`. Trước khi export chạy `cabin_check` để bắt lỗi còn sót.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai: D02 `seatbelt` (mình ghi `worn_incorrect`, bạn ghi `unknown`). Mình học được: dây an toàn không thấy rõ thì phải ghi `unknown` theo §4.7, không được đoán là đeo sai. Hai dòng `gaze_zone` ở D03 và D06 và box `phone` ở D05 cũng cho thấy mình thiên về `other` hoặc bỏ sót khi chưa chắc, trong khi bạn đã đọc kỹ hướng nhìn và hình dạng vật. Mình đã sửa các dòng này theo guideline rồi export lại.
