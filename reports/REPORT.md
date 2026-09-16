# BÁO CÁO KẾT QUẢ FINE-TUNE MODEL POSE (DAY 4)

## 1. Phân tích sự thay đổi của pose_mAP50-95 sau fine-tune
- **Chênh lệch:** mAP50-95 của Pose thay đổi từ **0.6853** lên **0.6908** (tăng **+0.0055**).
- **Giải thích:** 
  - Mặc dù lượng dữ liệu huấn luyện rất nhỏ (chỉ 20 ảnh), model vẫn giữ vững phong độ và cải thiện nhẹ về mặt độ chính xác trung bình (mAP50-95 tăng khoảng 0.55%). Điều này chứng tỏ tập nhãn của chúng ta có chất lượng tiệm cận chuẩn và không làm hỏng cấu trúc phân phối vốn có của COCO.
  - Tuy nhiên, 20 ảnh là quá ít để tạo ra bước nhảy vọt lớn. Vòng lặp dữ liệu này đóng vai trò kiểm thử quy trình (pipeline) hoạt động chính xác trước khi scale-up quy mô dữ liệu.

## 2. So sánh độ khó giữa tìm Người (Box) và tìm Khớp (Pose)
- **Kết quả mAP50-95:**
  - Box mAP50-95 (Sau FT): **0.8041**
  - Pose mAP50-95 (Sau FT): **0.6908**
- **Nhận xét:** Model tìm **Người (Box) dễ hơn nhiều** so với tìm **Khớp (Pose)** (chênh lệch khoảng 11.33%). Điều này là hoàn toàn hợp lý vì chiếc hộp bao quanh cơ thể là một cấu trúc thô, dễ nhận diện dựa trên biên dạng tổng thể, trong khi các khớp đòi hỏi độ chính xác cục bộ cao và rất dễ bị che khuất hoặc nhầm lẫn góc xoay.

## 3. Nhận diện lỗi của model trên tập test (Mục 5)
- Trực quan hóa 10 ảnh test cho thấy model dự đoán tốt ở các tư thế đứng thẳng thông thường.
- **Lỗi điển hình:** Ở các ảnh có người bị che khuất một phần hoặc có tư thế gập người sâu (như `test_02`, `test_06` với nhiều người chồng chéo), model có xu hướng gặp lỗi **Lệch nhẹ (Jitter)** ở các khớp xa như cổ chân/cổ tay, hoặc đôi khi **Nhầm người (Mismatched)** khi khớp của người này bị gán sang khung của người kia do khoảng cách quá gần.

## 4. Đánh giá sự bất đồng giữa nhãn tự gán và dự đoán của model (Mục 6)
- **Ảnh có OKS thấp nhất:** `train_13` (OKS lần lượt là 0.645 và 0.814) và `train_06` (OKS: 0.648).
- **Ai đúng?**
  - Qua kiểm tra lỗi cảnh báo từ công cụ kiểm nhãn ở Mục 1, chúng ta thấy nhãn tự gán bị cảnh báo lạm dụng cờ `v=0` (như ở `train_13` có 6 khớp v=0 dù người nằm gọn giữa ảnh). Do đó, **model dự đoán đúng hơn** về mặt logic phân bổ khớp bị che khuất (occluded - đáng ra phải là `v=1` kèm điểm ước lượng thay vì bỏ qua đặt `v=0`). Nhãn thủ công cần được điều chỉnh lại theo đúng Guideline Mini.

## 5. Mối liên hệ với kết quả chấm chéo (Gold Label)
- Các ảnh có OKS nhãn-vs-model thấp nhất trùng khớp với các ảnh có OKS nhãn-vs-gold thấp nhất (đặc biệt là các ca có tư thế khó hoặc bị che khuất nhiều).
- **Kết luận:** Điều này chỉ ra rằng những ảnh có độ phức tạp cao (bối cảnh rối, tư thế lạ) là thách thức chung cho cả người gán nhãn lẫn mô hình học máy. Việc cải thiện chất lượng nhãn ở những ca khó này chính là chìa khóa để nâng cao hiệu năng của mô hình.
