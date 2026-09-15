# Third-party notices and sources

## Bộ 10 ảnh trong `data/images/`

- **HSRD-100: 100 High-Quality 3D Human Scans Dataset**, Digital Reality Lab, pinned revision `fe753f2eb34ec0194511eecebbcbffa2fddb67e1`, licensed CC BY 4.0: <https://huggingface.co/datasets/digitalrealitylab/HSRD-100>. Changes: selected two consented scan previews, cropped one front view/person, stripped metadata and re-encoded JPEG.
- **Driver Risk Behavior Dataset for Embedded Vision Applications**, Juan Manuel Calvo Duque, version 1, DOI `10.17632/562zj8n7xf.1`, licensed CC BY 4.0: <https://data.mendeley.com/datasets/562zj8n7xf/1>. Changes: selected five frames from five recording groups, cropped to driver ROI, applied opaque face masks, stripped metadata and re-encoded JPEG.
- **RGB and Depth videos directory**, Leandro L. Di Stasi et al., version 1, DOI `10.25452/figshare.plus.22277668.v1`, licensed CC BY-NC 4.0: <https://plus.figshare.com/articles/dataset/RGB_and_Depth_videos_directory/22277668>. Changes: selected three frames from three participant videos, preserved the publisher's face mask, applied documented sensor stressors, stripped metadata and re-encoded JPEG.

**Bộ ảnh gộp lại là phi thương mại** — ràng buộc CC BY-NC 4.0 là ràng buộc nghiêm nhất trong ba
nguồn nên nó áp cho cả pack. Attribution, link license và ghi chú thay đổi phải đi kèm mọi bản
sao. Không ngụ ý tác giả nguồn bảo trợ cho bài học này.

Pack này được phân phối công khai kèm repo, theo đúng điều khoản redistribute của cả ba license
nguồn. Ảnh là người thật đã đồng ý cho sử dụng dữ liệu, 8 ảnh có mask che mặt; xem ràng buộc sử
dụng ở [RULES.md](RULES.md). Nếu bạn là người trong ảnh hoặc là tác giả nguồn và muốn gỡ một ảnh
khỏi pack, liên hệ người phụ trách lớp — ảnh sẽ được gỡ khỏi bản phát hành mà không cần tranh
luận về license.

Nguồn, license và thay đổi của từng ảnh: [`data/image-manifest.csv`](data/image-manifest.csv).

## Công cụ và tham chiếu kỹ thuật

- CVAT skeleton SVG / COCO Keypoints workflow: dự án Apache-2.0 tại <https://github.com/cvat-ai/cvat> và <https://docs.cvat.ai/docs/manual/advanced/skeletons/>.
- Tên và topology 17 điểm COCO: <https://cocodataset.org/#keypoints-2020>.
- Diagnostic tuỳ chọn: `ultralytics` và checkpoint pose tải lúc chạy; xem điều khoản AGPL-3.0/Enterprise tại <https://www.ultralytics.com/license>. Không redistribute weight.

Tài liệu này ghi attribution và ranh giới kỹ thuật; không phải tư vấn pháp lý.
