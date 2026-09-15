# Rubric Ngày 4 — khối cabin (100 điểm)

Rubric này chấm **riêng khối cabin 10 ảnh** của repo này. Route chính của buổi lab 240 phút
dùng rubric trong repo đề bài `Day4-TrackData-Keypoint-Pose`. **Hai thang điểm không cộng vào
nhau** và không bù cho nhau.

Điều được chấm là **quyết định và bằng chứng**, không phải số giờ ngồi làm. Tám ảnh cabin
**không có đáp án gold**: mặt đã bị mask nên không ai — kể cả giảng viên — dựng lại được toạ độ
5 điểm mặt. Vì vậy khối này **không có điểm OKS**. Bạn được chấm ở chỗ bạn gọi đúng tên cái mình
không nhìn thấy, và ghi lại được vì sao.

## Bảng tiêu chí

| Tiêu chí | Bằng chứng | Điểm |
| --- | --- | ---: |
| Định dạng và tính hợp lệ | `tools/validate-submission.py` in `PASS structural audit`; export đúng **COCO Keypoints 1.0** (mỗi `keypoints` có 51 số), không phải COCO 1.0 hay YOLO 1.1 | 10 |
| Độ bao phủ | đủ 10 image record đúng tên manifest; đúng **một** annotation `person` mỗi ảnh; mọi skeleton đủ 17 điểm, không xoá bớt điểm nào | 10 |
| **Quyết định visibility trên vùng privacy mask** | trong `VISIBILITY_REPORT.csv`, cả năm dòng `nose` / `left_eye` / `right_eye` / `left_ear` / `right_ear` có `v0_outside_or_unlabeled` **đúng bằng 8** — 8 ảnh cabin `Outside`, 2 ảnh calibration có chấm thật | 25 |
| **Trái/phải theo cơ thể người trong ảnh** | không có `left_*` / `right_*` bị hoán đổi ở bất kỳ ảnh nào; cạnh skeleton không bắt chéo bất thường | 20 |
| Phân biệt `v=1` và `v=0` ngoài nhóm điểm mặt | khớp bị che nhưng **còn trong khung** có `Occluded` và **vẫn có chấm ước lượng**; `v=0` chỉ dùng cho khớp ra ngoài mép ảnh; không kéo điểm vào sát mép để "làm đủ" | 10 |
| Vị trí giải phẫu | 2 ảnh calibration chấm sát tâm khớp (gồm cả 5 điểm mặt); spot-check vai/hông/cổ tay trên ảnh cabin không lệch khỏi khớp | 10 |
| Cabin data-quality triage | `POSE_REVIEW.md` điền đủ 8 ảnh cabin: `Decision`, `Evidence limitation`, `Affected keypoints`, `Downstream action` — không còn `TODO` | 10 |
| Kiểm chéo và rework | bảng Peer review có ít nhất một finding thật, hoặc một row `no defect found after three passes` nêu evidence của cả ba lượt; mục Rework ghi đã đổi gì và đã re-export | 5 |

## Cổng bắt buộc

- **Export sai định dạng làm mất 17 điểm** (COCO 1.0 hoặc YOLO 1.1 — chỉ còn bounding box):
  tối đa 40 điểm. Bài hôm nay chính là 17 điểm đó.
- **Dùng `Hidden` (`h`) thay cho `Outside` (`o`)**: tối đa 40 điểm. Điểm vẫn xuất ra `v=2` ở
  toạ độ cũ mà không có cảnh báo nào — đây là cách hỏng dữ liệu âm thầm nhất, và validator
  cũng không bắt được.
- **Đặt chấm vào giữa vùng privacy mask** trên ảnh cabin thay vì `Outside`: tối đa 59 điểm.
  Đó là bịa toạ độ ở chỗ không còn bằng chứng, tức là làm sai đúng bài học của khối này.
- Không chạy validator, hoặc nộp file còn lỗi structural: tối đa 49 điểm.
- Thiếu `POSE_REVIEW.md`, hoặc nộp bản còn `TODO` / còn `SELF_QC_COMPLETE: no`: mất trọn
  15 điểm của hai mục cuối và tối đa 69 điểm.
