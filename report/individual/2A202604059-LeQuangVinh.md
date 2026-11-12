# Báo cáo cá nhân — 2A202604059_LeQuangVinh

Đổi tên file theo quy tắc MSSV_HoTen.md. Ví dụ: 20241234_NguyenVanAn.md.
Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Lê Quang Vinh
- MSSV: 2A202604059
- Email: 26ai.vinhlq@vinuni.edu.vn
- Link repository: https://github.com/v1nhleee/K5-L2-2A202604059-LeQuangVinh-KeypointPose-Student
- Phiên bản CVAT (Help → About): v2.x.x
- Máy: hệ điều hành Windows 11 · RAM 16 GB · chạy CVAT chế độ đầy đủ
- Bộ ảnh (src/exports/assignment.json do lab.py setup tạo): có
- Giờ bắt đầu: 16:00
- Giờ kết thúc: 19:30

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
| :--- | :--- | :--- | :--- | :--- | :--- |
| person | skeleton | 17 | Có | Có | 19 |
| face | skeleton | 13 | Có | Có | 13 |

- Mẫu person dựng theo người quay mặt về camera (các điểm left_* nằm bên phần phải khung mẫu)? Có
- Cách đổi cờ occluded / outside cho một điểm trên phiên bản CVAT này (một câu): Bấm phím tắt `O` hoặc click vào biểu tượng switch outside property trên thanh công cụ của điểm khớp.

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| easy | 5 | 5 | 0 | 0 | 25 |
| back | 2 | 2 | 0 | 0 | 15 |
| small | 1 | 0 | 2 | 0 | 10 |
| crowd | 1 | 3 | 0 | 0 | 20 |
| faces | 8 | 9 | — | — | 35 |

- Số ảnh người đã xong ở mốc phút 105: 12 / 12
- Thời gian thực tế (phút): CP1 45 · CP2 40 · CP3 20
- Ảnh tốn thời gian nhất là ảnh nào, vì sao? Ảnh nhóm đông người (`crowd`) và ảnh có người bị che khuất một phần vì phải cân nhắc kỹ giữa việc điểm bị che khuất hoàn toàn (`v = 0`) hay còn ước lượng được vị trí (`v = 1`), đồng thời xác định chính xác trái/phải theo hướng của từng nhân vật.

## 3. Bảng ca khó (CP1, CP2)

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 000000163118.jpg | Người 1 | right_wrist | v = 1 | v = 1 và v = 2 | Cổ tay bị khuất một phần bởi thân người nhưng vẫn nhìn thấy mép và định hình được khớp nối. |
| face_342273.jpg | Mặt 2 | right_eye_inner | v = 0 | v = 0 và v = 2 | Điểm nằm sát mép và tràn ra ngoài biên khung hình bức ảnh, theo quy định bắt buộc phải chuyển thành outside. |
| 000000240340.jpg | Người 1 | left_ankle | v = 1 | v = 1 và v = 0 | Chân bị che khuất sau vật cản nhưng dựa vào ống quần có thể ước lượng được vị trí tương đối. |

