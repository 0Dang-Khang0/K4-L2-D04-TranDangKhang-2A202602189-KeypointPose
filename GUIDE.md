# GUIDE Ngày 4 - làm theo đúng thứ tự này

Bốn giờ, bảy chặng. Mốc thời gian là mốc thật, không phải gợi ý: chặng 5 chỉ mở
sau khi cả lớp đã khoá nhãn.

| Mốc | Chặng | Bạn làm gì |
| --- | --- | --- |
| 0:00-0:20 | 1 | Dựng skeleton label, tạo task CVAT |
| 0:20-0:40 | 2 | Warm-up: gán 2 ảnh, tự soi bằng `visualize_pose.py` |
| 0:40-2:10 | 3 | Gán 18 ảnh còn lại + bộ face/hand cho 5 ảnh |
| 2:10-2:30 | 4 | Ba lượt kiểm, visibility report, kiểm chéo, **khoá nhãn** |
| 2:30-3:10 | 5 | Nhận gold, chấm bằng OKS, rework |
| 3:10-3:50 | 6 | Colab: fine-tune, visualize, đánh giá |
| 3:50-4:00 | 7 | Báo cáo, commit, push |

---

## Chặng 1 - Dựng skeleton label (0:00-0:20)

**Làm một lần, dùng cho cả hai task.** Topology bị khoá ngay khi task được tạo:
đặt thiếu một điểm hay sai thứ tự là làm lại cả task từ đầu.

**Đừng đặt tay 17 điểm.** Gõ tay 17 cái tên là 17 cơ hội gõ sai, và sai một chữ
trong `left_wrist` thì `coco_kp_to_yolo_pose.py` báo lỗi, bạn phải export lại.
Có ba đường, chọn đường nào có sẵn trên CVAT bạn đang dùng:

**Cách A — dán JSON (nhanh nhất, chạy ở mọi bản CVAT).** Gói tài sản lớp học có file
`labels_day4.json`.

1. Tạo **Project** mới → ở khung Labels, mở tab **Raw**.
2. Dán toàn bộ nội dung `labels_day4.json` vào, bấm **Done**.
3. Xong: có luôn cả ba skeleton `person` (17 điểm), `hand` (21), `face` (5),
   đúng tên đúng thứ tự COCO.
4. Tạo **Task** bên trong project đó và upload 20 ảnh `dataset/images/train/`.

**Cách B — upload file `.SVG`** (đúng cách slide 29 mô tả). Gói tài sản lớp học có
`skeleton_person_17.svg`.

1. Ở khung Labels, bấm **Setup skeleton**.
2. Trong màn hình đó, bấm nút **Upload a skeleton from SVG** (icon mũi tên lên,
   góc trên bên phải khung vẽ) và chọn file `.svg`.
