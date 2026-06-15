# RogoAI Materials 2026A (Jan-Jun)

---

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
- **説明**: Wan 2.2 Bernini-R の R2V（Reference-to-Video）ワークフロー（ルートB / GGUF版）。公式・Kijai版とも R2V 用が提供されていないため、当チャンネルで作成
- **対応**: 16GB VRAM（RTX 4060 Ti 16GB）で検証済み。低VRAM向けGGUF（Q4_K_M）構成
- **ルートAについて**: ルートA（fp8 native）はKijaiのオリジナルワークフローをそのまま使用し、ソースビデオの結線を外せばR2V化できます（当リポジトリでは配布しません）
- **YouTube解説**: （動画タイトルを記入）

#### 機能

- 参照画像から人物・被写体を起こして動画を生成（編集対象の動画は不要）
- I2Vで蓄積する色ズレ・顔ドリフトを回避
- GGUF構成で16GB VRAMでも動作

#### 必要なカスタムノード

- ComfyUI-GGUF（UnetLoaderGGUF）
- ルートB用 Bernini-R ノードパック（BerniniRApplyPatches / BerniniRGuider / BerniniRSourceStream）

#### 使用モデル

- bernini_r_high_noise_14B-Q4_K_M.gguf
- bernini_r_low_noise_14B-Q4_K_M.gguf
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
- ルートA用 fp8重み(Kijai): https://huggingface.co/Kijai/WanVideo_comfy_fp8_scaled/tree/main/Bernini
- YouTube動画: （動画タイトルを記入）

#### クレジット

- 開発: 老後AI (takejii)
- Bernini: ByteDance (Apache-2.0)
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
- ComfyUI-PromptRelay（PromptRelayEncode）※V2は必須。requirements未記載の場合あり
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
- `Missing Node` / `Undefined Output Error` が出たら ComfyUI-PromptRelay を custom_nodes に git clone

#### 参考

- MSR LoRA: https://huggingface.co/LiconStudio/LTX-2.3-Multiple-Subject-Reference
- カスタムノード: https://github.com/liconstudio/ComfyUI-Licon-MSR
- ワークフロー参考: https://huggingface.co/RuneXX/LTX-2.3-Workflows
- YouTube動画: （動画タイトルを記入）

#### クレジット

- 開発: 老後AI (takejii)
- LTX 2.3 / MSR: Licon Studio
- ワークフロー参考: RuneXX
- LTX2モデル: Lightricks
