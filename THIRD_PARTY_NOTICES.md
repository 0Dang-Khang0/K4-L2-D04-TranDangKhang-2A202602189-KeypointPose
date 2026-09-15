# Third-party notices

## Cabin practice pack

- **HSRD-100: 100 High-Quality 3D Human Scans Dataset**, Digital Reality Lab, revision `fe753f2eb34ec0194511eecebbcbffa2fddb67e1`, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): <https://huggingface.co/datasets/digitalrealitylab/HSRD-100>. Two consented scan previews were selected, cropped, metadata-stripped and re-encoded.
- **Driver Risk Behavior Dataset for Embedded Vision Applications**, Juan Manuel Calvo Duque, version 1, DOI `10.17632/562zj8n7xf.1`, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): <https://data.mendeley.com/datasets/562zj8n7xf/1>. Five frames were cropped to the driver ROI, every visible face was covered with an opaque mask, and metadata was stripped.
- **RGB and Depth videos directory**, Leandro L. Di Stasi et al., version 1, DOI `10.25452/figshare.plus.22277668.v1`, [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/): <https://plus.figshare.com/articles/dataset/RGB_and_Depth_videos_directory/22277668>. Three frames preserve the published face mask, receive documented sensor stressors and have metadata stripped.

The combined cabin practice pack is classroom/noncommercial only. Attribution, license links and change notices must travel with any copy. No endorsement by source authors is implied.

Exact source members, timestamps, integrity values, transforms and output SHA-256 hashes are in `data/source-selection.json` and `data/image-manifest.csv`.

## Technical references

- CVAT skeleton workflow: <https://github.com/cvat-ai/cvat> and <https://docs.cvat.ai/docs/manual/advanced/skeletons/>.
- COCO person keypoint topology: <https://cocodataset.org/#keypoints-2020>.
- Ultralytics is installed only when the Colab notebook runs. No model weights are redistributed; review <https://www.ultralytics.com/license> for its terms.

This notice records attribution and engineering boundaries; it is not legal advice.
