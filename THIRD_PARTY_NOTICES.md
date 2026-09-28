# 第三方来源与分发权限

本独立代码仓库没有携带 Puffin 基础权重、上游源代码、R19 LoRA 二进制、GeoCalib 模型、原始照片或训练数据。R19 LoRA 通过 [v0.1.0-rc1 Release](https://github.com/caolitong0717/TSRG-inference/releases/tag/v0.1.0-rc1) **独立下载附件**提供，而非提交进源码仓库；各组件均保留原版权与授权边界。

- [Puffin 官方仓库](https://github.com/KangLiao929/Puffin) 使用 S-Lab License 1.0，涉及非商业用途、版权声明等条款；请核对[上游许可原文](https://github.com/KangLiao929/Puffin/blob/main/LICENSE)。TSRG 并非 Puffin 官方产品。
- [Puffin 官方基础模型](https://huggingface.co/KangLiao/Puffin) 遵循原作者模型页及其 S-Lab 相关条件；[Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) 模型卡标注 Apache-2.0；[NVIDIA C-RADIOv3-H](https://huggingface.co/nvidia/C-RADIOv3-H) 采用 NVIDIA Open Model License。这些许可互不替代，用户自行从官方获取相应资源。
- 原始研究中使用过的 [GeoCalib](https://github.com/cvg/GeoCalib) 属于历史研究来源；该独立推理包未收录其文件。
- R19 Epoch-3 LoRA 属于项目开发者同意公开的研究适配器，现以独立非商业研究用附件提供；其分发条件独立于本仓库的自有程序代码，且须遵守 Puffin 等第三方来源的相关条款。

本仓库的 MIT 许可仅覆盖 LICENSE 明确列出的自有代码。项目开发者已授权发布自有成果；研究素材来源与权重完整性已有审计记录。第三方资源仍须遵守其原条款。
项目开发者已明确同意公开自己的代码及 R19 适配器；非商业许可及第三方权利边界仍须保留。**R19 权重已作为 Pre-release 附件发布，仓库转 Public 前仍需仓库读取权限。**模型身份与审计记录见 [R19 模型卡](docs/R19_MODEL_CARD_CN.md) 和 [公开授权审核记录](docs/PUBLIC_RELEASE_AUDIT_CN.md)。
