# Qwen Image 2.1 GGUF Multi-Queue Workflows for ComfyUI

Ready-to-use Text-to-Image (T2I) and multi-reference Image-to-Image (I2I) workflows with automatic seed randomization for continuous queued generation in ComfyUI.

These workflows were tested on an NVIDIA GeForce RTX 4060 Laptop GPU with 8 GB VRAM using the `Q4_K_M` GGUF model. They process images one at a time instead of creating a large image batch, which is safer on low-VRAM systems.

日本語の説明は[こちら](#日本語)です。

## T2I example

![T2I generated image and embedded workflow](previews/Qwen-Image-2.1-T2I-Workflow-Share.png)

The PNG above is the actual output of the published T2I workflow and contains its ComfyUI workflow metadata. Its embedded generation prompt is identical to the default prompt in `Qwen-Image-2.1-T2I-GGUF-Multi-Queue.json`:

```text
日本のアニメスタイル。
公園の噴水の前でショートワンピースを着て立っている一人の女の子。
笑顔で手を振っている。天気は快晴。
```

The browser screenshot below shows the T2I multi-queue workflow loaded in ComfyUI. The queue count is set to `4`; each queued run is processed sequentially with a newly randomized seed.

<img width="1536" height="720" alt="ComfyUI T2I multi-queue workflow in browser" src="https://github.com/user-attachments/assets/7a2b40c1-1def-4345-8239-be22258d5d19" />




### T2I作例について

上のPNGは、公開しているT2Iワークフローで実際に生成した画像です。ComfyUIのワークフローメタデータも埋め込まれています。画像内の生成プロンプトと、公開JSONの初期プロンプトが一致することを確認済みです。

ブラウザ画面ではキュー回数を`4`に設定しています。4枚を同時処理するのではなく、Seedを毎回ランダム化しながら1枚ずつ順番に生成します。

## Features

- T2I and multi-reference I2I workflows
- Automatic seed randomization after every queued generation
- Works with ComfyUI's queue count for continuous generation
- Configured and tested for an 8 GB VRAM environment
- Uses the memory-friendly `Q4_K_M` quantization by default
- Includes a PNG with embedded ComfyUI workflow metadata

## Included workflows

- `workflows/Qwen-Image-2.1-T2I-GGUF-Multi-Queue.json`
- `workflows/Qwen-Image-2.1-I2I-GGUF-Multi-Queue.json`

## Requirements

- ComfyUI 0.37.0 or newer
- [leejet/ComfyUI-GGUF](https://github.com/leejet/ComfyUI-GGUF) with `qwen_image21` support
- [Qwen Image 2.1 GGUF models](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF)

Place the required files as follows:

```text
ComfyUI/
└── models/
    ├── diffusion_models/
    │   └── qwen-image-2.1-UC-Q4_K_M.gguf
    ├── text_encoders/
    │   └── qwen3vl_8b_int8_convrot.safetensors
    └── vae/
        └── qwen_image_2.1_vae_bf16.safetensors
```

## Model selection

The included workflows use `Q4_K_M` because the setup was designed for and tested on an 8 GB VRAM GPU. This does not mean Q4_K_M is the highest-quality option.

If your system has more available VRAM or RAM, you can select a higher-quality quantization in the `Unet Loader (GGUF)` node.

| Model | Approximate size | Suggested use |
|---|---:|---|
| Q4_K_M | 4.6 GB | Default for this 8 GB VRAM setup |
| Q5_K_M | 5.2 GB | Slightly higher quality |
| Q6_K | 5.9 GB | Systems with more available memory |
| Q8_0 | 7.6 GB | Higher quality and memory usage |

GGUF reduces storage and memory requirements, but generation can still be slow on an 8 GB GPU because model components may be moved between system RAM and VRAM.

## Multi-queue usage

1. Load one of the workflow JSON files in ComfyUI.
2. Enter your prompt and choose the required input images for I2I.
3. Open the extra queue options next to the Queue button.
4. Set the queue/batch count to `2` or more.
5. Queue the workflow.

The added `Primitive Int` node randomizes the seed after each queued execution. Each image is generated sequentially with a different seed. This is multi-queue generation, not simultaneous latent batching.

## I2I image roles

- `image_1`: the primary edit target, pose reference, or composition reference
- `image_2` and later: character, clothing, style, or other visual references
- Refer to them in the prompt as `<image1>`, `<image2>`, and so on

## Sharing the workflow PNG

The PNG in `previews/` contains embedded ComfyUI `workflow` and `prompt` metadata. Download the original PNG file and drag it onto the ComfyUI canvas to load the workflow. Social networks may strip this metadata, so the standalone JSON files are included as a reliable fallback.

## Notes and licenses

- Model weights are not included in this repository.
- Review and follow the license terms of Qwen Image 2.1 and each downloaded model file.
- The workflows are based on the official ComfyUI Qwen Image 2.1 workflow templates and adapted for GGUF and multi-queue use.
- Model and custom-node names remain the property of their respective authors.

---

## 日本語

ComfyUIで連続キュー生成を行うための、Qwen Image 2.1 GGUF対応Text-to-Image（T2I）および複数画像参照Image-to-Image（I2I）ワークフローです。生成のたびにSeedを自動変更します。

このワークフローは、VRAM 8 GBのNVIDIA GeForce RTX 4060 Laptop GPUと`Q4_K_M` GGUFモデルを使用して動作確認しています。大きな画像バッチを一度に処理するのではなく、1枚ずつ順番に生成するため、少ないVRAMでも比較的安全に連続生成できます。

### 主な機能

- T2Iおよび複数画像参照I2Iに対応
- キュー実行ごとにSeedを自動ランダム化
- ComfyUIのキュー回数指定による連続生成
- VRAM 8 GB環境で設定・動作確認済み
- 標準設定では省メモリな`Q4_K_M`を使用
- ComfyUIワークフロー情報を埋め込んだPNGを同梱

### 必要な環境

- ComfyUI 0.37.0以降
- `qwen_image21`対応の[leejet/ComfyUI-GGUF](https://github.com/leejet/ComfyUI-GGUF)
- [Qwen Image 2.1 GGUFモデル一式](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF)

モデルは以下へ配置してください。

```text
ComfyUI/
└── models/
    ├── diffusion_models/
    │   └── qwen-image-2.1-UC-Q4_K_M.gguf
    ├── text_encoders/
    │   └── qwen3vl_8b_int8_convrot.safetensors
    └── vae/
        └── qwen_image_2.1_vae_bf16.safetensors
```

### モデルの選択

同梱ワークフローは、VRAM 8 GB環境での利用を想定して`Q4_K_M`を選択しています。Q4_K_Mが最高品質という意味ではありません。

VRAMやシステムRAMに余裕がある場合は、`Unet Loader (GGUF)`ノードでQ5_K_M、Q6_K、Q8_0などの高品質な量子化モデルへ変更できます。

| モデル | おおよそのサイズ | 用途の目安 |
|---|---:|---|
| Q4_K_M | 4.6 GB | 今回の8 GB VRAM向け標準設定 |
| Q5_K_M | 5.2 GB | 品質を少し優先したい場合 |
| Q6_K | 5.9 GB | メモリに余裕がある環境 |
| Q8_0 | 7.6 GB | より高品質・高メモリ使用量 |

GGUFによって必要なストレージ容量とメモリは削減されますが、8 GB VRAM環境ではシステムRAMとVRAMの間でモデルを移動するため、生成に時間がかかる場合があります。

### 連続生成の方法

1. ComfyUIでワークフローJSONを開きます。
2. プロンプトを入力し、I2Iでは参照画像を指定します。
3. Queueボタン付近の追加オプションを開きます。
4. キュー回数を`2`以上に設定します。
5. Queueを実行します。

追加した`Primitive Int`ノードが、キュー実行後にSeedを自動ランダム化します。各画像は異なるSeedで1枚ずつ順番に生成されます。同時に複数画像を処理するlatent batch方式ではありません。

### I2Iの画像指定

- `image_1`：編集対象、ポーズ参照、または構図参照
- `image_2`以降：キャラクター、服装、画風などの追加参照
- プロンプトでは`<image1>`、`<image2>`のように指定します

### ワークフロー画像の共有

`previews/`内のPNGには、ComfyUIの`workflow`と`prompt`メタデータが埋め込まれています。GitHubから元のPNGをダウンロードし、ComfyUIのキャンバスへドラッグ＆ドロップするとワークフローを読み込めます。SNSではメタデータが削除される場合があるため、確実な共有用としてJSONも同梱しています。

### 注意事項とライセンス

- モデル本体はこのリポジトリに含まれません。
- Qwen Image 2.1および各モデルファイルのライセンスを確認し、条件に従って使用してください。
- このワークフローはComfyUI公式のQwen Image 2.1テンプレートを基に、GGUFおよびマルチキュー向けに調整しています。
- モデル名・カスタムノード名の権利は各作者に帰属します。

