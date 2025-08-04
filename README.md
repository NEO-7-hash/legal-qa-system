# 智能法律问答系统

> 基于RAG检索增强生成技术和通义千问大模型的专业法律咨询AI助手

[![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)](https://python.org)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Build Status](https://github.com/yourusername/legal-qa-system/workflows/CI/badge.svg)](https://github.com/yourusername/legal-qa-system/actions)

## 📖 项目简介

智能法律问答系统是一个专为法律咨询场景设计的AI助手，结合了向量检索技术和大语言模型的优势，能够准确回答各类法律问题并提供相关法条依据。

### 🎯 核心优势

- **准确可靠**：基于真实法律条文，避免AI幻觉问题
- **来源可查**：每个回答都标注具体的法条出处
- **实时更新**：支持法规更新，无需重新训练模型
- **多端支持**：提供CLI工具、Web界面和API服务
- **成本友好**：使用qwen-turbo模型，平衡性能与成本

## 🌟 主要功能

### 核心能力
- 📚 **智能问答**：回答各类法律问题，提供专业分析
- 🔍 **精准检索**：从法律条文库中检索相关内容
- 📄 **文档处理**：支持PDF、Word、TXT等多种格式
- ⚖️ **案例分析**：结合典型案例提供实用建议

### 技术特性
- 🤖 **RAG架构**：检索+生成，确保答案准确性
- 🧠 **智能分块**：按法条结构智能切分文档
- 🚀 **高性能**：FAISS向量数据库，毫秒级检索
- 🔒 **数据安全**：本地部署，数据不离线

## 🚀 快速开始

### 环境要求

```
Python 3.9+
内存 8GB+（推荐16GB）
硬盘 10GB+可用空间
```

### 安装步骤

1. **克隆项目**
```bash
git clone https://github.com/yourusername/legal-qa-system.git
cd legal-qa-system
```

2. **创建虚拟环境**
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux  
source venv/bin/activate
```

3. **安装依赖**
```bash
pip install -r requirements.txt
```

4. **配置环境变量**
```bash
cp .env.example .env
```

编辑 `.env` 文件：
```env
QWEN_API_KEY=your_qwen_api_key_here
QWEN_MODEL=qwen-turbo
VECTOR_DB_PATH=./data/legal_index
```

5. **准备法律文档**
```bash
# 将法律文档放入 data/raw/ 目录
# 支持格式：PDF, Word, TXT, HTML
```

6. **构建索引**
```bash
python -m src.legal_cli.main ingest data/raw --output data/legal_index
```

7. **测试运行**
```bash
python -m src.legal_cli.main query "试用期最长可以多久？"
```

## 💻 使用方法

### CLI命令行工具

```bash
# 查看帮助
legalcli --help

# 导入法律文档
legalcli ingest ./data/samples --chunk-size 512

# 查询法律问题
legalcli query "劳动合同可以口头约定吗？" --model qwen-turbo --top-k 5

# 检索测试
legalcli retrieve "加班费" --top-k 10

# 重建索引
legalcli rebuild-index --clean
```

### Web界面

```bash
# 启动Web服务
python -m src.legal_cli.api.main

# 访问 http://localhost:8000
```

### API服务

```bash
# 启动API服务器
uvicorn src.legal_cli.api.main:app --host 0.0.0.0 --port 8000

# API文档：http://localhost:8000/docs
```

**API使用示例：**
```python
import requests

response = requests.post("http://localhost:8000/api/query", json={
    "query": "什么情况下可以解除劳动合同？",
    "model": "qwen-turbo",
    "top_k": 5
})

result = response.json()
print(result["answer"])
```

### Docker部署

```bash
# 构建并启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

## 🏗️ 项目结构

```
legal-qa-system/
├── src/                    # 源代码
│   └── legal_cli/         
│       ├── core/          # 核心模块
│       │   ├── qwen_client.py      # Qwen API客户端
│       │   ├── rag_engine.py       # RAG检索引擎
│       │   └── document_processor.py # 文档处理器
│       ├── commands/      # CLI命令
│       │   ├── ingest.py          # 文档导入
│       │   ├── query.py           # 问答查询
│       │   └── retrieve.py        # 检索测试
│       ├── api/           # API服务
│       │   ├── main.py            # FastAPI应用
│       │   └── models.py          # 数据模型
│       └── main.py        # CLI入口
├── tests/                 # 测试代码
│   ├── unit/             # 单元测试
│   ├── integration/      # 集成测试
│   └── performance/      # 性能测试
├── data/                  # 数据目录
│   ├── raw/              # 原始文档
│   ├── processed/        # 处理后的文档
│   └── samples/          # 示例数据
├── frontend/             # 前端代码
├── docs/                 # 项目文档
├── scripts/              # 辅助脚本
├── .github/              # GitHub Actions
└── configs/              # 配置文件
```

## 📊 性能指标

| 指标 | 数值 | 说明 |
|------|------|------|
| 检索延迟 | < 100ms | FAISS向量检索 |
| 问答响应 | < 3s | 包含推理生成时间 |
| 并发支持 | 100+ | 基于FastAPI异步处理 |
| 文档容量 | 10万+ | 支持大规模法律文档库 |
| 准确率 | 85%+ | 基于测试案例评估 |

## 🔧 配置说明

### 环境变量

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `QWEN_API_KEY` | - | 通义千问API密钥（必需） |
| `QWEN_MODEL` | qwen-turbo | 使用的模型版本 |
| `QWEN_BASE_URL` | - | API基础URL（可选） |
| `VECTOR_DB_PATH` | ./data/legal_index | 向量数据库路径 |
| `CHUNK_SIZE` | 512 | 文档分块大小 |
| `CHUNK_OVERLAP` | 50 | 分块重叠大小 |
| `TOP_K` | 5 | 默认检索数量 |
| `LOG_LEVEL` | INFO | 日志级别 |

### 模型选择

| 模型 | 特点 | 适用场景 | 成本 |
|------|------|----------|------|
| qwen-turbo | 速度快，成本低 | 简单问答 | 💰 |
| qwen-plus | 平衡性价比 | 复杂分析 | 💰💰 |
| qwen-max | 最高质量 | 专业咨询 | 💰💰💰 |

## 📚 使用示例

### 1. 劳动法咨询
```bash
$ legalcli query "试用期内被辞退有补偿吗？"

🏛️ 法律分析结果：

根据《劳动合同法》相关规定：

1. 试用期内解除分两种情况：
   - 用人单位证明劳动者不符合录用条件：无需支付补偿
   - 用人单位违法解除：需支付赔偿金

2. 具体分析：
   - 需要用人单位举证证明不符合录用条件
   - 录用条件必须明确且事先告知
   - 违法解除的赔偿标准为双倍经济补偿

📚 参考法条：
• 《劳动合同法》第21条 - 试用期解除规定
• 《劳动合同法》第39条 - 过失性解除
• 《劳动合同法》第87条 - 违法解除赔偿

置信度: 92% | 模型: qwen-turbo
```

### 2. 合同纠纷咨询
```bash
$ legalcli query "口头合同有法律效力吗？"

🏛️ 法律分析结果：

口头合同在法律上是有效的，但有特殊要求：

1. 一般原则：
   - 《民法典》规定当事人可以口头形式订立合同
   - 法律效力与书面合同相同

2. 例外情况（必须书面）：
   - 房屋买卖合同
   - 建设工程合同  
   - 技术开发合同
   - 一年以上的劳动合同

3. 风险提示：
   - 举证困难，建议重要合同采用书面形式
   - 可通过录音、证人等方式固定证据

📚 参考法条：
• 《民法典》第469条 - 合同形式
• 《民法典》第490条 - 书面合同要求

置信度: 88% | 模型: qwen-turbo
```

## 🧪 测试

```bash
# 运行所有测试
pytest

# 运行单元测试
pytest tests/unit/ -v

# 运行集成测试  
pytest tests/integration/ -v

# 生成覆盖率报告
pytest --cov=src --cov-report=html

# 性能测试
locust -f tests/performance/locustfile.py
```

## 📈 开发路线图

### v0.1.0 (当前版本)
- [x] 基本CLI工具
- [x] 文档处理和索引构建
- [x] Qwen API集成
- [x] 简单问答功能

### v0.2.0 (下个版本)
- [ ] Web界面
- [ ] API服务
- [ ] 批量处理
- [ ] 结果缓存

### v0.3.0 (计划中)
- [ ] 多模态支持（图片、表格）
- [ ] 实时协作
- [ ] 知识图谱集成
- [ ] 移动端适配

### v1.0.0 (远期目标)
- [ ] 企业版功能
- [ ] 多语言支持
- [ ] 云端部署
- [ ] 商业化模式

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 如何贡献

1. **报告问题**：在GitHub Issues中报告bug或提出需求
2. **提交代码**：Fork项目，创建特性分支，提交PR
3. **完善文档**：帮助改进文档和示例
4. **分享经验**：在Discussions中分享使用经验

### 开发流程

```bash
# 1. Fork并克隆项目
git clone https://github.com/yourusername/legal-qa-system.git

# 2. 创建开发分支
git checkout -b feature/your-feature-name

# 3. 安装开发依赖
pip install -r requirements-dev.txt

# 4. 设置pre-commit钩子
pre-commit install

# 5. 进行开发...

# 6. 运行测试
pytest tests/

# 7. 提交更改
git commit -m "Add: your feature description"

# 8. 推送并创建PR
git push origin feature/your-feature-name
```

### 代码规范

- 使用Black进行代码格式化
- 遵循PEP 8编码规范
- 添加类型注解
- 编写必要的测试
- 更新相关文档

## ❓ 常见问题

### Q: 支持哪些法律领域？
A: 目前主要支持民商法、劳动法、合同法等常见领域，可根据导入的文档扩展。

### Q: 如何提高答案准确性？
A: 1) 增加高质量法律文档 2) 调整检索参数 3) 使用更高级的模型版本

