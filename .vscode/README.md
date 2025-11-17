# BettaFish VSCode 配置说明

本目录包含 BettaFish 子模块的 VSCode 配置，已自动检测并配置所有语言工具链。

## 检测到的技术栈

### Python (主要语言)

- **工具链**: conda (主要) + pip (备选)
- **配置文件**: `requirements.txt`, `config.py`, `app.py`
- **Python 版本**: 3.8+
- **Linter**: flake8 (max-line-length=120)
- **Formatter**: black (line-length=120)
- **类型检查**: mypy
- **项目类型**: Flask Web 应用 + 多智能体系统 (Insight/Media/Query/Report/Forum Engine)

## 快速开始

### Python 环境设置

#### 方案 1: 使用 conda (主要 - 推荐)

```bash
# 安装 conda
# 下载地址: https://docs.conda.io/projects/conda/en/latest/user-guide/install/

# 创建 conda 环境
conda create -n bettafish python=3.10

# 激活环境
conda activate bettafish

# 安装依赖
pip install -r requirements.txt
```

#### 方案 2: 使用 pip (备选)

```bash
# 创建虚拟环境
python -m venv .venv

# 激活虚拟环境
source .venv/bin/activate  # Linux/macOS
# 或
.venv\Scripts\activate  # Windows

# 安装依赖
pip install -r requirements.txt
```

**切换方式**: 编辑 `settings.json`

- 使用 conda: 保持当前配置 (默认)
- 使用 pip: 注释掉 conda 配置，取消注释 pip 配置部分

### 运行项目

```bash
# 启动 Flask 主应用
python app.py

# 或使用 Flask 开发服务器
flask run

# 访问 Web 界面
# http://localhost:5000
```

## 配置说明

### 自动格式化

- **Python**: 保存时自动使用 black 格式化
- **导入排序**: Python 自动组织导入

### 排除的目录

以下目录已配置为隐藏，以减少编辑器负担:

- `__pycache__/`, `.pytest_cache/`, `.mypy_cache/`
- `dist/`, `build/`, `.egg-info/`
- `env/`, `.venv/`, `venv/`
- `.env` 文件
- `logs/`, `output/`, `temp/`, `tmp/`
- `*_streamlit_reports/`, `final_reports/`

### 工具链切换

#### Python: conda ↔ pip

编辑 `settings.json` 中的 `python.defaultInterpreterPath`:

##### 使用 conda (默认)

```json
"python.defaultInterpreterPath": "${workspaceFolder}/env/bin/python",
```

##### 使用 pip

```json
"python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
```

## 必需的 VSCode 扩展

### Python (必需)

- **Python** (ms-python.python) - 官方 Python 扩展，包含 Pylance 语言服务器

### 推荐

- **Git Graph** (mhutchie.git-graph) - Git 可视化
- **YAML** (redhat.vscode-yaml) - YAML 语法支持
- **Even Better TOML** (tamasfe.even-better-toml) - TOML 语法支持和格式化
- **SQLTools** (mtxr.sqltools) - SQL 工具支持

## 项目结构

```text
BettaFish/
├── app.py                      # Flask 主应用入口
├── config.py                   # 配置管理 (Pydantic Settings)
├── requirements.txt            # 依赖列表
├── ForumEngine/               # 论坛引擎
├── InsightEngine/             # 洞察引擎
├── MediaEngine/               # 媒体引擎
├── QueryEngine/               # 查询引擎
├── ReportEngine/              # 报告引擎
├── MindSpider/                # 爬虫引擎
├── SentimentAnalysisModel/    # 情感分析模型
├── SingleEngineApp/           # Streamlit 单引擎应用
├── templates/                 # HTML 模板
├── static/                    # 静态资源
├── tests/                     # 测试文件
├── utils/                     # 工具函数
├── logs/                      # 日志目录
├── docker/                    # Docker 配置
└── .vscode/                   # VSCode 配置 (团队共享)
```

## 常见问题

