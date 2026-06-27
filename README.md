# RogoAI Materials 2026A (Jan-Jun)

---

## SCAIL-2 / SAM 3.1 Video Workflows

### workflows/sam3.1_t2v_test.json

- **公開日**: 2026年6月27日
- **説明**: SAM 3.1で動画内の対象を検出・追跡し、そのマスクをLTX 2.3 masked T2V / refineへ渡す検証用ComfyUIワークフロー
- **構成**: Stage 1で低解像度masked T2Vを生成し、フレームとマスクを640x360へ拡大後、Stage 2で低denoise refinementを行う
- **YouTube解説**: SCAIL-2 / SAM 3.1 / RoPE拡張の検証動画

#### 機能

- SAM 3.1による初期フレームの対象検出
- SAM 3.1による動画全体の対象追跡
- 追跡マスクのプレビュー動画とマスク動画を出力
- LTX 2.3 GGUFモデルによるmasked T2V置換
- 低解像度生成後の640x360 refine

#### 必要なカスタムノード

- SAM 3 / SAM 3.1 ComfyUI nodes
- ComfyUI-LTXVideo
- ComfyUI-GGUF
- ComfyUI-KJNodes
- ComfyUI-VideoHelperSuite
- Easy Use nodes

#### 使用モデル

- sam3.1_multiplex_fp16.safetensors
- ltx-2.3-22b-distilled-Q4_K_M.gguf
- ltx23_inpaint_masked_t2v_rank128_v1_02500steps.safetensors
- LTX23_video_vae_bf16.safetensors
- gemma_3_12B_it_fp8_e4m3fn.safetensors
- ltx-2.3_text_projection_bf16.safetensors

#### 使い方

1. ComfyUIを起動
2. `workflows/sam3.1_t2v_test.json` を読み込む
3. 入力動画を指定する
4. `Mask Target Prompt` に追跡したい対象を入力する
5. `T2V Replacement Prompt` に置換後の見た目・動きを入力する
6. マスクプレビューを確認してから本生成する

#### 注意事項

- モデル本体、入力動画、参照画像は含めていません
- LTX 2.3本体はLightricks community licenseです
- LTX 2.3 Inpaint LoRAはAlissonerdx/LTX-LoRAs由来です
- SAM 3.1はMetaのSAM licenseに従ってください
- 小さな対象や密集した群れでは追跡が外れる場合があります

#### 参考

- SAM 3: https://github.com/facebookresearch/sam3
- SAM 3.1 model: https://huggingface.co/facebook/sam3
- LTX 2.3: https://huggingface.co/Lightricks/LTX-2.3
- LTX 2.3 Inpaint LoRA: https://huggingface.co/Alissonerdx/LTX-LoRAs
- ComfyUI-LTXVideo: https://github.com/Lightricks/ComfyUI-LTXVideo
- ComfyUI-GGUF: https://github.com/city96/ComfyUI-GGUF
- ComfyUI-KJNodes: https://github.com/kijai/ComfyUI-KJNodes
- ComfyUI-VideoHelperSuite: https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite

#### クレジット

- 開発: 老後AI (takejii)
- SAM 3 / SAM 3.1: Meta
- LTX 2.3: Lightricks
- LTX 2.3 Inpaint LoRA: Alissonerdx

### workflows/my_video_wan21_scail2_auto_long_metabatch_preview_range_lightx2v_4step.json

- **公開日**: 2026年6月27日
- **説明**: SCAIL-2 / Wan 2.1 の長尺動画生成用ComfyUIワークフロー。MetaBatchで動画をチャンク処理し、preview range、フレーム間引き、出力fps調整、LightX2V 4-step設定に対応
- **対応**: ローカルComfyUI環境向け。入力動画と参照画像はサンプル名に置換済み
- **YouTube解説**: SCAIL-2 / RoPE拡張の検証動画

#### 機能

- SCAIL-2のポーズ動画参照を長尺動画向けにチャンク分割
- MetaBatchで自動再キューし、最終動画へ連結
- `start_frame` / `process_frames` によるpreview range確認
- `select_every_nth` / `force_rate` によるフレーム選択と出力fps制御
- LightX2V Wan 2.1 I2V rank64 LoRAを使った4-step高速化設定
- `overlap_frames=0` の独立チャンク生成と、重なりありの連続生成を切り替え可能