## 4. Bảng tự review (CP3)

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| face_342273.jpg | Mặt 2 | right_eye_inner | position | Điểm nằm ngoài biên ảnh để nhầm v = 2 | Có |
| face_342273.jpg | Mặt 2 | face (skeleton #2) | extra | Thừa skeleton khuôn mặt trong ảnh | Có |
| 000000323705.jpg | Người | toàn thân | missing | Gán nhầm cho người < 60px | Có |
| 000000163118.jpg | Người 1 | ankles | ok | Kiểm tra đúng vị trí trái/phải | Có |
| 000000240340.jpg | Người 1 | ankles | ok | Khớp nối chính xác | Có |
| 000000027565.jpg | Người 1 | skeleton | ok | Hoàn thiện chuẩn | Có |
| 000000072098.jpg | Người 1 | skeleton | ok | Hoàn thiện chuẩn | Có |
| face_10579.jpg | Mặt 1 | skeleton | ok | Đủ 13 điểm | Có |

- Tổng hợp số lỗi theo mã:
  - `lr_swap`: 0
  - `vis_flag`: 0
  - `position`: 1
  - `missing`: 0
  - `extra`: 1
  - `ok`: 6
- Điểm còn phân vân giữa v = 1 và v = 0 (ghi cả hai lựa chọn và lý do chọn): Điểm khớp cổ tay bị che khuất một phần nhỏ phía sau đạo cụ (`v = 1` vì vẫn đoán được vị trí dựa vào cẳng tay, không chọn `v = 0` vì chưa mất hoàn toàn khỏi tầm nhìn ước lượng).

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

- Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử keypoints | Điểm outside ra v = 0, occluded ra v = 1? | num_keypoints khớp? |
| :--- | :--- | :--- | :--- | :--- |
| LeQuangVinh_persons.zip | 000000027565.jpg (#1) | 51 | Có | Có |
| LeQuangVinh_persons.zip | 000000072098.jpg (#1) | 51 | Có | Có |
| LeQuangVinh_faces.zip | face_10579.jpg (#1) | 39 | Có | Có |

- Sau khi xuất, CVAT ghi occluded thành `v = 1` và outside thành `v = 0`. Tọa độ của điểm outside trong file là tọa độ cũ (CVAT 2.x xuất như vậy, chuẩn COCO ghi 0, 0)[cite: 9].
- Hai job đã chuyển sang completed trên CVAT? Có
- Dán phần TÓM TẮT in ra từ src/check_submission.py:
```text
=== C:\Users\Nitro\K5-L2-2A202604059-LeQuangVinh-KeypointPose-Student\src\exports\LeQuangVinh_persons.zip (label `person`) ===
=== C:\Users\Nitro\K5-L2-2A202604059-LeQuangVinh-KeypointPose-Student\src\exports\LeQuangVinh_faces.zip (label `face`) ===
- File person: LeQuangVinh_persons.zip
  Số ảnh: 12 · số skeleton `person`: 17 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000027565=1, 000000072098=1, 000000090646=1, 000000147780=2, 000000163118=2, 000000240340=1, 000000261732=1, 000000294329=1, 000000297227=2, 000000323705=0, 000000413160=3, 000000467300=2
  Cờ v: v=2 207 (72%) · v=1 21 (7%) · v=0 61 (21%)
  num_keypoints trung bình: 13.4
- File face: LeQuangVinh_faces.zip
  Số ảnh: 8 · số skeleton `face`: 9 · mỗi annotation 39 số
  Skeleton theo ảnh: face_10579=1, face_137371=1, face_261458=1, face_309728=1, face_342273=2, face_359139=1, face_560282=1, face_79591=1
  Cờ v: v=2 98 (84%) · v=1 10 (9%) · v=0 9 (8%)
  num_keypoints trung bình: 12.0
- Ghi chú: 70 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0). Không phải lỗi: điểm v = 0 bị bỏ qua khi huấn luyện và khi chấm.
- Số [LỖI]: 0 · số [CẢNH BÁO]: 4
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)

## 6. Ảnh chụp màn hình

- **Label person trong label constructor (đủ 17 điểm có tên):**
  <img width="332" height="562" alt="image" src="https://github.com/user-attachments/assets/e305cc58-0423-4c8d-8edf-76d6539bf0f9" />

- **Một người quay lưng đã gán, cờ đã đặt:**
 <img width="216" height="532" alt="image" src="https://github.com/user-attachments/assets/27cd9541-e2b3-4096-a267-b461ca2dbdf0" />

- **Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:**
  ![Uploading image.png…]()


- **Một đoạn JSON xuất của một người:**


*Nguồn ảnh: COCO val2017 / Wikimedia Commons.*

## 7. Tự đánh giá

- **Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:** Nhầm lẫn cờ `v = 2` thành `v = 0` đối với các điểm nằm sát biên ảnh hoặc khuất hẳn. Cách tránh là luôn kiểm tra kỹ tọa độ biên và bật chế độ tự động check script trước khi export chính thức.
- **Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):** Quy định rõ ràng về ngưỡng kích thước cắt cúp đối với các trường hợp người bị che khuất một phần nửa người trên/dưới.
- **Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):** Lỗi để nhầm điểm ngoài ảnh có cờ `v = 2` thay vì `v = 0` sẽ làm sai lệch định tâm heatmap của các điểm mốc (keypoints), khiến mô hình đưa trọng số cao vào các giá trị tọa độ nhiễu nằm ngoài thực tế ảnh trong hàm loss.

## 8. Khó khăn và lỗi kỹ thuật

- Đôi khi thao tác chọn nhầm điểm `left`/`right` khi nhân vật quay lưng ở góc nghiêng lớn, cần cẩn thận xác định theo hướng cơ thể nhân vật thay vì hướng nhìn của màn hình.
