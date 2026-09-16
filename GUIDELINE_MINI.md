# Mini guideline - nhóm: Nhóm 1  |  người gán: Nguyễn Văn A  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng vị trí khớp hông thực tế, gán `v=1`. Không gán theo nếp gấp quần áo.<br><br>![Hông](./dataset/images/train/train_01.jpg) | Quần áo dài che khuất điểm thực tế, nhưng điểm đó vẫn nằm trong ảnh và có thể nội suy. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Dựa vào mắt, mũi và góc nghiêng của đầu để ước lượng vị trí, gán `v=1`.<br><br>![Tai](./dataset/images/train/train_02.jpg) | Điểm này rất quan trọng để mô hình học hướng nhìn của khuôn mặt. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Gán `v=2` hoặc `v=1` cho các điểm trong ảnh. Các điểm ngoài ảnh gán `v=0`.<br><br>![Cắt ảnh](./dataset/images/train/train_03.jpg) | Tránh đánh dấu ra ngoài ảnh gây nhiễu dữ liệu. Điểm ngoài mép ảnh bắt buộc `v=0`. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng theo độ dài và hướng của cẳng tay, gán `v=1`.<br><br>![Cổ tay](./dataset/images/train/train_04.jpg) | Mặc dù không nhìn thấy trực tiếp nhưng vẫn nội suy được do cơ cấu khớp xương. |
| Hai người chồng lên nhau | Gán đúng điểm cho từng người (tránh gán nhầm điểm người này cho người kia). Phần bị che gán `v=1`.<br><br>![Chồng lấp](./dataset/images/train/train_05.jpg) | Gán nhầm điểm sẽ khiến mô hình học sai hình dạng cơ thể người. |
| Người nhỏ đến mức nào thì không gán nữa | Dưới 50x50 pixel hoặc không thể phân biệt được các bộ phận cơ thể.<br><br>![Người nhỏ](./dataset/images/train/train_06.jpg) | Người quá nhỏ thì annotation không chính xác, gây noise cho mô hình. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_07.jpg`, người thứ `1`, khớp `vai trái`

- Mơ hồ ở chỗ nào: Người này đang mặc áo khoác rất dày, không rõ vị trí chính xác của khớp vai nằm ở phần mép áo hay sâu bên trong.
- Bạn quyết thế nào: Đặt điểm lùi vào trong một chút so với mép áo khoác ngoài, gán `v=1`.
- Vì sao: Để sát với cấu trúc xương thực tế thay vì hình dáng bên ngoài của áo.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Mô hình sẽ dự đoán khớp vai nằm ở viền ngoài của trang phục, dẫn đến sai lệch lớn khi inference trên người mặc áo mỏng.

### Ca 2 - ảnh `train_08.jpg`, người thứ `2`, khớp `đầu gối phải`

- Mơ hồ ở chỗ nào: Người đang ngồi vắt chéo chân, đầu gối phải bị đùi trái che khuất hoàn toàn, rất khó xác định độ cao của đầu gối.
- Bạn quyết thế nào: Dựa vào hướng của đùi phải và cẳng chân phải để tìm giao điểm ước lượng, gán `v=1`.
- Vì sao: Theo cấu trúc hình học của chân thì khớp phải nằm ở vị trí giao nhau đó.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Mô hình sẽ không học được cấu trúc vật lý của việc vắt chéo chân mà chỉ dựa vào điểm ảnh hiển thị.

### Ca 3 - ảnh `train_09.jpg`, người thứ `3`, khớp `mắt cá chân trái`

- Mơ hồ ở chỗ nào: Bàn chân trái bị bụi cây che lấp toàn bộ từ bắp chân trở xuống, không nhìn thấy cả giày.
- Bạn quyết thế nào: Dựa vào độ dài chân phải để suy ra độ dài chân trái tương ứng và đánh dấu ở vị trí bụi cây, gán `v=1`.
- Vì sao: Mặc dù bị che hoàn toàn nhưng điểm đó vẫn nằm bên trong khung hình và độ dài cơ thể tương đối cân xứng.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gán `v=0` (không có) thì sai luật, nếu đặt bừa thì model bị nhiễu do điểm gán không tỷ lệ với cơ thể.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `khớp hông trái` (bạn `45%` / họ `20%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline chưa rõ về việc khi nào mặc áo thun rộng trùm qua mông thì gán `v=1` (che khuất) hay `v=2` (nhìn thấy).
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Nếu mặc quần áo rộng (áo khoác, áo thùng thình) trùm qua khớp hông, tất cả đều phải chuyển thành gán `v=1` (bị che khuất) vì không nhìn thấy khớp trực tiếp bằng mắt.
