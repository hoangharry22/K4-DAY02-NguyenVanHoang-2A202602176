# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Văn Hoàng<br>
**MSSV:** 2A202602176<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: "drive_022", "drive_033", "drive_038", "drive_008"
- Số vật thể thực tế: 60
- Mã SHA-256 của gói YOLO của bạn: 9e4a37e71c5ec2924b7116ba616450cc8aa5dc3e1b9cc9848371568f33b777ce
- Mã SHA-256 của gói CVAT gốc của bạn: 0a7c103ee70fc193ff859983fcd42759694fbfd0768870a79c72dfbe9aeaf184
- Nguồn đối chiếu: bộ tham chiếu do người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu: c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: Lần phát 1, nhận lúc 11:46 ngày 14/09/2026

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:
>Tôi đã hoàn thành gán nhãn toàn bộ 4 ảnh và xuất gói YOLO/CVAT trước khi nhận bộ tham chiếu. Trong quá trình gán nhãn tôi không trao đổi đáp án với ai, và chỉ dựa vào quy tắc phân lớp được cấp ở đầu buổi.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_008/object 12 | bus | xe dài, nhiều cửa sổ | xe dài, nhiều cửa sổ |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:
>Ở ảnh drive_008, lớp của vật thể là "bus", còn "inside" là thuộc tính mô tả vị trí của xe so với mép ảnh, không phải là một lớp riêng. Có chiếc xe bus bị cắt bởi mép ảnh vẫn cùng một lớp "bus".

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| 0 | - | - | 0 |

- Số hộp `needs_review` trước và sau khi kiểm: 0-0
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: -

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`:
>row: [0, 0.265656, 0.502437, 0.098219, 0.057281]
- Tên lớp và tọa độ điểm ảnh `xyxy`:
>lớp=0 (car) | tâm=(0.2657, 0.5024) | kích thước=(0.0982, 0.0573)
>pixel xyxy: [138.6, 303.2, 201.4, 339.9]
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
>Định dạng chỉ kiểm tra được rằng có đủ 5 giá trị số hợp lệ trong khoảng [0,1] - nó không biết nội dung ảnh thực tế. Người gán nhãn có thể vô tình chọn sai mã lớp, vẽ hộp không khớp

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện:
>"drive_022", "drive_033", "drive_038"
- Mã ảnh thẩm định:
>"drive_008"
- Mô tả một dự đoán trong `detect_result.jpg`:
>Model không phát hiện được vật thể nào trong ảnh thẩm định drive_008.
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
>Số lượng ảnh huấn luyện quá ít nên model chưa học đủ đặc trưng của lớp vật thể đó.
- Minh chứng nào có thể bác bỏ nhận định của bạn?
>Nếu huấn luyện lại model với nhiều ảnh hơn (ví dụ 20–30 ảnh thay vì 3) mà model vẫn không phát hiện được vật thể trong ảnh thẩm định, thì giả thuyết 'thiếu dữ liệu' bị bác bỏ. Khi đó nguyên nhân có thể nằm ở cấu hình huấn luyện.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
>4 ảnh là mẫu quá nhỏ để rút ra kết luận tổng quát về hiệu năng.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 37
- IoU trung bình và trung vị: 0.857421 - 0.877331
- Mức đồng thuận lớp: 0.648649
- Số hộp phía bạn không ghép được: 23
- Số hộp phía đối chiếu không ghép được: 13
- Một điểm khác biệt cụ thể:
>Ở ảnh drive_038, tôi gán một vật thể phía góc trên bên trái là 'bus', nhưng bộ tham chiếu không gán nhãn cho vật thể đó.
- Quy tắc hoặc hành động sửa phát sinh:
>Vật thể ở góc trên bên trái của drive_038 có kích thước tương đối lớn và không bị che khuất đáng kể, nên khả năng cao đây là trường hợp bộ tham chiếu bị bỏ sót.
>Tôi đề xuất quy tắc: khi phát hiện chênh lệch mà vật thể đủ rõ ràng, đủ lớn để nhận diện, cần kiểm tra lại bằng cách zoom vào đúng khu vực đó trên cả hai bộ nhãn trước khi kết luận, thay vì mặc định cho rằng nhãn của mình sai.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
>Cả hai người gán nhãn có thể cùng mắc một loại lỗi hệ thống giống nhau (ví dụ cùng hiểu sai một quy tắc mơ hồ, hoặc cùng bỏ sót một loại vật thể khó nhìn thấy), khi đó IoU và đồng thuận lớp vẫn cao dù cả hai đều sai so với thực tế. Đồng thuận chỉ đo độ nhất quán giữa hai người, không đo độ chính xác tuyệt đối.

## 7. Kiểm tra kho GitHub cá nhân

- [ ] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

>Minh chứng mạnh nhất trong bài là phân tích trường hợp cụ thể ở drive_038, cho thấy tôi có thể lý giải khác biệt bằng bằng chứng quan sát được (kích thước vật thể) thay vì mặc định nhãn của mình sai.
>Câu hỏi còn lại cho Lab Coach: mức đồng thuận lớp của tôi chỉ đạt 64.86% dù IoU trung bình khá cao (0.857). Điều này cho thấy vị trí hộp thường khớp nhưng việc gán tên lớp lại lệch nhiều; tôi nên ưu tiên xem lại ranh giới giữa những lớp nào để cải thiện phần này?
