---
name: ComfyUI Setup
description: Local ComfyUI installation, model management, workflows, and troubleshooting for AI image/video/3D generation on Windows with RTX 3090.
---

# ComfyUI Setup — Local AI Generation

## Machine
- Windows 10 Pro, RTX 3090 24 GB (Ampere), 28 GB RAM (→40 GB), Xeon E5-2696 v4
- Drives: C: (OS, 35 GB free), D: SSD 207 GB, E: HDD 9.3 TB
- ComfyUI 0.44.0 portable (CUDA 13, Python 3.13) at `D:\AI\ComfyUI`
- Server: `http://127.0.0.1:8188`

## Folder structure
- `D:\AI\ComfyUI` — app + python_embeded
- `D:\AI\models\` — all models (via extra_model_paths.yaml)
- `D:\AI\workflows\` — 2d/, 3d/, video/ workflow JSONs
- `D:\AI\outputs\staging\<project>\` — per-project staging
- `D:\AI\cache\` — HF/Torch downloads (HF_HOME, TORCH_HOME)
- `D:\AI\README.txt`, `SETUP_DOCUMENTATION.md`, `HANDOFF.md`, `PROJECTS.md`

## Model folders
| Type | Folder | Loader |
|---|---|---|
| SDXL checkpoint | `D:\AI\models\checkpoints\` | CheckpointLoaderSimple |
| Diffusion model (UNET) | `D:\AI\models\diffusion_models\` | UNETLoader |
| Text encoder | `D:\AI\models\text_encoders\` | CLIPLoader |
| VAE | `D:\AI\models\vae\` | VAELoader |
| ControlNet/patch | `D:\AI\models\model_patches\` | ModelPatchLoader |
| LoRA | `D:\AI\models\loras\` | LoraLoader |
| CLIP vision | `D:\AI\models\clip_vision\` | CLIPVisionLoader |
| Upscale | `D:\AI\models\upscale_models\` | UpscaleModelLoader |
| Background removal | `D:\AI\models\background_removal\` | LoadBackgroundRemovalModel |
| 3D | `D:\AI\models\diffusion_models\` + `vae\` | Native 3D nodes |

## Critical rules

1. **Both `widgets_values` AND `widgets_values_named`** must be updated when changing model names in a workflow. ComfyUI 0.44.0 uses `widgets_values_named` for actual values.
2. **Restart ComfyUI** after adding models (model list cached at startup).
3. **Browser cache:** If "missing model" appears, reload page (F5) and reopen workflow.
4. **int8/FP8 builds** required for 28 GB RAM. BF16 needs 40 GB.
5. **H3 requires:** `--disable-pinned-memory --fp16-intermediates` flags.
6. **Don't mix model types.** Z-Image uses UNET/CLIP/VAE (separate). SDXL checkpoints use CheckpointLoaderSimple.
7. **extra_model_paths.yaml** uses forward slashes: `D:/AI/models/`.
8. **HF/Torch cache** must be on D: (set in launch bat).
9. **MarkdownNote nodes** can reference model names in text; clean them to avoid false "missing model" warnings.

## Install a new model

1. Download to `D:\AI\downloads\` via curl.
2. Move to correct folder under `D:\AI\models\`.
3. Verify safetensors header:
   ```powershell
   D:\AI\ComfyUI\python_embeded\python.exe -c "import struct,json,sys; sys.stdout.reconfigure(encoding='utf-8'); f=open(r'<path>','rb'); n=struct.unpack('<Q',f.read(8))[0]; h=json.loads(f.read(n)); print('OK', len(h), 'tensors')"
   ```
4. Restart ComfyUI.
5. Verify loader sees it via `http://127.0.0.1:8188/object_info/<LoaderNode>`.
6. Create workflow from existing template, update both `widgets_values` and `widgets_values_named`.
7. Test via API (see `test_zimage.py` for pattern).
8. Update README and documentation.

## Launch commands

```bat
:: Normal (images, 3D, general)
D:\AI\ComfyUI\run_nvidia_gpu.bat
:: --enable-manager

:: Video (MiniMax H3)
D:\AI\ComfyUI\run_nvidia_gpu_video.bat
:: --enable-manager --disable-pinned-memory --fp16-intermediates
```

## Key models and settings

| Model | Files | Settings |
|---|---|---|
| Z-Image Turbo | int8 UNET + FP8 encoder + ae.safetensors | 8-9 steps, CFG 1.0, res_multistep, simple, 1024-1344px |
| Nova Anime XL v19 | NovaAnimeILV190.safetensors (SDXL) | 28 steps, CFG 5, euler_ancestral, normal, CLIP skip 2 |
| Prefect Pony XL v6 | PrefectPonyXLV6.safetensors (Pony/SDXL) | 28 steps, CFG 6, euler_ancestral, normal, CLIP skip 2 |
| Pixal3D/TRELLIS.2 | int8 UNETs + VAEs + DINOv3 + BiRefNet + MoGe | 1024px, boolean: off=Pixal3D, on=TRELLIS.2 |
| MiniMax H3 | pruned int8 + NVFP4 encoder + video/audio VAEs | 5s 864x480, 4-step turbo LoRA; 15s 1344x768 ~23 min |

## Common issues

| Problem | Fix |
|---|---|
| `hostbuf_file_reader_read failed` | `--disable-dynamic-vram` or use int8/FP8 builds |
| `Fault failed: 2` (H3) | `--disable-pinned-memory --fp16-intermediates` + lower resolution |
| Screen goes black (TDR) | Reduce resolution/steps; increase TDR delay in registry |
| "Missing model" dialog | Fix both widgets fields; restart ComfyUI; reload page |
| Model not in dropdown | Restart ComfyUI (model list cached at startup) |
| H3 I2V fails | FL2VA not installed; use R2V safe workflow |

## Documentation files

- `D:\AI\README.txt` — user quick reference
- `D:\AI\SETUP_DOCUMENTATION.md` — full 13-section reference
- `D:\AI\HANDOFF.md` — agent quick reference (install procedures)
- `D:\AI\PROJECTS.md` — project tracker
- `D:\AI\projects\<slug>\README.md` — per-project docs

## Not installed (available on request)

SDXL base, FLUX, pixel-art LoRAs, SDXL ControlNet, H3 FL2VA, upscale models (ESRGAN/SeedVR2), texture LoRAs, Kohya training tools, Krita AI Diffusion, Aseprite/Pixelorama.
