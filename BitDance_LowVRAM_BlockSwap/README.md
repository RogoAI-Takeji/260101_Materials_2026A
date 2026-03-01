# BitDance 14B - Low VRAM Block-Swap

ComfyUI custom node (nodes.py) with block-swap optimization for running BitDance 14B on low VRAM GPUs.

## Overview

BitDance 14B normally requires 24GB+ VRAM due to its large text encoder (18GB+).
This modified `nodes.py` implements **block-swap streaming**, enabling generation on 16GB VRAM GPUs (e.g. RTX 4080). Lower VRAM may also work by adjusting `num_gpu_blocks`.

### Results (RTX 4080 16GB)
- Generation time: ~13 minutes (vs ~40 min without block-swap)
- VRAM usage: ~10.6GB peak
- `num_gpu_blocks`: 15 (optimal for 16GB)

## How Block-Swap Works

The 40-layer LLM transformer is offloaded to CPU RAM.
During inference, each block streams: CPU → GPU (compute) → CPU (result).
KV-cache is also offloaded to CPU between blocks.

| VRAM | Recommended num_gpu_blocks |
|------|---------------------------|
| 16GB | 15 |
| 12GB | 10 (untested) |
| 8GB  | 5 (untested) |

## Files

| File | Description |
|------|-------------|
| `nodes.py` | Modified BitDance custom node with block-swap implementation |
| `BitDance_14B_64x_portrait.json` | ComfyUI workflow for portrait image generation (768x1280) |

## Requirements

- GPU: 16GB VRAM verified (RTX 4080). Lower VRAM may work.
- RAM: 64GB recommended (text encoder loads to CPU)
- ComfyUI with [ComfyUI-BitDance](https://github.com/shallowdream204/BitDance) installed

## Models Required

Download and place in your ComfyUI models/bitdance/ folder:

- `BitDance_14B_MainModel_FP8.safetensors`
- `BitDance_TextEncoder_FP8.safetensors`
- `BitDance_VAE_FP16.safetensors`

Recommended: Use FP8 versions from ComfyUI Blog for reduced VRAM usage.

## Usage

1. Replace `nodes.py` in your `custom_nodes/Comfyui-bitdance/` folder
2. Restart ComfyUI
3. Load `BitDance_14B_64x_portrait.json` workflow
4. Set `num_gpu_blocks: 15` in the BitDanceSampler node (adjust for your VRAM)
5. Run

## License

Apache 2.0 (same as original BitDance repository)

## Credits

- Original BitDance: [shallowdream204/BitDance](https://github.com/shallowdream204/BitDance)
- Block-swap implementation: [RogoAI-Takeji](https://github.com/RogoAI-Takeji) / YouTube: 老後AI (RogoAI)
