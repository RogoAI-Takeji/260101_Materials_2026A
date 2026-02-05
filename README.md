# RogoAI Materials 2026A (Jan-Jun)



---



## LTX2 Sound-to-Video Workflow



### LTX2\_S2V\_GGUF\_12GB.json

- \*\*公開日\*\*: 2026年1月28日

- \*\*説明\*\*: LTX2でSound-to-Videoを実現するComfyUIワークフロー

- \*\*モデル\*\*: LTX-AV GGUF 12GBモデル対応

- \*\*Civitai版を修正\*\*: 修正版ワークフロー



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

- ltx-av-step-1751000\_vocoder\_24K.safetensors

- gemma-3-12b-it-qat-q4\_0-unquantized\_readout\_proj



#### 使い方

1\. ComfyUIを起動

2\. 「Load」からこのJSONファイルを読み込む

3\. 音声ファイルを準備（Qwen3-TTS等で生成）

4\. プロンプトを記入

5\. フレーム数を設定（25fps × 秒数）

6\. Queueボタンで生成開始



#### 注意事項

- \*\*重要\*\*: LTX2は音声生成機能を持たないため、音声は別途TTSで生成する必要があります

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

