# MinerU Dev Container 使用指南

## 概述

这个 dev-container 配置为 MinerU 项目提供了完整的开发环境，包括：

- 🐍 Python 3.11 环境
- 🚀 CUDA 支持（如果可用）
- 📦 预安装的 MinerU 依赖
- 🛠️ 开发工具和扩展
- 📚 模型下载和配置
- 🧪 测试和调试环境

## 快速开始

### 1. 启动 Dev Container

1. 在 VS Code 中打开 MinerU 项目
2. 按 `Ctrl+Shift+P` 打开命令面板
3. 选择 "Dev Containers: Reopen in Container"
4. 等待容器构建完成

### 2. 首次设置

容器启动后会自动运行设置脚本，包括：
- 检查 CUDA 可用性
- 设置常用别名
- 创建配置文件

## 可用命令

### MinerU 相关
```bash
# 查看 MinerU CLI 帮助
mineru-dev --help

# 启动 SGLang 服务器
mineru-serve

# 查看 MinerU 版本
mineru-version

# 下载模型
mineru-models-download -s huggingface -m all
```

### 开发工具
```bash
# 运行测试
mineru-test

# 代码格式化和检查
mineru-lint

# 清理缓存文件
mineru-clean

# 查看 GPU 信息
gpu-info
```

### 示例用法
```bash
# 处理单个 PDF 文件
mineru-dev demo/demo.pdf -o output.md

# 批量处理 PDF 文件
mineru-dev demo/pdfs/ -o output/ --batch

# 使用 VLM 模式
mineru-dev demo/demo.pdf -o output.md --mode vlm
```

## 端口映射

- `30000`: MinerU SGLang 服务器
- `8888`: Jupyter Notebook
- `6006`: TensorBoard

## 环境变量

- `MINERU_MODEL_SOURCE=local`: 使用本地模型
- `PYTHONPATH=/workspace`: Python 路径
- `CUDA_VISIBLE_DEVICES=0`: GPU 设备

## 模型管理

### 自动下载
模型会在首次使用时自动下载到 `/home/vscode/.cache/mineru/`

### 手动下载
```bash
# 下载所有模型
mineru-models-download -s huggingface -m all

# 下载特定模型
mineru-models-download -s huggingface -m vlm
mineru-models-download -s huggingface -m pipeline
```

## 开发工作流

### 1. 代码开发
- 在 `mineru/` 目录下进行开发
- 使用 `mineru-lint` 保持代码质量
- 运行 `mineru-test` 确保测试通过

### 2. 测试
```bash
# 运行所有测试
pytest tests/ -v

# 运行特定测试
pytest tests/test_cli/ -v

# 生成覆盖率报告
pytest tests/ --cov=mineru --cov-report=html
```

### 3. 调试
- 使用 VS Code 调试器
- 设置断点进行调试
- 查看变量和调用栈

## 故障排除

### CUDA 问题
```bash
# 检查 CUDA 是否可用
nvidia-smi

# 检查 PyTorch CUDA 支持
python -c "import torch; print(torch.cuda.is_available())"
```

### 模型下载问题
```bash
# 清理模型缓存
rm -rf /home/vscode/.cache/mineru/*

# 重新下载模型
mineru-models-download -s huggingface -m all
```

### 内存不足
- 减少批处理大小
- 使用 CPU 模式：`--device cpu`
- 增加容器共享内存：`--shm-size=4g`

## 扩展开发

### 添加新功能
1. 在 `mineru/` 目录下创建新模块
2. 添加相应的测试
3. 更新文档
4. 运行测试确保通过

### 自定义配置
- 编辑 `mineru.json` 配置文件
- 参考 `mineru.template.json` 模板

## 贡献指南

1. Fork 项目
2. 创建功能分支
3. 提交更改
4. 创建 Pull Request

## 更多资源

- [MinerU 官方文档](https://mineru.net/)
- [GitHub 仓库](https://github.com/opendatalab/MinerU)
- [在线演示](https://huggingface.co/spaces/opendatalab/MinerU) 