#### 必要なカスタムノード

- ComfyUI native SCAIL-2 nodes
- ComfyUI-VideoHelperSuite
- ComfyUI-GGUF
- SCAIL-2 long-video helper nodes

#### 使用モデル

- SCAIL-2 / Wan 2.1 model or GGUF variant
- wan2.1_SCAIL_2_DPO_lora_bf16.safetensors
- lightx2v_I2V_14B_480p_cfg_step_distill_rank64_bf16.safetensors
- umt5_xxl_fp8_e4m3fn_scaled.safetensors
- Wan2_1_VAE_bf16.safetensors
- clip_vision_h.safetensors
- sam3.1_multiplex_fp16.safetensors

#### 使い方

1. ComfyUIを起動
2. `workflows/my_video_wan21_scail2_auto_long_metabatch_preview_range_lightx2v_4step.json` を読み込む
3. `example_driving_video.mp4` を任意の駆動動画に置き換える
4. `example_reference_image.png` を任意の参照画像に置き換える
5. preview rangeで短い範囲を確認してから、必要に応じて全体を生成する

#### 注意事項

- モデル本体、入力動画、参照画像は含めていません
- workflow内の入力ファイル名は公開用のプレースホルダーです
- 長尺生成ではチャンク境界で見た目や動きが揺れる場合があります
- 音声は含めていません。必要な場合は生成後に元動画の音声を合成してください

#### 参考

- SCAIL-2: https://github.com/zai-org/SCAIL-2
- Comfy-Org SCAIL-2: https://huggingface.co/Comfy-Org/SCAIL-2
- Wan 2.1: https://github.com/Wan-Video/Wan2.1
- LightX2V LoRA: https://huggingface.co/Kijai/WanVideo_comfy/tree/main/Lightx2v
- ComfyUI-VideoHelperSuite: https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite
- ComfyUI-GGUF: https://github.com/city96/ComfyUI-GGUF

#### クレジット

- 開発: 老後AI (takejii)
- SCAIL-2: Zhipu AI / Z.ai
- Wan 2.1: Alibaba
- LightX2V / ComfyUI model packaging: Kijai / Comfy-Org

## LTX2 Sound-to-Video Workflow

### LTX2_S2V_GGUF_12GB.json

- **公開日**: 2026年1月28日
- **説明**: LTX2でSound-to-Videoを実現するComfyUIワークフロー
- **モデル**: LTX-AV GGUF 12GBモデル対応
- **Civitai版を修正**: 修正版ワークフロー

#### 機能

- 音声ファイル（Qwen3-TTS等で生成）からリップシンク動画を生成
- 複数人会話対応（声ごとに顔を指定可能）
- 25fps、最大13秒（326フレーム）の動画生成
- 10分で13秒の動画を生成

#### 必要なノード

- LTXVGemmaCLIPModelLoader
- LTXVAudioVAELoader
- MultimodalGuider
- EmptyLTXVLatentVideo
- LTXVEmptyLatentAudio
- LTXVConditioning
- LTXVConcatAVLatent
- LTXVSeparateAVLatent

#### 使用モデル

- ltx-av-step-1751000_vocoder_24K.safetensors
- gemma-3-12b-it-qat-q4_0-unquantized_readout_proj

#### 使い方

1. ComfyUIを起動
2. 「Load」からこのJSONファイルを読み込む
3. 音声ファイルを準備（Qwen3-TTS等で生成）
4. プロンプトを記入
5. フレーム数を設定（25fps × 秒数）
6. Queueボタンで生成開始

#### 注意事項

- **重要**: LTX2は音声生成機能を持たないため、音声は別途TTSで生成する必要があります
- VRAMは12GB以上推奨（GGUF版使用）

#### 参考

- Civitai版LTX2 Workflow
- FlybirdXX氏のComfyUI-Qwen3-TTS
- YouTube動画: 【Qwen3-TTS ComfyUI】エラー完全解決！Coqui超え日本語音声＋LTX2複数人会話

#### クレジット

- 開発: 老後AI (takejii)
- ComfyUI-Qwen3-TTS: FlybirdXX
- LTX2モデル: Lightricks

## Z-Image Hybrid Workflow

### z_image_hybrid.json

