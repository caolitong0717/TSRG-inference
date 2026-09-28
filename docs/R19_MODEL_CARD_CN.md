# R19 Epoch-3 LoRA 模型卡（公开发布前草稿）

> **发布状态：未授权公开分发，当前没有公开权重下载地址。** 此页说明拟单独随公开软件发布的研究适配器身份，不代表同意任何形式的未经授权转载。公开发布时需按审核结果补齐作者署名、许可条款、权重地址和 Release SHA256。

## 模型身份及功能

- 所属软件：TSRG — Tourism-aware Spatial Reasoning and Geometry；研究用新照片相机参数预测。
- 适配器身份：R19 M1-v4 full-development pitch specialist（LoRA），Epoch 3 在正式 test239 评估之前冻结。R18/R19 原始研发和 R20 正式结果保留在私有科研档案。
- 训练协议保存记录：primary 526、auxiliary 216、总计 742，训练 3 个 epoch；Epoch 3 事先锁定，未根据已揭示的 test239 结果重新挑选模型。
- 基底：Puffin-Base 的 Qwen2.5-1.5B 语言模型部分；R19 使用 q/k/v/o 投影层 LoRA，`r=8`、`lora_alpha=16`、`lora_dropout=0.05`，PEFT `0.19.1`。该 LoRA 不能单独运行，需要官方 Puffin-Base 的代码和权重。
- 文件：`epoch_3/qwen_lora/adapter_model.safetensors`（校验值见下文）和 `epoch_3/qwen_lora/adapter_config.json`。原始 JSON 曾记录训练时 Kaggle 本地绝对路径；公开版是否需要独立清理配置，须在新照片运行验证之后决定。
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

## 权利及引用边界（待补齐）

- [Puffin 上游源码与 S-Lab License 1.0](https://github.com/KangLiao929/Puffin/blob/main/LICENSE) 含非商业用途及通知要求；[官方基础模型](https://huggingface.co/KangLiao/Puffin)由原作者分发。
- [Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) 标注 Apache-2.0；[C-RADIOv3-H](https://huggingface.co/nvidia/C-RADIOv3-H) 采用 NVIDIA Open Model License。
- 本适配器的共同作者、训练数据使用权限、适用许可与具体公开分发方式尚待项目所有者及相关权利人明确确认；不得将 TSRG 自有工具代码的 MIT 范围直接套用至原始基础模型或 LoRA。

**计划的交付位置（待审核后执行）**：用户仓库单独的 GitHub Release，包含 R19 Epoch-3 的两个必要文件（或无损文件 ZIP）、摘要清单和本模型卡，不上传科研档案库中的历史训练数据、优化器检查点及原始旅游照片。