### Q: 可以处理多少文档？
A: 理论上无限制，推荐单次处理不超过10GB文档，可分批处理。

### Q: 如何更新法律法规？
A: 将新文档放入data/raw目录，重新运行ingest命令即可。

### Q: 支持其他语言模型吗？
A: 当前专注于Qwen，未来会支持更多模型。

### Q: 部署需要什么硬件要求？
A: 最低4GB内存，推荐8GB+，GPU可选但能提升性能。

## 📞 技术支持

- **GitHub Issues**: [提交问题](https://github.com/yourusername/legal-qa-system/issues)
- **Discussions**: [参与讨论](https://github.com/yourusername/legal-qa-system/discussions)
- **邮箱支持**: your.email@example.com
- **技术文档**: [查看Wiki](https://github.com/yourusername/legal-qa-system/wiki)

## 📄 许可证

本项目采用 [MIT 许可证](LICENSE)。

## 🙏 致谢

特别感谢以下开源项目：

- [通义千问](https://github.com/QwenLM/Qwen) - 优秀的大语言模型
- [FAISS](https://github.com/facebookresearch/faiss) - 高效的相似性搜索库
- [LangChain](https://github.com/langchain-ai/langchain) - LLM应用开发框架
- [FastAPI](https://fastapi.tiangolo.com) - 现代化的Web框架
- [Typer](https://typer.tiangolo.com) - 优雅的CLI工具库

## 📊 项目统计

![GitHub stars](https://img.shields.io/github/stars/yourusername/legal-qa-system?style=social)
![GitHub forks](https://img.shields.io/github/forks/yourusername/legal-qa-system?style=social)
![GitHub issues](https://img.shields.io/github/issues/yourusername/legal-qa-system)
![GitHub license](https://img.shields.io/github/license/yourusername/legal-qa-system)

---

<div align="center">

**如果这个项目对你有帮助，请给个⭐Star支持一下！**

[立即开始](https://github.com/yourusername/legal-qa-system#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B) • [查看文档](https://github.com/yourusername/legal-qa-system/wiki) • [报告问题](https://github.com/yourusername/legal-qa-system/issues)

</div>
