# TSRG-inference 发布来源与审核记录

本文件汇总公开使用版的已执行审查结果及其范围。原科研档案库 `TSRG` 不因本使用版发布而公开；训练照片、完整实验文件与原始测试明细均不属于公开包。

## 开发者授权与第三方边界

2026-09-28，项目开发者明确确认 TSRG 由其开发，并同意公开自有推理代码和 R19 Epoch-3 LoRA。Puffin 上游源码采用 [S-Lab License 1.0](https://github.com/KangLiao929/Puffin/blob/main/LICENSE)，载有非商业使用和再分发条件；模型适配器随附上游许可声明，不将 Puffin-Base 本身重打包。使用者须从 [Puffin 官方](https://huggingface.co/KangLiao/Puffin)取得基础权重，遵守相应版权与许可。Qwen 和 NVIDIA RADIO 组件各保留自身许可。本仓库限定范围的 MIT 许可不涵盖这些第三方资源。

## 训练素材来源记录检查

原始 Kaggle 四份元数据文件在私有环境中执行跨表核验：图片来源和归属表各 1,425 条；R19 组合训练记录 742 条，分为 526 条主训练和 216 条辅助投影训练。526 条主样本均与原始图片清单关联；216 条辅助记录对应两张经来源和作者记录的原始全景，各产生 108 条投影；比对字段中未发现缺失或不一致。原记录的 CC 授权类型属于已记录的 CC0、公有领域、CC BY 和 CC BY-SA 范围，附有署名、来源与许可链接。元数据核验不等于重新下载所有原图、检查每张图片当前页面，或作出全部衍生使用合法性的保证。原始元数据含内部路径，不作为公开附件。

## 冻结模型与发布包核验

原始 R19 Epoch-3 LoRA `adapter_model.safetensors`：8,745,704 bytes，SHA256 `1dc8afa37c4947d30f58e9ae3aa260c76730dd7201a042f98fcbfd97ae0da8b0`，与原正式实验前冻结记录一致。原始 `adapter_config.json` 含历史 Kaggle 本地路径，该字段为来源元数据，不是外部部署必需的实际基础模型路径；最终发布包原样保存其字节。现有推理代码显式加载用户提供的 Puffin 基础模型对象，原始配置曾在双 T4 FP16 运行验证。

[v0.1.0-rc1 Pre-release](https://github.com/caolitong0717/TSRG-inference/releases/tag/v0.1.0-rc1) 已附带 `TSRG-R19-Epoch3-LoRA-v0.1.0-rc1.zip`。GitHub 报告的文件大小为 8,020,529 bytes，SHA256 为 `5c277a6a467f0d3e4f8befd53821247aa6c7691ce56e31ed1ecef8964e3ab076`，与本地候选发布包一致。压缩包包含原始 LoRA 参数与配置、校验清单、发布说明和 Puffin 上游许可证；不含原始照片、基础权重、历史优化器和科研档案。

## 推理验证范围与公开后复验

现有独立命令行在 Kaggle 双 Tesla T4、FP16 上对新照片运行成功，GPU 验收与原 Notebook 同图结果一致；GitHub Actions 的 CPU 测试和确定性打包通过。这些软件验收不是新增精度报告，也未重新运行冻结的 R20 V3 BF16 test239。

仓库由 Private 转 Public 后，仍需在未登录环境核验源码和 Release 的公开访问与下载，并建议由独立双 GPU 环境按文档复验。未证实单 GPU、纯 CPU 大模型推理或通用跨硬件可运行性。