- **公開日**: 2026年1月29日
- **説明**: Z-Image BaseとTurboを2段階で使用し、多様性と高速化を両立するハイブリッドワークフロー
- **対応**: 低VRAM環境（12GB以上推奨）
- **YouTube解説**: 【Z-Image-base】金太郎飴現象の正体と解決策・低VRAM環境の7モデル比較

#### 特徴

- **Stage 1 (Base)**: 初期8ステップで多様性を確保
- **Stage 2 (Turbo)**: ステップ2-9で高速詳細化
- 金太郎飴現象（クローンフェイス問題）を解決
- Baseの多様性とTurboの速度を両立

#### 必要なモデル

- z_image_bf16.safetensors (Base)
- z_image_turbo_bf16.safetensors (Turbo)
- qwen_3_4b.safetensors (CLIP)
- ae.safetensors (VAE)

#### 使い方

1. ComfyUIを起動
2. 「Load」からこのJSONファイルを読み込む
3. プロンプトを記入（複数人物の場合は個性を明記）
4. Stage 1でシード固定推奨（多様性確認用）
5. Queueボタンで生成開始

#### 設定のポイント

**Stage 1 (Base)**

- Steps: 7-8
- CFG: 5.0
- Sampler: res_multistep
- Scheduler: simple
- Denoise: 1.0
- return_with_leftover_noise: 有効（重要）

**Stage 2 (Turbo)**

- Start at step: 2
- Steps: 9
- CFG: 1.0（Turbo用低設定）
- Sampler: euler
- Scheduler: simple
- Denoise: 0.5

#### 注意事項

- Stage 1でノイズを残して次に渡す設定が必須
- 複数人物生成時にそれぞれの個性が保たれる
- Turbo単体使用時の金太郎飴現象を回避

#### 参考

- YouTube動画: 【Z-Image-base】金太郎飴現象の正体と解決策・低VRAM環境の7モデル比較
- GitHub Issues: 金太郎飴現象の技術解説

#### クレジット

- 開発: 老後AI (takejii)
- Z-Image Base/Turbo: ZhangYuhan (Alibaba)

## Bernini R2V (GGUF) Workflow

### bernini_r2v_gguf_dual_ui.json

- **公開日**: 2026年6月14日
- **説明**: Wan 2.2 Bernini-R の R2V（Reference-to-Video）ワークフロー（ルートB / GGUFデュアルエキスパート / UI形式）。`neuregex/ComfyUI-BerniniR` の `workflows/ui/` には t2v・i2i・i2i_gguf_dual はあるが **R2V のUIワークフローが無い**ため、当チャンネルで作成
- **対応**: 16GB VRAM（RTX 4060 Ti 16GB）で検証済み。低VRAM向けGGUF（Q4_K_M / デュアルエキスパート）構成
- **ルートAについて**: ルートA（fp8 native）はKijaiのオリジナルワークフローをそのまま使用し、ソースビデオの結線を外せばR2V化できます（当リポジトリでは配布しません）
- **YouTube解説**: （動画タイトルを記入）

#### 機能

- 参照画像から人物・被写体を起こして動画を生成（編集対象の動画は不要）
- I2Vで蓄積する色ズレ・顔ドリフトを回避
- GGUF構成で16GB VRAMでも動作

#### 必要なカスタムノード

- ComfyUI-BerniniR（neuregex / Apache-2.0）— BerniniRApplyPatches / BerniniRGuider / BerniniRSourceStream
- ComfyUI-GGUF（city96）— UnetLoaderGGUF

#### 使用モデル

- bernini_r_high_noise_14B-Q4_K_M.gguf ／ bernini_r_low_noise_14B-Q4_K_M.gguf
  （配布元: https://huggingface.co/neuregex/Bernini-R-GGUF → `ComfyUI/models/unet/`）
- umt5_xxl_fp8_e4m3fn_scaled.safetensors（CLIP / wan）
- wan_2.1_vae.safetensors（VAE）

#### 使い方

1. ComfyUIを起動
2. 「Load」からこのJSONファイルを読み込む
3. 参照画像を LoadImage（reference.png）に入力
4. プロンプトを記入
5. 解像度・フレーム数を設定（既定 512×512 / 57フレーム）
6. Queueボタンで生成開始（出力はWebP / 16fps）

#### 注意事項

- IPロック機能は無く、Identity に揺れ幅があります
- OOM が出る場合はフレーム数・解像度を下げてください
- 長尺はメタバッチで分割しますが、チャンク間の揺れは残り得ます