### Q: Python 解释器未找到

A: 确保已创建虚拟环境或 conda 环境并激活。检查 `settings.json` 中的 `python.defaultInterpreterPath` 是否正确。

### Q: 导入错误

A: 确保已运行 `pip install -r requirements.txt` 安装所有依赖。

### Q: 格式化不工作

A: 确保已安装 Python 扩展，并在 VSCode 中启用格式化。

### Q: 如何配置 LLM API 密钥

A: 编辑 `config.py` 或创建 `.env` 文件，配置以下环境变量：

- `INSIGHT_ENGINE_API_KEY` - Insight Agent API 密钥
- `MEDIA_ENGINE_API_KEY` - Media Agent API 密钥
- `QUERY_ENGINE_API_KEY` - Query Agent API 密钥
- `REPORT_ENGINE_API_KEY` - Report Agent API 密钥
- `TAVILY_API_KEY` - Tavily 搜索 API 密钥
- `BOCHA_WEB_SEARCH_API_KEY` - Bocha 搜索 API 密钥

### Q: 如何启动 Streamlit 应用

A: 项目包含三个 Streamlit 应用，通过 Flask 主应用统一管理：

- Insight Engine Streamlit App (端口 8501)
- Media Engine Streamlit App (端口 8502)
- Query Engine Streamlit App (端口 8503)

启动 Flask 应用后，可通过 Web 界面启动各个 Streamlit 应用。

### Q: 如何使用数据库

A: 配置以下环境变量：

- `DB_DIALECT` - 数据库类型 (mysql 或 postgresql)
- `DB_HOST` - 数据库主机
- `DB_PORT` - 数据库端口
- `DB_USER` - 数据库用户名
- `DB_PASSWORD` - 数据库密码
- `DB_NAME` - 数据库名称

## 注意事项

1. **.vscode 文件夹已提交到 Git**，用于团队共享配置
2. **环境变量**: 使用 `.env` 文件配置敏感信息，不提交到 Git
3. **配置文件**: `config.py` 支持从 `.env` 文件和环境变量加载配置
4. **Python 版本**: 推荐 Python 3.10+ (根据 requirements.txt)
5. **依赖管理**: 项目使用 conda 管理依赖，确保跨平台兼容性

## 关键命令

```bash
# 创建 conda 环境
conda create -n bettafish python=3.10

# 激活环境
conda activate bettafish

# 安装依赖
pip install -r requirements.txt

# 启动 Flask 应用
python app.py

# 启动 Flask 开发服务器
flask run

# 运行测试
python tests/run_tests.py

# 查看日志
tail -f logs/app.log
```

## 环境变量

BettaFish 支持通过 `.env` 文件或环境变量配置所有参数：

```bash
# Flask 服务器
HOST=0.0.0.0
PORT=5000

# 数据库
DB_DIALECT=postgresql
DB_HOST=localhost
DB_PORT=5432
DB_USER=user
DB_PASSWORD=password
DB_NAME=bettafish

# LLM API
INSIGHT_ENGINE_API_KEY=your_key
INSIGHT_ENGINE_BASE_URL=https://api.moonshot.cn/v1
INSIGHT_ENGINE_MODEL_NAME=kimi-k2-0711-preview

# 搜索 API
TAVILY_API_KEY=your_key
BOCHA_WEB_SEARCH_API_KEY=your_key
```

## 架构说明

BettaFish 是一个多智能体舆情分析系统，包含以下核心组件：

- **ForumEngine**: 论坛监控和讨论管理
- **InsightEngine**: 深度洞察分析
- **MediaEngine**: 媒体内容分析
- **QueryEngine**: 查询和搜索引擎
- **ReportEngine**: 报告生成
- **MindSpider**: 网络爬虫和数据采集
- **SentimentAnalysisModel**: 情感分析模型

所有引擎通过 Flask 主应用统一管理，支持 Web UI 和 API 接口。

---

**最后更新**: 2025-11-17
