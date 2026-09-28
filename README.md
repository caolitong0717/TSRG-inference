# TSRG-inference

**TSRG — Tourism-aware Spatial Reasoning and Geometry | 新照片相机参数预测使用版**

这里存放已通过实际 GPU 验收的独立推理代码，服务于其他研究者下载和使用。原始 R18—R20 V3 训练 Notebook、完整研究记录、正式测试输出以及原始图片保留在独立科研档案库 `TSRG`，不会同步到本使用仓库。

> **状态：v0.1.0-rc1，当前 Private。** 独立命令行已在 Kaggle 双 Tesla T4、FP16 环境中对新照片成功运行，结果与原 Notebook 同图结果一致；这项验证不替代冻结的 R20 V3 BF16 正式测试，也没有新增准确性结论。当前没有图形化网页、多视角图片生成或自动景点分类功能。

## 它能做什么

输入一张新的旅游照片和明确的景点类别，输出 Roll（左右倾斜角）、Pitch（上下俯仰角）与中心方形裁剪垂直 FoV（视场角），全部使用角度制。按冻结的参数路由：Roll 和 FoV 始终取 M0，`event` / `heritage` 的 Pitch 取 R19，`natural` / `purpose_built` 的 Pitch 取 M0。类别由使用者提供。

## 如何下载、安装和运行

请从 [保姆级中文使用说明](DOWNLOAD_README_CN.md) 开始；需要完整的服务器部署命令及文件夹示意，可直接查看 [两块 GPU 部署教程](docs/DEPLOY_CN.md)。准备两块 CUDA GPU，以及从官方获得的 Puffin 源码和基础模型 `Puffin-Base.pth`；另需取得授权访问的 R19 Epoch-3 LoRA 适配器（程序检查 SHA256）。GPU 版 PyTorch 应按设备驱动单独安装，随后安装本仓库 `requirements-inference.txt`。已验证版本为 PyTorch 2.10.0、Transformers 5.0.0、PEFT 0.19.1。

在本仓库根目录运行命令（下列路径是示例，需根据实际文件位置修改）：

```bash
python -m tsrg.inference \
  --image "my_photos/attraction.jpg" \
  --category heritage \
  --puffin-source "upstream/Puffin/Puffin" \
  --base-checkpoint "models/Puffin-Base.pth" \
  --adapter-dir "models/r19/epoch_3/qwen_lora" \
  --output "outputs/attraction_camera.json"
```

如需查看参数说明，运行 `python -m tsrg.inference --help`。当前程序必须使用两块 CUDA GPU，单 GPU 或纯 CPU 的大型模型推理未做验收。

## 下载精简 ZIP / 检查代码

在本仓库根目录执行：

```bash
python -m pip install -r requirements-dev.txt
python -m pytest -q
python tools/build_download_zip.py
```

最后一项生成 `dist/TSRG-inference-v0.1.0-rc1.zip`，内部附有 `MANIFEST_SHA256.txt` 校验单。GitHub Actions 也会自动构建供有权限成员下载的工作流 Artifact。精简包包含代码、说明、依赖和测试，不携带原始第三方模型、R19 权重、训练数据及旅游图片。

**计划公开模式**：最终提供代码与 R19 Epoch-3 LoRA 两个独立下载入口。授权审查进行中，当前仓库与 R19 权重均未公开；使用者在获得适配器授权前仍不能完成全部推理部署。

相关说明：[GPU 验收记录](docs/INFERENCE_QUICKSTART_CN.md) · [R19 模型卡（待授权）](docs/R19_MODEL_CARD_CN.md) · [公开授权审核记录](docs/PUBLIC_RELEASE_AUDIT_CN.md) · [公开前审核要求](docs/RELEASE_GATE_CN.md) · [第三方来源](THIRD_PARTY_NOTICES.md) · [独立代码许可](LICENSE)。