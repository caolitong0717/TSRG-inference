# TSRG-inference 公开代码与 R19 权重发布审核记录

**拟采用的发布方式**：最终公开本独立使用仓库的代码，并公开经校验的 R19 Epoch-3 LoRA 适配器；正式对外公开须先核实权利及共同作者意见。用户明确选择了这个方向，**尚未授权跳过审核或立即改变 Private 状态**。原始研究档案仓库 `TSRG` 保持 Private，完整 R18/R19/R20 V3 Notebook、正式预测明细、原始训练图片均不得随用户版同步。

## 目前可以凭文件或官方来源核实的事项

| 项目 | 已有证据 | 当前核验结论 |
|---|---|---|
| 代码包 | 使用仓库包含独立 `tsrg/inference.py`、路由、单元测试、README 与精简 ZIP 白名单；GitHub Actions 成功执行单元测试、打包与校验 | 技术打包已经通过；GitHub 公开权限仍未开启 |
| GPU 运行 | 项目所有者在 Kaggle 双 Tesla T4、FP16 中从下载版独立运行寒山寺新照片，退出码 0、JSON 结果与同图 Notebook 一致 | 验证了新照片独立推理入口，未复现冻结 R20 BF16 test239 |
| R19 权重身份 | 原科研档案 `R19_final_model_lock.json` 标记 `FROZEN_BEFORE_TEST239`、预锁定 Epoch 3；适配器 SHA256 `1dc8afa37c4947d30f58e9ae3aa260c76730dd7201a042f98fcbfd97ae0da8b0` | 可以定位明确的、经过原实验校验的权重。公开前须再次在拟发布文件上校验摘要 |
| R19 配置 | 原始 `adapter_config.json` 记载 LoRA `r=8`、`alpha=16`，目标模块 `q_proj/k_proj/v_proj/o_proj`，PEFT `0.19.1` | 适配器依赖 Puffin-Base 内的 Qwen 语言模型；它不是完整独立基础模型 |
| Puffin 上游 | [官方代码许可证](https://github.com/KangLiao929/Puffin/blob/main/LICENSE) 为 **S-Lab License 1.0**，保留版权及条款时允许非商业源代码/二进制再分发，商业使用或再分发应联系贡献者；[官方权重说明](https://huggingface.co/KangLiao/Puffin)另需遵守 | 公开下载应清楚写明非商业条件及来源，不得把 Puffin 基础权重错误标成 TSRG MIT 资源。公开微调适配器是否满足全部上游、数据及合作协议，需要具体核实 |
| Qwen 组件 | [Qwen2.5-1.5B-Instruct 官方模型页](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) 标注 **Apache-2.0** | 记录作者与原始模型许可，不能取代 Puffin 和训练材料所适用的条件 |
| NVIDIA RADIO 组件 | [C-RADIOv3-H 官方模型页](https://huggingface.co/nvidia/C-RADIOv3-H) 标注 **NVIDIA Open Model License** | 程序运行时引用外部组件，权重不在本仓库重新分发；部署者仍须遵守其条款 |

以上是研究文件和官方网页目前提供的事实记录，**不构成完整的法律许可意见或所有权确认**。

## 需要项目所有者或权利人补齐、目前无法仅靠仓库判断的事项

- [ ] **共同作者及单位要求**：明确 TSRG 源码作者、R19 训练贡献者的署名和公开许可，以及论文投稿、成果归属或单位知识产权规定是否需要审批；保留可追溯确认记录。
- [ ] **R19 训练数据的使用与输出发布权**：查明 526 个 primary 和 216 个 auxiliary 样本及其图像/描述/伪标签的来源、许可、下载约定、是否含隐私或需保密内容。仅上传 LoRA、没有上传原图，也不能自动推断衍生权重可以公开。
- [ ] **Puffin 衍生模型范围**：核对基础模型及模型卡的完整条款、是否需要上游作者同意、是否有额外非商业、署名、再分发条件；如不明确，取得作者或单位的书面确认。
- [ ] **R19 二进制发布前检查**：从冻结原始归档取出 `epoch_3/qwen_lora/adapter_model.safetensors` 和对应 `adapter_config.json`；检查 SHA256、是否含可识别的原始样本/凭据及配置中的本地绝对路径。对外提供原始文件，保留校验；如为去路径而调整 JSON，说明这是单独的公开配置副本，不能伪称逐字原始文件。
- [ ] **公开范围和说明**：将 `TSRG-inference` 单独设为 Public；保留原科研档案仓库 Private。准备 R19 单独下载链接、README 模型获取指引、适配器使用边界及必要版权声明。公开发布的 Github Release 文件名与 SHA256 应在模型卡里明确列出。
- [ ] **最后一次下载测试**：以无科研档案库访问权限的新环境，仅从公开代码、公开 R19 文件与官方 Puffin 来源完成安装及一张新照片推理；检查安装说明中的链接、目录和运行命令确实可用。

## 计划发布的最小内容

**代码仓库**：README、`tsrg/`、`tests/`、必要 `tools/`、依赖、安装指南、作者署名、第三方许可、简洁模型卡、验收记录。**权重**：单独的 R19 Epoch-3 LoRA `adapter_model.safetensors` 与 `adapter_config.json`，以及说明/摘要（只有审核完成后才上传）。**基础权重**：链接到 Puffin 官方，仍由使用者自行下载。**科研投稿档案**：不对外自动公开。

## 发布控制点

最后的 Public 操作或上传 R19 二进制文件，须在上述未完成项获得明确确认后执行。当前通过技术测试不等同于已取得模型的公开分发权。

## 2026-09-28 冻结 R19 实际二进制私有验收记录

用户从原始 Kaggle R19 数据集导出 `TSRG_R19_Epoch3_private_review.zip`，在当前私有会话中按真实字节打开并验证。ZIP 8,017,872 bytes，SHA256 为 `bffb31d320adca19779b4207c37050ad66896b449ffca4c7d60ac7421bcabcaf`，CRC 检查通过，恰好含 `epoch_3/qwen_lora/adapter_model.safetensors`、同目录 `adapter_config.json` 和 `WEIGHT_SHA256.txt` 三个文件，不含原始训练图片及优化器权重，ZIP 路径未发现目录穿越。

原始 `adapter_model.safetensors` 大小 8,745,704 bytes，SHA256 实算 `1dc8afa37c4947d30f58e9ae3aa260c76730dd7201a042f98fcbfd97ae0da8b0`，与冻结 R19 模型锁一致；safetensors header 可解析，224 个张量 / 28 层，全部 F32，覆盖 q/k/v/o LoRA 投影，二进制 data offsets 无空洞、重叠或越界。内附摘要清单与实算值一致。

原始 `adapter_config.json` 是 1,102 bytes，SHA256 `324d0d7ec6b086d06f04e4cef22629139fd86d9f6d5d1268a7b5fb62276c12a1`；配置 r=8、alpha=16、目标 q/k/v/o。其 `base_model_name_or_path` 为原 Kaggle 工作环境路径 `/kaggle/working/tourism_puffin_r19_full_pitch_specialist/qwen_text_assets`；这不是网络访问凭据，但不适合误当成外部部署路径，且须保留原始文件不做字节级修改。代码目前由用户提供 `Puffin-Base.pth` 并显式向 PEFT 提供已经构造的基础模型，尚未针对任何“清理路径后的配置副本”做双 GPU 独立运行验证。若后续发布修改过的配置，要分别标明原始与发布配置的 SHA256，并完成 GPU smoke test。

本轮仅验证 ZIP 完整性及 safetensors 元数据/文件结构，不运行大型模型、不复现 R20 正式 BF16 test239，也不构成权利或公开分发审批。对外正式上传和仓库转 Public 继续保持阻止状态，等待署名、成果归属与第三方许可证复核。

## 2026-09-28 作者授权及发布包准备更新

项目开发者在本项目会话中明确表示 TSRG 为其本人开发，且同意公开自己的推理代码与 R19 LoRA。该事实用于记录项目成果公开意愿，不代表其能替 Puffin、Wikimedia 或其他第三方权利人授予超出原许可的权利；外部使用按非商业研究边界和相应上游条款说明。

已经根据用户实际上传的冻结模型 ZIP 生成私有候选发布附件 `TSRG-R19-Epoch3-LoRA-v0.1.0-rc1_PRIVATE-REVIEW.zip`，大小 8,020,511 bytes，SHA256 `91119dd6408d9108e553ba7857d512f02d8b159c7c0021b6309f3d1c9386ef56`。内含字节原样的 `adapter_model.safetensors`（8,745,704 bytes；SHA256 `1dc8afa37c4947d30f58e9ae3aa260c76730dd7201a042f98fcbfd97ae0da8b0`）、原始 `adapter_config.json`、校验清单、README 和上游 Puffin S-Lab License 1.0 全文。ZIP CRC、文件白名单、解压路径安全和二进制/配置逐字节一致性测试通过。该文件是当前会话本地交付物，**尚未提交到 GitHub Release**。原配置的 Kaggle 路径是训练时元数据；现有推理代码明确向 PEFT 传入构造好的基础模型对象，原配置加载曾在双 T4 FP16 运行通过，本轮没有重新运行 GPU。

剩余操作是将已有的候选附件作为单独 Release Asset 上传、填写正式下载地址、核对无权限的新账户下载与 GPU 安装情况，最后才把独立使用仓库转 Public（原科研档案继续 Private）。
