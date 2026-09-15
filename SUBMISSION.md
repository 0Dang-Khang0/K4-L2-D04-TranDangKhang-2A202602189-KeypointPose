# Nộp bài — khối cabin

Ba file, đặt trong thư mục `submission/`:

| File | Sinh ra từ đâu | Kiểm bằng |
| --- | --- | --- |
| `COCO_KEYPOINTS_EXPORT.zip` | CVAT → Export task dataset → **COCO Keypoints 1.0** | validator |
| `VISIBILITY_REPORT.csv` | `tools/validate-submission.py --write-report` | validator |
| `POSE_REVIEW.md` | bản điền của `reports/POSE_REVIEW_TEMPLATE.md` | người chấm đọc |

## Chạy trước khi nộp

```bash
python3 tools/validate-submission.py --export submission/COCO_KEYPOINTS_EXPORT.zip --write-report submission/VISIBILITY_REPORT.csv
```

```bash
python3 tools/validate-submission.py --submission-dir submission
```

Lệnh thứ hai phải in `PASS` thì mới nộp.

## Contract mà validator kiểm

- Đúng một file COCO annotation JSON trong ZIP; **không** đổi tên file bên trong, **không** sửa JSON bằng tay.
- 10 image record, basename khớp `data/image-manifest.csv`.
- Đúng **một** annotation `person` trên mỗi ảnh.
- Mỗi `keypoints` có **51 số**; `num_keypoints == count(v > 0)`.
- Category có đúng 17 keypoint và 19 skeleton edge.
- Toạ độ của điểm `v > 0` phải nằm trong khung ảnh; `bbox` phải có width/height dương.

## Điều validator **không** kiểm

`PASS structural audit` chỉ nói định dạng đúng. Nó **không** chứng minh:

- chấm đúng vị trí giải phẫu;
- không nhầm trái/phải;
- quyết định `v=0` / `v=1` / `v=2` đúng ngữ cảnh.

Ba thứ đó là phần bạn chịu trách nhiệm, và là phần được đọc khi chấm — chúng chiếm 55/100 điểm
trong [RUBRIC.md](RUBRIC.md). Một bài PASS validator mà đảo trái/phải vẫn là bài sai.

## Định dạng export — chỗ mất điểm phổ biến nhất

Export **COCO 1.0** (không có chữ "Keypoints") chỉ ra bounding box, mất sạch 17 điểm. Export
**YOLO 1.1** cũng vậy. Kiểm lại tên định dạng trên menu export trước khi bấm.
