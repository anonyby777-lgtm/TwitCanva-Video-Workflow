# Seedance 2.5 Integration (ByteDance / Dreamina via Fal.ai)

TwitCanva supports **Seedance 2.5** as a video model on Video nodes and in the
Storyboard video generator.

## Why Seedance 2.5

- Single continuous takes from **4 to 30 seconds** (no clip stitching)
- **Native synchronized audio**: sound effects, ambience and **lip-synced speech**
- **Reference-to-video**: up to 30 reference images for character/style consistency
- Resolutions: 480p (fast) / 720p; aspect ratios: 21:9, 16:9, 4:3, 1:1, 3:4, 9:16

## Setup

Add your Fal.ai key to `.env` (same key already used for Kling 2.6):

```
FAL_API_KEY=your_fal_key
```

## How the node picks an endpoint

| Node inputs | Endpoint used |
|---|---|
| Prompt only | `bytedance/seedance-2.5/text-to-video` |
| 1 image input | `bytedance/seedance-2.5/image-to-video` (image = start frame) |
| 2+ image inputs | `bytedance/seedance-2.5/reference-to-video` (all images become `@Image1..@ImageN` references, in connection order) |
| 2+ inputs with explicit start/end frame assignment (Advanced → Frame Inputs) | `image-to-video` with `image_url` + `end_image_url` |

Notes:

- In reference mode, mention images in the prompt as `@Image1`, `@Image2`, …
  (indexes follow the order the parent nodes are connected).
- Connected TEXT nodes are prepended to the prompt — handy for a shared
  style/visual-bible block across many scene nodes.
- `Resolution` values map to Seedance's supported set: `480p` stays `480p`,
  everything else (`Auto`, `720p`, `1080p`) becomes `720p`.
- Duration is clamped to 4–30 s; the Audio toggle (Advanced settings) maps to
  `generate_audio`.

## Sample workflow

`public/workflows/turminha-da-floresta-e-o-trem-das-cores.json` is a full
kids-episode pipeline ("A Turminha da Floresta e o Trem das Cores") that uses
Seedance 2.5 reference-to-video with a shipped visual bible
(`public/assets/turminha/`, see `docs/visual-bible-turminha-da-floresta.md`).
Open the Workflows panel → **Public** tab → load it, then press Generate on any
scene node.
