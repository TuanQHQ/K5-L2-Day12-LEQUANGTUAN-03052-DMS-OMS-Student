# Danh sách lớp v0 — Le Quang Tuan · G06

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_left / eye_right | open / closed / unknown | Phát hiện buồn ngủ (mắt nhắm lâu) và mất tập trung | Có, trừ khi kính phản xạ LED hoặc kính râm |
| 2 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Đo thời gian tài xế rời mắt khỏi đường, nhận biết nhìn gương | Có (hướng đầu và mắt), kém khi mắt bị che |
| 3 | glasses | none / clear / sun / unknown | Chọn thuật toán đo mắt phù hợp, biết khi nào mắt có thể bị che | Một phần: kính râm có thể trông trong suốt trên NIR |
| 4 | face13 (13 điểm mặt) | tọa độ + cờ occluded/outside | Tính độ mở mắt, tư thế đầu, ngáp; biết điểm nào không tin được | Có, điểm bị che phải có cờ |
| 5 | hand (whose, side, on_wheel) | driver/passenger · left/right · yes/no/unknown | Biết tay có trên vô lăng không, cảnh báo khi lái thủ công mà rời tay | Có (hình dạng), kém khi tay nằm ngoài khung |
| 6 | phone | held / mounted / lap | Phát hiện dùng điện thoại khi lái | Có nếu thấy cạnh, góc hoặc màn hình; vật tối cầm gần tai thì không chắc |
| 7 | seatbelt (trên person) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây, phát hiện thắt sai | Không chắc: dây trên áo tối dễ chìm trong bóng tối, khi đó ghi unknown |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person + seat | rear_left / rear_middle / rear_right | Đếm số người và biết ngồi ghế nào để nhắc dây an toàn, chỉnh túi khí | Có (hình dạng, vị trí) |
| 2 | face (OMS, chỉ box) | box | Xác nhận một vật thể là người lớn thật, không phải đồ vật | Có nếu mặt đủ lớn; mặt nhỏ thì mắt và kính là unknown |
| 3 | child_seat | occupied: no / unknown, theo ghế | Biết ghế trẻ em có được lắp, nhắc kiểm tra | Có (hình dạng) |
| 4 | infant_seat | facing: rear / front, theo ghế | Nhắc quên trẻ trong xe; tắt túi khí ghế trước khi có ghế em bé quay ngược | Có (hình dạng) |
| 5 | object | type: bag / box / cup / other, theo ghế | Nhắc quên đồ, tránh nhầm đồ vật với người | Có (hình dạng), không dựa vào màu |
| 6 | seatbelt (trên person) | not_worn / unknown | Nhắc thắt dây cho hành khách sau | Không chắc khi dây chìm trong bóng tối |
| 7 | frame_meta | modality · light · camera_blocked | Biết loại ảnh, ánh sáng, camera có bị che, để chọn model và lọc dữ liệu | Có |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Guideline DMS không vẽ `object` cho vật cầm trên tay (chai, cốc), nhưng hành vi uống nước hoặc cầm vật là một dạng phân tâm cần model biết. Có nên thêm lớp vật cầm cho DMS không, hay chủ ý bỏ qua?
   Khung ví dụ: D03 (tay cầm một vật, không phải điện thoại).
   Bạn tạm gán thế nào và vì sao: Tạm vẽ một box `object` với `type = other`, `seat = driver` để giữ lại thông tin vật trên tay, và ghi vào disagreement_log là chỗ guideline không rõ. Em hiểu đây là chỗ khác với §4.2 và chờ coach quyết định có giữ hay xóa.
2. Câu hỏi: `occupied` của `child_seat` chỉ có `no` / `unknown`, không có `yes`. Khi có vật hoặc người ngồi trong ghế trẻ em thì nên ghi `occupied` là gì, khi vật trong ghế đã có box `object` riêng?
   Khung ví dụ: S01_rgb và S08_rgb (ghế trẻ em có thứ ngồi bên trong).
   Bạn tạm gán thế nào và vì sao: Tạm ghi `occupied = unknown` (vì `yes` không hợp lệ), vẽ box `child_seat` riêng, và nếu thấy rõ là đồ vật thì thêm box `object` cùng `seat`. Nếu thấy hình giống trẻ em thì báo coach, không gán.
