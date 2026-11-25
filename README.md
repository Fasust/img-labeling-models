# Image labeling models

Open image models in ONNX format, packaged for on-device use: face detection, face recognition, and
image–text embeddings with a precomputed label vocabulary. Each set is published as a GitHub release.
Files are never replaced after release; a changed model gets a new release.

| Release | Files | Model | Licence |
| --- | --- | --- | --- |
| `people-1` | `face_detection_yunet_2026may.onnx` | [YuNet](https://github.com/opencv/opencv_zoo/tree/main/models/face_detection_yunet) face detector, 2026may | MIT |
| `people-1` | `glintr100.onnx` | [AuraFace-v1](https://huggingface.co/fal/AuraFace-v1) face recognition, 512-d | Apache-2.0 |
| `content-search-1` | `siglip2_base_patch16_224_image_fp16.onnx` | Image tower of [SigLIP 2 B/16-224](https://huggingface.co/google/siglip2-base-patch16-224), exported to ONNX in float16 (float32 input `pixel_values` 1×3×224×224, output `pooler_output` 768-d) | Apache-2.0, modified |
| `content-search-1` | `labels_v1.bin` | 425 English labels with SigLIP 2 text vectors (mean of 3 prompts each, normalised) | Apache-2.0, derived |

Each release includes its models' licence texts and notices. The face models are redistributed
unchanged. The SigLIP 2 files are modified, as each release's `NOTICE.txt` describes.

## Label file format

Little-endian: `SCRYLBL1`, then int32 version, count and dimensions, float32 logit scale (already
exponentiated) and logit bias; for each label three strings (name, parent, exclusion group; empty for
none), each a uint16 byte length followed by UTF-8; then `count × dimensions` float32 vectors.

## Checksums

Every file's SHA-256 is listed in its release notes. Verify a download before use.
