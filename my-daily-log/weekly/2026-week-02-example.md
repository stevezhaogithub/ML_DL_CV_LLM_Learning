# 周报 - 第 02 周 (01-06 ~ 01-12)

## 🎯 本周目标
- [x] 完成 PyTorch 基础教程学习
- [x] 搭建本地深度学习开发环境
- [ ] 完成 CNN 图像分类项目（延期至下周）

## 📊 任务完成情况

| 任务 | 状态 | 备注 |
|------|------|------|
| PyTorch 官方教程 1-5 章 | ✅ | 已完成笔记整理 |
| 配置 CUDA 11.8 + cuDNN | ✅ | RTX 4090 测试通过 |
| 阅读 ResNet 论文 | ✅ | 重点理解残差连接 |
| CNN 图像分类项目 | 🚧 | 完成 60%，数据预处理已完成 |
| LLM 微调实验 | ❌ | 本周时间不足，取消 |

## 📚 本周学习

### 深度学习
- **主题**：PyTorch 基础 + CNN 原理
- **时长**：12 小时
- **成果**：
  - 掌握 Tensor 操作、自动求导机制
  - 理解 DataLoader 和 Dataset 使用方法
  - 完成 MNIST 手写数字识别 Demo

### 论文阅读
- **ResNet**: Deep Residual Learning for Image Recognition
- **关键收获**：残差连接解决深层网络退化问题

## 💻 代码产出
- [mnist_classifier.py](./code/mnist_classifier.py) - 手写数字分类
- [data_augmentation.py](./code/data_augmentation.py) - 数据增强工具

## 📈 本周总结
- **完成率**：3/5 (60%)
- **收获**：
  - PyTorch 基础扎实，可以独立搭建简单网络
  - 深度学习环境配置经验 +1
- **不足**：
  - 时间管理不够好，CNN 项目进度落后
  - 论文阅读速度需要提升

## 🐛 遇到的问题
1. CUDA 版本与 PyTorch 不兼容 → 重新安装对应版本解决
2. DataLoader 多进程在 Windows 报错 → 设置 `num_workers=0`

## 📌 下周计划
- [ ] 完成 CNN 图像分类项目
- [ ] 学习 Transformer 架构基础
- [ ] 阅读 Attention Is All You Need 论文
- [ ] 整理本周学习笔记到博客

## 💡 想法与备忘
> 考虑尝试 Hugging Face 的 transformers 库，为后续 LLM 学习做准备。
> 
> 下周需要更合理分配时间，优先完成核心任务。
