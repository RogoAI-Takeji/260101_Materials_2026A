# RogoAI Materials 2026A (Jan-Jun)



---



\## LTX2 Sound-to-Video Workflow



\### LTX2\_S2V\_GGUF\_12GB.json

\- \*\*公開日\*\*: 2026年1月28日

\- \*\*説明\*\*: LTX2でSound-to-Videoを実現するComfyUIワークフロー

\- \*\*モデル\*\*: LTX-AV GGUF 12GBモデル対応

\- \*\*Civitai版を修正\*\*: 修正版ワークフロー



\#### 機能

\- 音声ファイル（Qwen3-TTS等で生成）からリップシンク動画を生成

\- 複数人会話対応（声ごとに顔を指定可能）

\- 25fps、最大13秒（326フレーム）の動画生成

\- 10分で13秒の動画を生成



\#### 必要なノード

\- LTXVGemmaCLIPModelLoader

\- LTXVAudioVAELoader

\- MultimodalGuider

\- EmptyLTXVLatentVideo

\- LTXVEmptyLatentAudio

\- LTXVConditioning

\- LTXVConcatAVLatent

\- LTXVSeparateAVLatent



\#### 使用モデル

\- ltx-av-step-1751000\_vocoder\_24K.safetensors

\- gemma-3-12b-it-qat-q4\_0-unquantized\_readout\_proj



\#### 使い方

1\. ComfyUIを起動

2\. 「Load」からこのJSONファイルを読み込む

3\. 音声ファイルを準備（Qwen3-TTS等で生成）

4\. プロンプトを記入

5\. フレーム数を設定（25fps × 秒数）

6\. Queueボタンで生成開始



\#### 注意事項

\- \*\*重要\*\*: LTX2は音声生成機能を持たないため、音声は別途TTSで生成する必要があります

\- VRAMは12GB以上推奨（GGUF版使用）



\#### 参考

\- Civitai版LTX2 Workflow

\- FlybirdXX氏のComfyUI-Qwen3-TTS

\- YouTube動画: 【Qwen3-TTS ComfyUI】エラー完全解決！Coqui超え日本語音声＋LTX2複数人会話



\#### クレジット

\- 開発: 老後AI (takejii)

\- ComfyUI-Qwen3-TTS: FlybirdXX

\- LTX2モデル: Lightricks