- Thiếu `VISIBILITY_REPORT.csv`: trừ 10 điểm — nó là một trong ba deliverable.
- **Sửa JSON trong ZIP bằng tay**, hoặc đổi tên file bên trong ZIP: bài không được chấm.
- **Truy ngược ba bộ dữ liệu nguồn** để lấy nhãn có sẵn: bài không được chấm; giảng viên xem
  xét theo quy định học phần.
- **Chạy model diagnostic trước khi khoá nhãn** rồi sửa nhãn theo output của nó: coi như không
  có phần annotation. Thứ tự tự-gán-trước là bắt buộc.
- **Cố khôi phục khuôn mặt bị mask hoặc định danh người trong ảnh**: xử lý theo
  [RULES.md](RULES.md). Vi phạm phần dữ liệu nặng hơn mọi lỗi kỹ thuật trong bảng trên — nó
  ảnh hưởng tới người thật.

## Mức chất lượng

Không có gold cho ảnh cabin nên không có ngưỡng OKS. Ba cột dưới đây đọc trực tiếp từ
`VISIBILITY_REPORT.csv` và từ lượt review của giảng viên.

| Mức | Số facial keypoint có `v0 == 8` | Lỗi đảo trái/phải | Khớp `v=1` trong cả bài | Diễn giải |
| --- | ---: | ---: | ---: | --- |
| Xuất sắc | 5/5 | 0 | >= 1, và giải thích được từng ca trong review | gọi đúng cả chỗ thấy lẫn chỗ không thấy |
| Đạt | >= 4/5 | 0 | >= 1 | qua learning gate của khối cabin |
| Cần rework | <= 3/5 | >= 1 | 0 | đọc lại mục 0b của [GUIDE.md](GUIDE.md), sửa rồi export lại |

**Cổng qua bài:** validator `PASS` **và** 0 lỗi đảo trái/phải **và** cả 5 dòng facial keypoint
có `v0_outside_or_unlabeled == 8`.

Cột cuối là một cái bẫy có thật: **cả bài không có một khớp `v=1` nào** gần như luôn nghĩa là
bạn đã dùng `v=0` để né những khớp khó đọc, chứ không phải bạn gặp toàn khớp dễ. Trong cabin
xe, vai và hông bị ghế/vô-lăng che là chuyện bình thường — chúng vẫn ở trong khung, nên vẫn
phải có chấm ước lượng và cờ `Occluded`.

## Cách đọc điểm cho đúng

- **Rework không bị trừ điểm.** Vòng sửa nhãn sau khi đọc finding là phần được dạy, không phải
  phần bị phạt. Bài đi từ 3/5 lên 5/5 và giải thích được mình sửa gì thì tốt hơn bài 5/5 ngay
  từ đầu mà không nói được vì sao.
- **`PASS structural audit` không phải điểm cao.** Validator chỉ kiểm định dạng. Nó không biết
  bạn có đảo trái/phải, có chấm lệch khớp, hay có quyết định `v=0` đúng ngữ cảnh hay không —
  và đúng ba thứ đó chiếm 55/100 điểm.
- **Model sai không phải lỗi của bạn, và cũng không phải đáp án.** Trên ảnh cabin, model
  thường đoán cổ chân ở mép ảnh dù không nhìn thấy gì. Bất đồng giữa bạn và model chỉ tạo
  **câu hỏi** để bạn kiểm lại bằng chứng.
- **`needs-review` không phải điểm kém.** Gọi đúng một ảnh là `needs-review` kèm evidence
  limitation cụ thể được điểm cao hơn ép nó thành `usable` cho đẹp bảng. Nghề dữ liệu trả tiền
  cho việc biết dữ liệu nào không dùng được.
- **Triage là về chất lượng dữ liệu pose, không phải về tài xế.** Đừng suy luận trạng thái,
  hành vi hay sự chú ý của người trong ảnh — nó nằm ngoài phạm vi bài và ngoài phạm vi đồng ý
  của họ.
- **Lệch so với luật của repo đề bài trên 5 điểm mặt là đúng ở đây.** Đó là lý do nhãn khối
  cabin không bao giờ được trộn vào tập train của repo đề bài — xem [README.md](README.md).