3. Đặt tên label là `person`, bấm **Continue**/**Done**.
4. Lặp lại cho `skeleton_hand_21.svg` và `skeleton_face_5.svg` nếu cần bộ face/hand.

**Cách C — `From model -> Human pose estimation`.** CVAT dựng sẵn 17 điểm đúng
chuẩn. **Chỉ có trên app.cvat.ai** hoặc bản self-hosted đã cài serverless (Nuclio);
trên CVAT cài trần thì danh sách model rỗng và nút này không dùng được.

Dù đi đường nào, sau khi xong hãy vào **Setup skeleton → Download skeleton as SVG**
và cất file lại. Mọi task sau chỉ việc Upload lại — kể cả task buổi tới.

Thứ tự đúng, từ đầu xuống chân, trái trước phải sau:

```text
0  nose
1  left_eye        2  right_eye
3  left_ear        4  right_ear
5  left_shoulder   6  right_shoulder
7  left_elbow      8  right_elbow
9  left_wrist     10  right_wrist
11 left_hip       12  right_hip
13 left_knee      14  right_knee
15 left_ankle     16  right_ankle
```

---

## Chặng 2 - Warm-up hai ảnh (0:20-0:40)

Gán `train_01` và `train_02` thôi, rồi dừng lại kiểm ngay. Bắt lỗi thao tác ở phút 30
rẻ hơn bắt ở phút 130.

1. Thanh công cụ -> **Draw new skeleton** -> chọn **Shape** (đây là ảnh tĩnh, không phải clip).
2. Vẽ: CVAT thả cả bộ 17 điểm xuống trong một cái box.
3. Kéo/xoay cả box cho khớp thô **trước**, rồi mới kéo từng điểm.
4. Đặt cờ cho mọi điểm không nhìn thấy rõ (xem bảng dưới), rồi bấm **Save** trên toolbar.
5. Export thử: **Export -> COCO Keypoints 1.0**. Giải nén, rồi:

```bash
python3 tools/coco_kp_to_yolo_pose.py --coco <file>.json --out dataset/labels/train
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train \
    --labels dataset/labels/train --out outputs/vis_train
```

Mở `outputs/vis_train/train_01.jpg`. Xanh = bên trái cơ thể, cam = bên phải, vàng = bị che.
Hai màu cắt chéo nhau ở vai hay hông => bạn vừa đảo trái/phải.

> **Bấm thẳng vào biểu tượng Save trên toolbar.** `Ctrl+S` không nhận nếu con trỏ
> không ở trên canvas - đây là lỗi mất nhãn phổ biến nhất của Ngày 3.

### Ba trạng thái, hai câu hỏi

| Bạn thấy khớp đó không? | Nó còn trong khung hình không? | Chọn | Trong CVAT | Ra file |
| --- | --- | --- | --- | ---: |
| Có | - | nhìn thấy rõ | không tick gì | `v = 2` |
| Không, bị che | Còn | **vẫn đặt chấm** ở vị trí ước lượng | tick **Occluded** (`q`) | `v = 1` |
| Không, ra ngoài mép ảnh | Không | **không đặt chấm** | tick **Outside** (`o`) | `v = 0` |

**Không bao giờ dùng `h` (Hidden).** Nó trông y hệt Outside trên màn hình nhưng
không được lưu - điểm đó vẫn xuất ra `v = 2` ở vị trí cũ, sai mà không có một
dòng cảnh báo nào.

Di chuột lên **một điểm** rồi bấm phím: chỉ điểm đó đổi cờ. Di chuột lên **box bao**
rồi bấm: cả skeleton đổi cờ.

---

## Chặng 3 - Gán 18 ảnh còn lại (0:40-2:10)

Khoảng 4 phút một ảnh. Nếu đang mất 8 phút cho một ảnh, bạn đang chỉnh đến từng pixel -
dừng lại. **Đúng khớp quan trọng hơn đúng pixel:** model chỉ trả lời chính xác được
đến khoảng 4 pixel, còn sai trái/phải thì nó học sai vĩnh viễn.

Luật bắt buộc:

1. **Luôn đủ 17 điểm cho mọi người.** Điểm không dùng được thì gắn cờ, không bao giờ xoá.
2. **Trái/phải tính theo cơ thể người, không theo bức ảnh.** Người quay mặt về phía bạn
   thì tay trái của họ xuất hiện ở bên phải ảnh - vẫn ghi `left_wrist`.
   Cách kiểm trong 1 giây: tự tưởng tượng bạn đứng vào chỗ người đó rồi giơ tay trái lên.
3. **Làm xong hẳn một người rồi mới sang người kế tiếp.** Đây là cách duy nhất tránh lỗi
   "nhầm người".
4. **Người quá nhỏ**: bộ ảnh này đã được chọn sao cho mọi người trong ảnh đều đủ lớn để gán.
   Nếu bạn thấy một ca mình phân vân, đó là một ca mơ hồ thật - ghi vào `GUIDELINE_MINI.md`.
5. **Hông**: không nhìn thấy được trên bất kỳ người mặc quần áo nào. Nó là ước lượng giải phẫu.
   Nhóm bạn phải thống nhất một luật và ghi vào `GUIDELINE_MINI.md`, kèm một ảnh mẫu.

### Bộ thứ hai: face/hand cho 5 ảnh

Dựng **một skeleton label riêng**, topology riêng, **không trộn** vào bộ 17 điểm:

- 21 điểm một bàn tay (MediaPipe / COCO-WholeBody)
- 5 điểm mặt rút gọn: hai mắt, mũi, hai khoé miệng

Tạo một task CVAT thứ hai với 5 ảnh bạn chọn (ưu tiên ảnh thấy rõ bàn tay trên tay lái).
Export riêng, đặt vào `annotations/face_hand/`. Bộ này **không** được chấm bằng OKS -
không có gold cho nó - nhưng nó nằm trong reviewer checklist và trong rubric.

> Bàn tay một bên đã là 21 điểm, hơn cả bộ thân 17 điểm. Đó là lý do bộ này chỉ làm
> 5 ảnh, không phải 20.

---

## Chặng 4 - Ba lượt kiểm rồi khoá nhãn (2:10-2:30)

Rẻ trước, đắt sau. Đừng đảo thứ tự.

**Lượt 1 - hình dáng (1 giây/người).** Bật đường nối, không phóng to.

```bash
python3 tools/visualize_pose.py --images dataset/images/train \
    --labels dataset/labels/train --out outputs/vis_train
```

Bắt: đảo trái/phải (xương cắt chéo ở thân), nhầm người (xương kéo sang cơ thể bên cạnh).
Một bộ xương người gần như không bao giờ tự cắt chéo ở thân - trừ khi người đó đang vặn
mình. Thấy cắt chéo thì **nghi ngờ và kiểm lại pose trước**, rồi mới sửa.

**Lượt 2 - đếm (không cần mắt).**

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visibility_report.py --labels dataset/labels/train \
    --out outputs/visibility_report.json --markdown reports/visibility_report.md
```

Bắt: thiếu điểm, cờ sai, và **xoá khớp bị che**. Dấu hiệu của lỗi cuối: cả bài không có
một khớp `v = 1` nào, hoặc một người nằm gọn giữa ảnh mà lại có 5-6 khớp `v = 0`.

**Lượt 3 - phóng to (chỉ vài người mẫu).** Zoom 200%, chọn 2-3 người. Bắt: chấm lệch khỏi khớp.
Đây là lượt đắt nhất nên làm cuối cùng và làm ít.

**Kiểm chéo với bạn cùng nhóm - so bảng đếm TRƯỚC, so hình sau:**

```bash
python3 tools/visibility_report.py --labels dataset/labels/train \
    --compare ../ban_cung_nhom/dataset/labels/train --markdown reports/visibility_compare.md
```

Khớp nào lệch `%v=1` nhiều nhất là khớp guideline của nhóm chưa nói rõ. Sửa guideline
trước, sửa nhãn sau. Điền `reports/REVIEWER_CHECKLIST.md` cho bài của người kia và ghi
lỗi tìm được vào `reports/review_partner.md`.

**Khoá nhãn.** Commit. Từ đây trở đi không sửa nhãn nữa cho tới khi nhận gold.

---

## Chặng 5 - Chấm với gold và rework (2:30-3:10)

Protected release mở sau khi cả lớp đã khoá. Giải nén sao cho có `gold/labels/train/*.txt`.

```bash
python3 tools/evaluate_pose_annotations.py --pred dataset/labels/train \
    --gold gold/labels/train --images dataset/images/train --out outputs/eval_vs_gold.json
```

Script trả về OKS và một **danh sách lỗi đã gọi tên**. Sửa theo đúng thứ tự này:

| Ưu tiên | Lỗi | Sửa thế nào |
| ---: | --- | --- |
| 1 | Đảo trái/phải | Đổi lại hai điểm. Nguy hiểm nhất vì augmentation lật ảnh dạy cái sai này hai lần |
| 2 | Nhầm người | Đặt lại điểm về đúng cơ thể |
| 3 | Thiếu/thừa người | Gán bổ sung, hoặc xoá skeleton thừa |
| 4 | Xoá khớp bị che | Gán lại với `v = 1` và đặt chấm ước lượng |
| 5 | Trượt hẳn | Kéo chấm về đúng khớp |
| 6 | Lệch nhẹ | Sửa nếu còn thời gian. Ít hại nhất |

Bỏ qua hai mục `Cờ khác gold` và `Gold để v=0` - chúng không trừ điểm, xem mục cuối README.

Chạy lại script sau khi sửa. **Rework không bị trừ điểm** - ghi vào báo cáo: điểm trước,
sửa gì, điểm sau.

> Điều bất ngờ: lỗi đảo trái/phải không xảy ra ở ảnh khó. Nó xảy ra ở ảnh dễ, rõ ràng,
> lúc bạn đang làm nhanh. Kiểm cả những ảnh bạn thấy chắc chắn nhất.

---

## Chặng 6 - Colab: fine-tune và đánh giá (3:10-3:50)

1. Nén cả thư mục `Day4-Lab/` (bỏ `gold/` ra - notebook không được dùng gold để train),
   upload lên Colab hoặc mount Drive. Bật GPU: Runtime -> Change runtime type -> T4.
2. Mở `notebooks/day4_pose_finetune_yolo26.ipynb`, Run all.
3. Notebook sẽ: kiểm nhãn -> đo model gốc trên tập test -> fine-tune trên 20 ảnh của bạn
   -> đo lại -> vẽ kết quả -> so nhãn của bạn với model.
4. Tải `outputs/eval_model.json` về, commit.

20 ảnh là quá ít để ra một model dùng được. Con số đáng đọc là **chênh lệch** trước/sau,
và **kiểu sai** bạn nhìn thấy ở phần visualize - không phải giá trị mAP tuyệt đối.

---

## Chặng 7 - Nộp (3:50-4:00)

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
git add dataset/labels/train annotations reports outputs GUIDELINE_MINI.md
git commit -m "Day 4: pose annotation + eval"
```

Đối chiếu [RUBRIC.md](RUBRIC.md) một lượt trước khi push.
