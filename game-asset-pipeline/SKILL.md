---
name: Game Asset Pipeline
description: Local AI game asset generation pipeline for sprites, textures, 3D props, and video. Covers model selection, workflows, quality, and licensing for indie game dev.
---

# Game Asset Pipeline — Local AI

## Machine
- Windows 10, RTX 3090 24 GB, 28 GB RAM (→40 GB), Xeon E5-2696 v4
- ComfyUI 0.44.0 at `D:\AI\ComfyUI`, models at `D:\AI\models\`
- Docs: `D:\AI\SETUP_DOCUMENTATION.md`, `D:\AI\HANDOFF.md`, `D:\AI\PROJECTS.md`

## Task → Model mapping

| Task | Model | Workflow | Notes |
|---|---|---|---|
| Fast general images | Z-Image Turbo (int8/FP8) | `image_z_image_turbo_int8.json` | 8-9 steps, CFG 1.0, 1024-1344px |
| Anime illustration | Nova Anime XL v19 / Prefect Pony XL v6 | `nova_anime_xl_v19.json` / `prefect_pony_xl_v6.json` | 28 steps, CFG 5-6, CLIP skip 2 |
| Structure-guided (pose/depth/canny) | Z-Image Turbo + ControlNet Union | `image_z_image_turbo_fun_union_controlnet_int8.json` | Input image drives structure |
| Sprites (pixel art) | SDXL + Pixel-Art-XL LoRA (not installed yet) | To be created | Downscale nearest-neighbor, palette-limit |
| Tileable textures | Z-Image Turbo or SDXL + tileable LoRA | To be created | + TextureAlchemy PBR extraction |
| 3D props | Pixal3D / TRELLIS.2 (native) | `3d_pixal3d_trellis2_image_to_model.json` | GLB + PBR, 1024-1536px |
| Video clips | MiniMax H3 (Ref2VA) | `video_minimax_h3_r2v_safe.json` | 5s 864x480, 4-step turbo; 15s ~23 min |
| Concept art / skyboxes | Z-Image Turbo | t2i workflow | Equirectangular LoRA for 360 skies |

## Sprite pipeline

1. Generate at 1024 with SDXL + Pixel-Art-XL LoRA (or Z-Image + pixel style).
2. Downscale with nearest-neighbor to 32/48/64/96/128 px.
3. Palette-limit to 16-32 colors (Aseprite / Pixelorama).
4. Manual cleanup (fix stray pixels, align silhouette).
5. Consistency: lock seed, ControlNet OpenPose for poses, or train a character LoRA.
6. Animation: generate key poses with ControlNet; hand-clean; pack sprite sheet.

**Honest limits:** AI makes "pixel-art-like" images, not true pixel grids. Walk cycles need hand-cleaning. Cloud tools (PixelLab, Retro Diffusion, Sprite AI) are more turnkey for animation, but local wins on privacy/cost/style consistency.

## Texture pipeline

1. Tileable albedo: SDXL + tileable LoRA or Z-Image Turbo, 1024/2048, top-down prompt.
2. Seamless fix: TextureAlchemy "Seamless Tiling Maker" + inpaint seam.
3. PBR extraction: TextureAlchemy (Marigold/Lotus) → normal, roughness, metallic, AO, height, curvature.
   - Alternative: CHORD (Ubisoft, research-only — not for commercial).
4. Post: Texture Equalizer (remove baked lighting), channel-pack ORM for Unity/Unreal.
5. Upscale to 2K/4K with SeedVR2 (conservative, open source) or ESRGAN (fast).

**Honest limits:** organic/stylized materials are great; precise parametric architecture (brick patterns) still belongs in Substance Designer. CHORD is research-only.

## 3D pipeline

1. Concept image (Z-Image Turbo) — clean background, front view. Optional multi-view.
2. Image-to-3D: ComfyUI template "Pixal3D & TRELLIS.2" → GLB + PBR.
   - Boolean off = Pixal3D (better input fidelity), on = TRELLIS.2 (complex topology).
3. Post-process in ComfyUI (remesh, decimate, fill holes, smooth normals) or Blender.
4. Retopo/UV/LOD/collision in Blender; game-ready pass.
5. Rig: SkinTokens (MIT, auto-rig, ~6 GB GGML) or Mesh2Motion (free, local, Mixamo-like).
6. Export FBX/GLB to engine.

**Honest limits:** static props are usable after cleanup; characters need manual rigging/weight painting; animation is the hard part. Closed-source cloud (Meshy, Tripo, Rodin) still better for characters.

## Consistency and automation

- **Workflow JSONs live in the project repo.** Commit them; same workflow + seed = reproducible output.
- **Staging folder discipline:** output → `staging/`, human review, then move to engine project.
- **ComfyUI API:** POST workflow JSON to `/prompt`; poll `/history` or use websocket.
- **LoRA training:** Kohya sd-scripts. SDXL 12-16 GB, FLUX 16-24 GB. 10-30 images, 1024px, 1000-2500 steps. This is the key to a coherent art style.

## Licensing (personal use = no restrictions; for commercial:)

| Category | Models | Commercial? |
|---|---|---|
| Apache 2.0 / MIT | Z-Image/Turbo, FLUX.2 klein 4B, FLUX.1 schnell, Qwen-Image/2512, ERNIE, HiDream (MIT), GLM (MIT), LongCat, Pixal3D, TRELLIS.2 (MIT), SkinTokens (MIT), Mesh2Motion | Yes |
| Revenue-capped | FLUX 3 Klein (<$5M + filters/review), SD 3.5 (<$1M), Krea 2 (<$1M), Stable Fast 3D (<$1M) | Under threshold |
| Non-commercial | FLUX.1 dev, FLUX.2 dev/klein 9B, Qwen-Image-2.1, Ideogram 4, HunyuanImage-3.0 (territorial) | No |
| Research-only | CHORD (Ubisoft PBR), Marigold/Lotus (check) | Prototype only |

Also: Steam requires AI disclosure for shipped games. Civitai LoRAs have individual licenses. US copyright requires human authorship.

## What's installed vs. not

**Installed:** Z-Image Turbo, Nova Anime XL, Prefect Pony XL, Pixal3D, TRELLIS.2, MiniMax H3, ControlNet Union, BiRefNet, MoGe, DINOv3.

**NOT installed (available on request):** SDXL base, FLUX, pixel-art LoRAs, SDXL ControlNet, H3 FL2VA, upscale models (ESRGAN/SeedVR2), texture LoRAs, Kohya training tools, Krita AI Diffusion, Aseprite/Pixelorama.

## References

- `D:\AI\SETUP_DOCUMENTATION.md` — full 13-section reference
- `D:\AI\HANDOFF.md` — agent quick reference (install procedures)
- `D:\AI\PROJECTS.md` — project tracker
- `D:\AI\README.txt` — user quick reference
- Prompt repos: booru-prompt-gallery, sd-akashic, SupaGruen cheat sheet, Dalabad templates, roblaughter style-reference, zsky prompt library
