# 第三方来源与分发权限

本独立下载版没有携带 Puffin 基础权重、上游源代码、R19 LoRA 二进制、GeoCalib 模型、原始照片或训练数据。各组件均保留原版权与授权边界。

- [Puffin 官方仓库](https://github.com/KangLiao929/Puffin) 使用 S-Lab License 1.0，涉及非商业用途、版权声明等条款；请核对[上游许可原文](https://github.com/KangLiao929/Puffin/blob/main/LICENSE)。TSRG 并非 Puffin 官方产品。
- [Puffin 官方基础模型](https://huggingface.co/KangLiao/Puffin) 遵循原作者模型页及其 S-Lab 相关条件；[Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) 模型卡标注 Apache-2.0；[NVIDIA C-RADIOv3-H](https://huggingface.co/nvidia/C-RADIOv3-H) 采用 NVIDIA Open Model License。这些许可互不替代，用户自行从官方获取相应资源。
- 原始研究中使用过的 [GeoCalib](https://github.com/cvg/GeoCalib) 属于历史研究来源；该独立推理包未收录其文件。
- R19 Epoch-3 LoRA 属于本项目的研究适配器，需经许可的渠道单独取得，其分发权限独立于本仓库的自有程序代码。

本仓库的 MIT 许可仅覆盖 LICENSE 明确列出的自有代码。将本仓库设为 Public 或向外分发模型文件之前，应完成共同作者、来源、图片与第三方授权核验。
本仓库准备将自有推理代码与 R19 Epoch-3 LoRA 作为两个可公开获取的交付物；但 **R19 权重当前尚未取得明确的公开分发确认，仍未上传到公开位置**。模型身份与待核实事项见 [R19 模型卡](docs/R19_MODEL_CARD_CN.md) 和 [公开授权审核记录](docs/PUBLIC_RELEASE_AUDIT_CN.md)。
