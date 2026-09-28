# TSRG 使用版 GPU 实测记录

本页只记录新照片上的**软件验收**。历史 R18—R20 V3 正式研究材料和统计结果存于单独的科研仓库，不在使用版复制。

项目所有者在原 Kaggle Notebook 的双 Tesla T4、FP16 环境中分别运行南开大学图书馆（预先设置 `purpose_built`）和寒山寺普明宝塔（预先设置 `heritage`）。随后上传 GitHub 下载版代码，并从该下载包的 `tsrg/inference.py` 独立启动 `python -m tsrg.inference` 处理寒山寺照片。该命令退出码为 `0`，成功保存 JSON，输出如下：

```json
{
  "category": "heritage",
  "roll_deg": 2.337667804133759,
  "pitch_deg": 32.0168815919104,
  "square_vfov_deg": 29.06614894698666,
  "sources": {
    "roll": "m0",
    "pitch": "r19",
    "square_vfov": "m0"
  }
}
```

同一张照片在原 Notebook 的 FP16 结果与上述输出一致，验证了独立程序在已测试环境中的模型加载、参数解析和预设路由。照片没有真实拍摄参数标签，该样例不能用于评价预测误差或宣称已复现正式 R20 的 BF16 指标。用户版需要使用者自行指定类别，输出相机参数，不涉及多视角图像生成。

运行所需版本为 PyTorch 2.10.0、Transformers 5.0.0、PEFT 0.19.1；Vision 使用 cuda:0，语言模块使用 cuda:1。未测试单 GPU 或 CPU 大型模型推理。纯 CPU 契约测试可以用 `python -m pip install -r requirements-dev.txt` 和 `python -m pytest -q` 执行。