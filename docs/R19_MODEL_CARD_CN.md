# R19 Epoch-3 LoRA 模型卡（公开发布前草稿）

> **发布状态：项目开发者已明确同意公开其自有 R19 研究适配器；尚未创建公开 Release，也未改变使用仓库的 Private 状态。** 发布仍须遵守 Puffin 等上游组件的许可条件。此页为发布前模型卡，不能将 TSRG 自有推理代码的 MIT 许可理解为基础模型的许可。

## 模型身份及功能

- 所属软件：TSRG — Tourism-aware Spatial Reasoning and Geometry；研究用新照片相机参数预测。
- 适配器身份：R19 M1-v4 full-development pitch specialist（LoRA），Epoch 3 在正式 test239 评估之前冻结。R18/R19 原始研发和 R20 正式结果保留在私有科研档案。
- 训练协议保存记录：primary 526、auxiliary 216、总计 742，训练 3 个 epoch；Epoch 3 事先锁定，未根据已揭示的 test239 结果重新挑选模型。
- 基底：Puffin-Base 的 Qwen2.5-1.5B 语言模型部分；R19 使用 q/k/v/o 投影层 LoRA，`r=8`、`lora_alpha=16`、`lora_dropout=0.05`，PEFT `0.19.1`。该 LoRA 不能单独运行，需要官方 Puffin-Base 的代码和权重。
- 文件：`epoch_3/qwen_lora/adapter_model.safetensors`（校验值见下文）和 `epoch_3/qwen_lora/adapter_config.json`。原始 JSON 保留训练时 Kaggle 本地绝对路径，独立推理代码已经通过向 PEFT 显式传入基础模型对象加载原始文件的 GPU 实测。拟发布包保留该配置原样以保证冻结工件可追溯；发布说明明确该路径是历史元数据，并非外部部署地址。
- 程序用途：用于 `event` 和 `heritage` 类新照片的 Pitch 预测；最终 Roll 和中心方形裁剪垂直 FoV 始终使用 M0。其他类别的 Pitch 也使用 M0。类别由使用者指定。

## 冻结适配器校验

原科研档案 `R19_final_model_lock.json` 与后续 R20 验证时确认的原始二进制摘要：

```text
SHA256(adapter_model.safetensors):
1dc8afa37c4947d30f58e9ae3aa260c76730dd7201a042f98fcbfd97ae0da8b0
```

发布前应对拟上传的**实际文件字节**重新校验；文件名一致并不足以保证模型相同。模型代码会拒绝摘要不匹配的适配器。

## 运行验证及局限

- 已在双 Tesla T4、FP16 下实际运行 GitHub 下载版独立程序；寒山寺普明宝塔新照片、类别 `heritage` 的输出为 Roll `2.337667804133759°`（M0），Pitch `32.0168815919104°`（R19），方形垂直 FoV `29.06614894698666°`（M0）。
- 此项测试检查软件能否正确加载并路由，照片没有真实相机参数标签，不能将输出等同于准确性证据；原正式 R20 V3 的 BF16 test239 指标需按原论文和协议独立理解。
- 未验证普通笔记本、单 GPU、纯 CPU 的大模型推理，也不提供景点类别自动分类或图像生成功能。

## 权利及引用边界

- [Puffin 上游源码与 S-Lab License 1.0](https://github.com/KangLiao929/Puffin/blob/main/LICENSE) 含非商业用途及通知要求；[官方基础模型](https://huggingface.co/KangLiao/Puffin)由原作者分发。
- [Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) 标注 Apache-2.0；[C-RADIOv3-H](https://huggingface.co/nvidia/C-RADIOv3-H) 采用 NVIDIA Open Model License。
- TSRG 项目开发者于 2026-09-28 在会话中明确确认其开发成果并同意公开。素材来源交叉表结构核验已通过，但这不取代对第三方条款的遵守。适配器发布按非商业研究用途说明，不得将 TSRG 自有工具代码的 MIT 范围直接套用至 Puffin 基础模型或外部组件。

**拟交付位置（尚未发布）**：本使用仓库 GitHub Release；本轮已经制作本地私有审核包 `TSRG-R19-Epoch3-LoRA-v0.1.0-rc1_PRIVATE-REVIEW.zip`，包 SHA256 `91119dd6408d9108e553ba7857d512f02d8b159c7c0021b6309f3d1c9386ef56`。内含原样的两个适配器文件、SHA256 校验单、发布说明及完整 Puffin 上游许可，不含科研档案中的训练数据、优化器权重和旅游图片。正式公开后更新实际 Release URL 和文件摘要。
