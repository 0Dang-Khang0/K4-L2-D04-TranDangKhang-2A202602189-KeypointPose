# Quy định làm bài

## Thứ tự bắt buộc

**Tự gán nhãn và tự kiểm ba lượt trước.** Chỉ được mở model diagnostic sau khi đã khoá nhãn của
mình. Chạy model trước rồi gán theo output của nó là gian lận, không phải làm nhanh — và lịch sử
commit cho thấy điều đó.

Model sai thường xuyên trên ảnh cabin: nó đoán khớp chân ở mép ảnh ngay cả khi không nhìn thấy
cổ chân. Bất đồng giữa bạn và model chỉ tạo **câu hỏi**, không tạo đáp án.

## Không được làm

- Sửa file JSON trong ZIP export bằng tay.
- Truy ngược bộ dữ liệu nguồn để lấy nhãn có sẵn.
- Nộp nhãn của người khác dưới tên mình.
- Trộn nhãn của khối cabin vào tập train của repo đề bài (xem lý do ở [README.md](README.md)).

## Được làm

- Thảo luận luật gán nhãn với bạn cùng lớp.
- Nhờ hỗ trợ thao tác CVAT.
- Review chéo — nhưng mỗi người tự tạo annotation và evidence của mình.
- **Rework sau khi đọc báo cáo lỗi.** Vòng sửa nhãn là phần được dạy, không phải phần bị phạt.

## Dữ liệu

10 ảnh trong `data/images/` là người thật đã đồng ý cho sử dụng dữ liệu, trong đó 8 ảnh đã được
che mặt để giảm nhận dạng. Ràng buộc:

- **Chỉ dùng cho lớp học, phi thương mại.**
- Không đăng lại ảnh ra ngoài phạm vi lớp.
- Không cố khôi phục khuôn mặt bị mask, không cố định danh người trong ảnh.
- Chia sẻ lại trong phạm vi cho phép thì phải giữ nguyên attribution và ghi chú thay đổi
  ([THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)).

Vi phạm phần dữ liệu nặng hơn vi phạm phần kỹ thuật — nó ảnh hưởng tới người thật, không chỉ
tới điểm số.
