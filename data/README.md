# Cabin practice data — separate from model data

Thư mục này là **bộ luyện cabin 10 ảnh**: dùng để tập visibility, privacy và thao tác CVAT.
Nó tách hẳn khỏi `dataset/`, là tập dữ liệu Task A dùng để chuyển thành nhãn train và đánh giá model.

- `images/`: 2 consented full-body calibration crops + 8 privacy-reduced cabin derivatives, from 10 people and 3 sources; no AI-generated media.
- `image-manifest.csv`: filename, participant, source, licence, processing, dimensions and output SHA-256 contract.
- `source-selection.json`: nguồn, integrity reference, crop/mask và stressor của từng ảnh.
- `GENERATION_RECORD.md`: hồ sơ chọn và xử lý dữ liệu.
- `schema/coco17-keypoints.json`: canonical names, flip map and COCO 1-indexed edges.
- `schema/coco17-cvat-skeleton.svg`: uploadable CVAT skeleton template.

Không thêm archive/video nguồn thô, annotation tham chiếu hay bài nộp. Trước khi dùng bộ luyện cabin, chạy `python3 scripts/audit-data-pack.py` từ thư mục gốc và đọc `DATA_GOVERNANCE.md`.
