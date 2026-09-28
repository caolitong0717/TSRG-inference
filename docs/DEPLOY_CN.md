# TSRG 服务器部署步骤（研究版 v0.1.0-rc1）

本教程面向具有基本终端操作能力、已获授权访问 R19 微调权重的研究者。以 Linux/Bash 和两块 CUDA GPU 为例；实际验证平台为 Kaggle 双 Tesla T4，PyTorch 2.10.0 + Transformers 5.0.0 + PEFT 0.19.1，T4 采用 FP16。普通 Windows 笔记本、单显卡和纯 CPU 模型推理尚未验证。完整安装期间可能需要联网访问 GitHub 和 Hugging Face。

## 1. 获取 TSRG 使用版与官方代码

```bash
git clone https://github.com/caolitong0717/TSRG-inference.git
cd TSRG-inference
mkdir -p upstream models/r19 my_photos outputs
git clone https://github.com/KangLiao929/Puffin.git upstream/Puffin
test -f upstream/Puffin/Puffin/src/models/radiov3/hf_model.py
```

如果当前仓库仍是 Private，请先由仓库所有者邀请你成为有权访问的成员。上面的 Puffin 仓库结构已按其官方顶层 `Puffin/` 子目录配置。该仓库源码的许可和第三方模型权重应分别遵守。

## 2. 准备 Python 环境及依赖

请先根据[PyTorch 官方安装指南](https://pytorch.org/get-started/locally/)安装适配 GPU 驱动的 **GPU 版 PyTorch 2.10.0**，再在 TSRG-inference 根目录执行：

```bash
python -m pip install -r requirements-inference.txt
python -m tsrg.inference --help
```

当前推理代码对 PyTorch `2.10.0`、Transformers `5.0.0`、PEFT `0.19.1` 执行版本检查，安装不同版本时需以已经验证的版本为准。首次运行会下载公开的 RADIO 结构以及 Qwen 文本配置、分词器；这些资源由原提供方分发。

## 3. 放置两个模型资源

- 从 [Puffin 官方模型页](https://huggingface.co/KangLiao/Puffin)取得 `Puffin-Base.pth`，放到 `models/Puffin-Base.pth`。
- 从项目所有者授权的 R19 原始归档中取出 **Epoch 3** 的 `qwen_lora` 文件夹，放到 `models/r19/epoch_3/qwen_lora/`。确认该文件夹里同时有 `adapter_config.json` 和 `adapter_model.safetensors`。本程序自动检验适配器 SHA256：

```text
1dc8afa37c4947d30f58e9ae3aa260c76730dd7201a042f98fcbfd97ae0da8b0
```

**当前 R19 归档是私有研究资源**。拥有此 GitHub 使用仓库的读取权限，并不自动意味着获得了模型适配器的访问/再分发权限；如未获授权，请先向研究项目所有者申请。基础模型和适配器均未包含在本使用仓库或源代码 ZIP 中。

预期目录结构：

```text
TSRG-inference/
├── tsrg/
├── upstream/Puffin/Puffin/src/models/radiov3/hf_model.py
├── models/Puffin-Base.pth
├── models/r19/epoch_3/qwen_lora/adapter_config.json
├── models/r19/epoch_3/qwen_lora/adapter_model.safetensors
├── my_photos/attraction.jpg
└── outputs/
```

`models/` 和 `upstream/` 是用户自行创建的本地目录，不随代码仓库提供。

## 4. 运行一张新照片

将自己的照片放在 `my_photos/attraction.jpg`，根据照片及其元数据选择 `event`、`heritage`、`natural`、`purpose_built` 之一。例如历史遗产：

```bash
python -m tsrg.inference \
  --image "my_photos/attraction.jpg" \
  --category heritage \
  --puffin-source "upstream/Puffin/Puffin" \
  --base-checkpoint "models/Puffin-Base.pth" \
  --adapter-dir "models/r19/epoch_3/qwen_lora" \
  --output "outputs/attraction_camera.json"
```

输出 JSON 的 `roll_deg`、`pitch_deg`、`square_vfov_deg` 单位都是度。照片类别目前由用户指定，程序不会自动识别景点；所有类别的 Roll 与方形裁剪 FoV 使用 M0，活动/遗产的 Pitch 使用 R19，自然/专门建造类的 Pitch 使用 M0。

## 5. 代码与打包自检

```bash
python -m pip install -r requirements-dev.txt
python -m pytest -q
python tools/build_download_zip.py
```

最终一条命令制作可校验的精简代码 ZIP，模型资源和研究历史档案不会纳入其中。已验证过的独立 GPU 新照片样例请见 [验收记录](INFERENCE_QUICKSTART_CN.md)，许可界限见 [第三方来源](../THIRD_PARTY_NOTICES.md)。如对外引用模型效果，请使用研究中原有的冻结 R20 V3 统计与其限定口径，不要用单张示例输出代替预测精度评估。
