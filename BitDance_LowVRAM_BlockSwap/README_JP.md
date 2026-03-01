# BitDance 14B - 低VRAM ブロックスワップ版

16GB以下のVRAMでBitDance 14Bを動かすためのComfyUIカスタムノード（nodes.py）です。

## 解説動画

[![老後AI - BitDance BlockSwap](https://img.youtube.com/vi/kXjmxIa8l5Y/0.jpg)](https://youtu.be/kXjmxIa8l5Y)

https://youtu.be/kXjmxIa8l5Y

## 概要

BitDance 14Bは通常、テキストエンコーダーだけで18GB以上のVRAMを必要とするため、24GB未満のGPUでは動作しません。
この改造版`nodes.py`は**ブロックスワップ**技術を実装し、16GB VRAM（RTX 4080等）での動作を可能にしました。
`num_gpu_blocks`を調整することで12GBや8GBでも動作する可能性があります。

### 検証結果（RTX 4080 16GB）
- 生成時間：約13分（ブロックスワップなしの40分から短縮）
- VRAMピーク：約10.6GB
- `num_gpu_blocks`：15（16GB環境での最適値）

## ブロックスワップの仕組み

40層で構成されるLLMトランスフォーマーをCPU RAMに全層保持します。
推論時、各ブロックを順番に CPU → GPU（計算）→ CPU（結果書き戻し）とストリーミング処理します。
KVキャッシュもブロック間はCPUにオフロードしてVRAMを解放します。

| VRAM | 推奨 num_gpu_blocks |
|------|-------------------|
| 16GB | 15（検証済み） |
| 12GB | 10（未検証） |
| 8GB  | 5（未検証） |

## ファイル構成

| ファイル | 説明 |
|---------|------|
| `nodes.py` | ブロックスワップ実装済みカスタムノード |
| `BitDance_14B_64x_portrait.json` | 縦型ポートレート生成ワークフロー（768×1280） |

## 動作環境

- GPU：16GB VRAM検証済み（RTX 4080）。それ以下のVRAMも調整次第で動作可能性あり
- RAM：64GB推奨（テキストエンコーダーをCPUに展開）
- ComfyUI本体 + [ComfyUI-BitDance](https://github.com/shallowdream204/BitDance) インストール済み

## 必要なモデル

以下をダウンロードし、ComfyUIの`models/bitdance/`フォルダに配置してください：

- `BitDance_14B_MainModel_FP8.safetensors`
- `BitDance_TextEncoder_FP8.safetensors`
- `BitDance_VAE_FP16.safetensors`

※VRAMを抑えるためFP8版の使用を推奨します。

## 使い方

> **重要：** `nodes.py`を上書きする前に、必ずオリジナルファイルのバックアップを取ってください。
> `nodes.py.bak`にリネームするか、別の場所にコピーを保存してから作業してください。

1. `custom_nodes/Comfyui-bitdance/`内の元の`nodes.py`を`nodes.py.bak`にリネームしてバックアップ
2. この`nodes.py`を`custom_nodes/Comfyui-bitdance/`にコピー
3. ComfyUIを再起動
4. `BitDance_14B_64x_portrait.json`をComfyUIに読み込む
5. BitDanceSamplerノードの`num_gpu_blocks`を15に設定（VRAMに合わせて調整）
6. 実行

## ライセンス

Apache 2.0（オリジナルのBitDanceリポジトリと同じ）

## クレジット

- オリジナルBitDance：[shallowdream204/BitDance](https://github.com/shallowdream204/BitDance)
- ブロックスワップ実装：[RogoAI-Takeji](https://github.com/RogoAI-Takeji) / YouTube：[老後AI (RogoAI)](https://youtu.be/kXjmxIa8l5Y)
