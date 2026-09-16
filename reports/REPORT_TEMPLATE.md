# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: ______   Nhóm: ______   Ngày: ______

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | |
| Số skeleton | |
| v=2 / v=1 / v=0 | |
| Thời gian trung bình mỗi ảnh | |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1.
2.
3.

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | | |
| OKS@0.50 | | |
| OKS@0.75 | | |
| Lỗi `dao_trai_phai` | | |
| Lỗi `nham_nguoi` | | |
| Lỗi `xoa_khop_bi_che` | | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

-
-
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

-

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 5. Bài luyện cabin (tùy chọn)

Nếu bạn làm bộ luyện cabin: vì sao một khuôn mặt đã bị privacy mask không còn đủ bằng chứng
để gán toạ độ facial keypoint? Viết 3-5 câu, và nêu vì sao bộ này không được trộn vào train/test
của route chính.
