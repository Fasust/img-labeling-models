# Image labeling models

Open image models in ONNX format, packaged for on-device use: face detection, face recognition, and
image–text embeddings with a precomputed label vocabulary. Each set is published as a GitHub release.
Files are never replaced after release; a changed model gets a new release.

| Release | Files | Model | Licence |
| --- | --- | --- | --- |
| `people-1` | `face_detection_yunet_2026may.onnx` | [YuNet](https://github.com/opencv/opencv_zoo/tree/main/models/face_detection_yunet) face detector, 2026may | MIT |
| `people-1` | `glintr100.onnx` | [AuraFace-v1](https://huggingface.co/fal/AuraFace-v1) face recognition, 512-d | Apache-2.0 |
| `content-search-2` | `siglip2_base_patch16_224_image_fp16.onnx` | Image tower of [SigLIP 2 B/16-224](https://huggingface.co/google/siglip2-base-patch16-224), exported to ONNX in float16 (float32 input `pixel_values` 1×3×224×224, output `pooler_output` 768-d) | Apache-2.0, modified |
| `content-search-2` | `labels_v2.bin` | The 4,585 tags of [Recognize Anything](https://github.com/xinyu1205/recognize-anything) (RAM++, Apache-2.0, © 2022 OPPO) with SigLIP 2 text vectors (mean of 3 prompts each, normalised) | Apache-2.0, derived |
| `content-search-3` | `siglip2_base_patch16_224_image_fp16.onnx` | The same image tower as `content-search-2` | Apache-2.0, modified |
| `content-search-3` | `labels_v3.bin` | 1,437 of those tags, picked by public photo-label lists and measured on Open Images photos, each with a threshold and a SigLIP 2 text vector | Apache-2.0, derived |
| `content-search-4` | `siglip2_base_patch16_224_image_fp16.onnx`, `labels_v3.bin` | The same files as `content-search-3` | as there |
| `content-search-4` | `siglip2_base_patch16_224_text_int8x.onnx` | Text tower of SigLIP 2 B/16-224 in ONNX, dynamically quantised to int8 except layer 0's MLP fc2, kept in float32 (int64 input `input_ids` 1×64, output `pooler_output` 768-d) | Apache-2.0, modified |
| `content-search-4` | `siglip2_tokenizer_v1.bin` | SigLIP 2's tokenizer (Gemma's byte-fallback BPE) in a compact binary form; see below | Apache-2.0, derived |

Each release includes its models' licence texts and notices. The face models are redistributed
unchanged. The SigLIP 2 files are modified, as each release's `NOTICE.txt` describes.

## Label file format

Little-endian: `SCRYLBL2` (`labels_v3.bin`), then int32 version, count and dimensions, float32 logit scale
(already exponentiated) and logit bias; for each label three strings (name, parent, exclusion group; empty
for none), each a uint16 byte length followed by UTF-8, and a float32 threshold (the cosine a photo must
reach); then `count × dimensions` float32 vectors. `SCRYLBL1` (`labels_v2.bin`) is the same without
thresholds.

## Tokenizer file format

Little-endian int32: `SCRYTOK1`, then the pad, eos and unk token IDs and the sequence length (64); the
single-character tokens as a count and (code point, ID) pairs; 256 byte-token IDs (`<0x00>` to `<0xFF>`, -1
for none); then the merges in rank order as a count and (left, right, merged) ID triples. Text is lowercased,
spaces become `▁`, every character becomes its token (or its UTF-8 byte tokens, or `<unk>`), the adjacent
pair of lowest rank is merged until none is left, then `<eos>` and padding follow. This gives the original
tokenizer's IDs for text without its added tokens.

## Checksums

Every file's SHA-256 is listed in its release notes. Verify a download before use.
