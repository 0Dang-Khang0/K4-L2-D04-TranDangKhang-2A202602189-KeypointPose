# annotations/ - bản export gốc từ CVAT

Đặt **nguyên xi** file CVAT xuất ra vào đây. Đừng sửa tay, đừng đổi tên trường.
Đây là bằng chứng bạn đã export đúng định dạng — và là thứ được mở ra khi
điểm số của bạn trông lạ.

```text
annotations/
  coco_keypoints/     Export -> COCO Keypoints 1.0   (bộ 17 điểm thân, 20 ảnh)
  face_hand/          Export -> COCO Keypoints 1.0   (bộ 21 điểm tay + 5 điểm mặt, 5 ảnh)
```

## Bộ 17 điểm thân

1. Trong CVAT: **Export -> COCO Keypoints 1.0**.
2. Giải nén, tìm file `.json` (thường là `annotations/person_keypoints_default.json`).
3. Chép vào `annotations/coco_keypoints/`.
4. Chuyển sang nhãn để train:

```bash
python3 tools/coco_kp_to_yolo_pose.py \
    --coco annotations/coco_keypoints/person_keypoints_default.json \
    --out dataset/labels/train
```

**Phép đếm 30 giây** (slide 40) — mở file `.json` ra và đếm:

- Mảng `keypoints` của **mỗi người** phải có đúng **51** số = 17 × (x, y, v).
- Mỗi dòng trong `dataset/labels/train/*.txt` phải có đúng **56** số = 5 (box) + 51.
- Đếm ra 34 hoặc 39 → **export lại**, đừng gán lại nhãn.

Nếu file không có mảng `keypoints` nào: bạn đã chọn nhầm **COCO 1.0** hoặc **YOLO 1.1**.
Hai định dạng đó chỉ xuất box — 17 điểm biến mất, không lỗi, không cảnh báo.

## Bộ face/hand

Đây là **một skeleton label riêng, một task riêng**, không trộn vào bộ 17 điểm:

- 21 điểm một bàn tay (MediaPipe / COCO-WholeBody)
- 5 điểm mặt rút gọn: mắt trái, mắt phải, mũi, khoé miệng trái, khoé miệng phải

Export riêng, chép vào `annotations/face_hand/`. Bộ này không chấm bằng OKS (không có
gold), nhưng nằm trong reviewer checklist và rubric. `coco_kp_to_yolo_pose.py` chỉ xử lý
bộ 17 điểm — chạy nó trên bộ face/hand sẽ báo lỗi số điểm, và đó là hành vi đúng.