#### 参考

- 公式: https://github.com/bytedance/Bernini
- ルートB ノードパック: https://github.com/neuregex/ComfyUI-BerniniR
- GGUF重み: https://huggingface.co/neuregex/Bernini-R-GGUF
- ComfyUI-GGUF: https://github.com/city96/ComfyUI-GGUF
- ルートA用 fp8重み(Kijai): https://huggingface.co/Kijai/WanVideo_comfy_fp8_scaled/tree/main/Bernini
- YouTube動画: （動画タイトルを記入）

#### クレジット

- 開発: 老後AI (takejii)
- Bernini: ByteDance (Apache-2.0)
- ルートB ComfyUI-BerniniR: neuregex (Apache-2.0)
- ルートA fp8重み / オリジナルワークフロー: Kijai

## LTX 2.3 MSR Fixed Workflow

### LTX-2_3_MSR_sample_workflow_V2_fixed.json

- **公開日**: 2026年6月14日
- **説明**: LTX 2.3 MSR（Licon Studio）の V2（Prompt Relay）サンプルワークフローのリンク不整合を、当チャンネルで修正した版
- **対応**: 16GB VRAM（RTX 4060 Ti 16GB）。46GBの統合モデルを GGUF（Transformerのみ・Q3_K_M）＋軽量VAE/テキストエンコーダーへ換装
- **YouTube解説**: （動画タイトルを記入）

#### 機能

- 1つのシーン内で、複数の参照画像から複数の被写体を呼び出して制御
- Prompt Relay Encode 対応（V2）

#### 必要なカスタムノード

- ComfyUI-Licon-MSR（LiconMSR）
- kijai/ComfyUI-PromptRelay（PromptRelayEncode）※V2は必須。requirements未記載の場合あり → custom_nodes に git clone
- ComfyUI-LTXVideo（LTXICLoRALoaderModelOnly / LTXAddVideoICLoRAGuide 他）
- ComfyUI-KJNodes（VAELoaderKJ / ImageResizeKJv2）
- ComfyUI-GGUF（UnetLoaderGGUF）

#### 使用モデル

- LTX-2.3-22B-distilled-1.1-Q3_K_M.gguf（メイン / GGUF）
- LTX-2.3-Licon-MSR-V1.safetensors（MSR LoRA）
- LTX23_video_vae_bf16.safetensors / LTX23_audio_vae_bf16.safetensors（VAE）
- gemma_3_12B_it_fp4_mixed.safetensors + ltx-2.3_text_projection_bf16.safetensors（DualCLIP）

#### 使い方

1. ComfyUIを起動
2. 「Load」からこのJSONファイルを読み込む
3. 各参照画像（人物・背景）を LoadImage に入力
4. グローバル/ローカル/ネガティブプロンプトを設定
5. 解像度・フレーム数を設定（既定 1280×1920 / 145フレーム・24fps、用途に応じて調整）
6. Queueボタンで生成開始

#### 注意事項

- 顔の一貫性は強くありません（服装・髪型の一貫性は良好）
- 入力画像の解像度が異なると人物のアスペクト比に影響が出ます
- segment length（時間軸設定）は現状エラー回避のため空欄で運用
- ガイド用フレームが冒頭一瞬だけ出力に漏れる場合あり → 冒頭カット、または frame index を調整
- `Missing Node` / `Undefined Output Error` が出たら kijai/ComfyUI-PromptRelay を custom_nodes に git clone

#### 参考

- MSR LoRA: https://huggingface.co/LiconStudio/LTX-2.3-Multiple-Subject-Reference
- カスタムノード: https://github.com/liconstudio/ComfyUI-Licon-MSR
- Prompt Relay ノード（V2必須）: https://github.com/kijai/ComfyUI-PromptRelay
- LTX Director（時間軸側・参考 / WhatDreamsCost）: https://github.com/WhatDreamsCost/WhatDreamsCost-ComfyUI
- ワークフロー元: https://huggingface.co/RuneXX/LTX-2.3-Workflows
- YouTube動画: （動画タイトルを記入）

#### クレジット

- 開発: 老後AI (takejii)
- LTX 2.3 / MSR: Licon Studio
- ワークフロー参考: RuneXX
- LTX2モデル: Lightricks
