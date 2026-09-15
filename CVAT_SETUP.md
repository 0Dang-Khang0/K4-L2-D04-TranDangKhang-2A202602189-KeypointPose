# Dựng task CVAT — khối cabin

## Trước khi bấm Submit

- Task type: **Images**, **Shape mode**. Track mode làm export sai schema.
- **Skeleton không sửa được sau khi task đã tạo.** Preflight xong mới Submit — sai một sublabel
  là phải tạo lại task từ đầu.

## Task

- Images: đủ **10 file JPEG** trong `data/images/`, đúng byte và đúng thứ tự manifest.
- Label: **chỉ `person`** — skeleton 17 sublabel theo `data/schema/coco17-keypoints.json`, 19 cạnh.
- **Không** thêm `hand` / `face`, không thêm action attribute hay class khác.
- Một annotation `person` trên mỗi ảnh (vùng người lái).

## Dựng skeleton

Trong Skeleton Configurator, upload `data/schema/coco17-cvat-skeleton.svg`.

Ở tab Raw/Constructor, xác nhận **từng** `data-label-name` đúng tên và đúng thứ tự COCO-17. Nếu
CVAT của bạn không nhận file SVG, dựng thủ công từ `coco17-keypoints.json`, rồi download lại SVG
thực tế mà CVAT sinh ra và báo chênh lệch cho giảng viên.

## Visibility — ánh xạ bắt buộc

| Trạng thái điểm trong CVAT | Phím | COCO xuất ra |
| --- | --- | ---: |
| Visible | — | `v=2` |
| Occluded | `q` | `v=1` |
| Outside | `o` | `v=0` |

**Không dùng `Hidden` (`h`).** Nó chỉ đổi hiển thị, không phải trạng thái annotation bền vững:
điểm vẫn xuất ra `v=2` ở toạ độ cũ mà **không có cảnh báo nào**. Đây là cách âm thầm nhất để
hỏng cả bài.

## Save

Bấm thẳng biểu tượng **Save** trên toolbar. `Ctrl+S` không nhận nếu focus không nằm ở canvas —
bạn tưởng đã lưu mà chưa.

## Export

Task → Export task dataset → **COCO Keypoints 1.0** → Include images tuỳ chọn.

Không đổi tên file JSON bên trong ZIP, không mở JSON ra sửa tay. Validator sẽ bắt được, và sửa
tay là vi phạm quy định làm bài ([RULES.md](RULES.md)).
