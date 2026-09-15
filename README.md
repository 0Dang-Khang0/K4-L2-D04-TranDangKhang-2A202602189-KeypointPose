# Ngày 4 — Keypoint & Pose: khối cabin

**Thời lượng:** ~60 phút, nằm trong chặng gán nhãn (phút 40-130) của buổi lab 240 phút.
**Repo đề bài chính:** `Day4-TrackData-Keypoint-Pose` — repo này **không thay thế** nó.

Repo đề bài dạy bạn gán 17 điểm COCO trên ảnh thường. Khối cabin hỏi thêm một câu mà bộ ảnh
đó không hỏi được: **khi khuôn mặt bị privacy mask, bạn xử lý 5 điểm mặt thế nào?**

10 ảnh trong `data/images/` gồm 10 người từ ba bộ dữ liệu công khai — 2 ảnh calibration toàn
thân và 8 frame trong cabin xe. Bạn gán `person` 17 điểm cho từng ảnh, tự kiểm ba lượt, export
COCO Keypoints 1.0, rồi chạy validator.

## Luật quan trọng nhất — đọc trước khi đặt chấm đầu tiên

Trên **8 ảnh cabin**, năm điểm `nose` / `left_eye` / `right_eye` / `left_ear` / `right_ear`
bắt buộc để `Outside` (`v=0`) — **không** đặt chấm vào giữa vùng mask.

Luật này **ngược với luật của repo đề bài** (bị che mà còn trong khung thì `v=1` và vẫn đặt
chấm). Đây là mâu thuẫn có chủ ý: mask đã xoá bằng chứng, nên không có gì để ước lượng. Hệ quả
thực tế:

> Nhãn của khối cabin **không bao giờ** được trộn vào tập train của repo đề bài.

Trên **2 ảnh calibration**, làm đúng luật gốc: đủ 17 điểm theo evidence, gồm cả 5 điểm mặt.

Chi tiết đầy đủ: [GUIDE.md](GUIDE.md) mục 0 và 0b.

## Bạn cần nộp gì

Ba file, xem [SUBMISSION.md](SUBMISSION.md):

```text
COCO_KEYPOINTS_EXPORT.zip     export CVAT, COCO Keypoints 1.0, không sửa tay
VISIBILITY_REPORT.csv         sinh bởi validator
POSE_REVIEW.md                bản điền của reports/POSE_REVIEW_TEMPLATE.md
```

## Bắt đầu

```bash
python3 tools/validate-submission.py --export COCO_KEYPOINTS_EXPORT.zip --write-report VISIBILITY_REPORT.csv
```

1. Dựng task CVAT theo [CVAT_SETUP.md](CVAT_SETUP.md).
2. Gán nhãn theo [GUIDE.md](GUIDE.md), chặng 1 → 6.
3. Tự kiểm ba lượt **trước khi** mở model diagnostic. Model là công cụ chẩn đoán, không phải đáp án.
4. Export, chạy validator, điền review, nộp.

## Dữ liệu — ràng buộc bắt buộc

Pack 10 ảnh này **phi thương mại**, và phải giữ nguyên attribution + ghi chú thay đổi khi chia
sẻ lại. Nguồn, license và thay đổi của từng ảnh ghi trong
[data/image-manifest.csv](data/image-manifest.csv) và [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
Đây là người thật đã đồng ý cho dùng dữ liệu — đối xử với ảnh đúng như vậy.

## Chấm điểm

Thang 100 điểm của riêng khối cabin: [RUBRIC.md](RUBRIC.md). Đọc trước khi bắt đầu — cổng qua
bài và ba cái bẫy mất điểm nặng nhất đều nằm ở đó.

Quy định làm bài và liêm chính học thuật: [RULES.md](RULES.md).
