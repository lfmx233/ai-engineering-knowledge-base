# 便携式三端 AI 工程知识库

> 面向个人 AI 学习、编程开发与项目管理场景设计的便携式知识管理与本地 AI 工作环境。

**项目类型：个人 AI 工程效率系统 / 知识管理与本地 AI 应用**
**项目角色：项目负责人｜AI应用研发**
**项目周期：2026**

---

## 1. 项目背景

随着 Python、LLM、ComfyUI 及 AI 应用开发学习逐渐深入，我需要在**公司电脑、家庭电脑及其他设备**之间持续进行学习与开发。

实际使用过程中存在几个问题：

* 不同电脑的开发环境不统一；
* 公司电脑存在管理员权限、编译环境及网络环境等限制；
* Python 学习代码、Markdown 笔记、AI项目资料分散；
* 三台电脑之间需要同步学习进度；
* 希望通过本地 AI 辅助搜索和复盘个人知识；
* 后续还需要继续扩展 RAG、Agent 等 AI 应用能力。

因此，我设计并搭建了一套以**移动硬盘为核心载体**的个人 AI 工程工作区，将知识管理、代码开发、版本控制、本地 LLM 及 AI 知识检索整合到同一套架构中。

---

# 2. 项目目标

项目主要解决以下问题：

```text
多设备开发环境不统一
        ↓
移动硬盘统一工作环境

学习笔记、代码分散
        ↓
统一知识库

三台电脑无法高效同步
        ↓
Git版本控制

希望AI辅助学习和知识检索
        ↓
本地LLM + AI知识库

未来学习RAG / Agent
        ↓
预留AI应用扩展能力
```

最终形成：

> **一个可携带、可同步、可扩展的个人 AI 工程工作环境。**

---

# 3. 最终系统架构

```text
                    移动硬盘 AI-Workspace
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
     Obsidian             VSCode             Ollama
        │                   │                   │
     知识管理             代码开发             本地AI
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                      knowledge-base
                            │
                      Git版本管理
                            │
              ┌─────────────┼─────────────┐
              ↓             ↓             ↓
           电脑A          电脑B          电脑C
              │             │             │
            push           pull           pull
              └─────────────┼─────────────┘
                            ↓
                       GitHub私有仓库
```

移动硬盘目录：

```text
AI-Workspace/
│
├── PortableApps/
│   ├── Git/
│   ├── VSCode/
│   ├── Obsidian/
│   ├── Python/
│   ├── Ollama/
│   └── VeraCrypt/
│
├── knowledge-base/
│   ├── 00_Inbox/
│   ├── 01_Python/
│   ├── 02_LLM/
│   ├── 03_ComfyUI/
│   ├── 04_AI影视制作/
│   ├── 05_字字动画插件/
│   ├── 06_项目记录/
│   ├── 07_问题记录/
│   └── README.md
│
├── AI-Models/
│   └── OllamaModels/
│
├── Scripts/
│
├── Backup/
│
└── start.bat
```

---

# 4. 核心模块

## 4.1 Obsidian——知识管理

负责 Markdown 知识资产管理。

主要记录：

* Python 学习笔记
* LLM 学习笔记
* ComfyUI 技术笔记
* AI影视制作经验
* 字字动画插件开发记录
* 项目记录
* 技术问题与解决方案

通过结构化目录建立长期可积累的个人知识体系。

---

## 4.2 VSCode——代码与项目开发

使用 VSCode 管理 Python 学习代码及个人项目。

例如：

```text
05_字字动画插件/
├── plugin.py
├── config.py
└── README.md
```

实现：

> 知识笔记 ↔ 示例代码 ↔ 实际项目

之间的关联。

---

## 4.3 Git——三端版本同步

将 `knowledge-base` 作为 Git Repository。

基本同步流程：

```text
电脑A
 ↓
git add .
 ↓
git commit
 ↓
git push
 ↓
GitHub私有仓库
 ↓
git pull
 ↓
电脑B / 电脑C
```

通过版本控制解决多设备学习资料与代码同步问题，同时保留历史修改记录。

---

## 4.4 Ollama——本地 LLM

使用 Ollama 作为本地模型运行层。

模型统一存放到移动硬盘规划的：

```text
AI-Models/OllamaModels/
```

后续可根据设备性能选择不同规模的开源模型。

Ollama主要负责：

> **本地模型运行与推理**

为后续 AI 知识检索、Agent 等应用提供模型基础。

---

## 4.5 AnythingLLM——AI知识检索

在最终架构中使用 AnythingLLM 作为 AI 知识库应用层。

基本流程：

```text
knowledge-base
      ↓
文档解析
      ↓
知识索引 / 检索
      ↓
Ollama本地LLM
      ↓
AI问答
```

用于辅助搜索个人 Markdown、TXT、PDF 及其他知识资料。

---

# 5. 早期方案：Khoj 部署失败与技术选型调整

## 5.1 初始方案

项目最初尝试直接使用 **Khoj** 构建本地 AI 知识库。

初始思路为：

```text
Obsidian / Markdown
        ↓
       Khoj
        ↓
本地Embedding / LLM
        ↓
AI知识检索
```

希望通过单一工具完成：

> 知识管理 + AI搜索 + 本地模型

---

## 5.2 遇到的问题

实际部署过程中，Khoj 的 Python 依赖链较为复杂，其中涉及 Django、FastAPI、PyTorch、sentence-transformers 以及 `llama-cpp-python` 等组件。

其中 `llama-cpp-python` 的本地编译进一步依赖：

```text
C++
 ↓
CMake
 ↓
Visual Studio Build Tools
```

而项目的目标运行环境是：

> **移动硬盘 + 公司电脑**

公司电脑无法保证具备完整的 C++ 编译工具链，同时还可能存在：

* 管理员权限限制；
* 编译环境缺失；
* 网络环境限制；
* Python 环境不稳定。

最终导致原 Khoj 方案无法按照预期在便携环境中稳定部署。

---

## 5.3 问题定位

经过分析后发现，问题并不是单纯的：

> “Khoj 安装失败。”

真正的问题是：

> **将知识管理、向量检索、Embedding、本地模型运行等多个复杂组件集中在一个应用中，使系统对 Python 和本地编译环境产生了较强依赖。**

而我的实际目标并不是研究 Khoj 本身，而是：

> **利用 AI 提高个人学习和开发效率。**

因此继续围绕 Khoj 修复环境的收益较低。

---

# 6. 架构调整

在失败方案基础上，对系统进行模块化重新设计：

| 功能       | 原方案        | 新方案         |
| -------- | ---------- | ----------- |
| 知识管理     | Khoj       | Obsidian    |
| 代码开发     | Python环境内部 | VSCode      |
| 版本同步     | Git        | Git         |
| 本地模型     | Khoj内部能力   | Ollama      |
| AI知识检索   | Khoj       | AnythingLLM |
| Python环境 | 与Khoj耦合    | 独立便携Python  |
| 系统结构     | 单体         | 模块化         |

调整后的核心思想：

> **一个软件只承担一类核心职责。**

这样可以降低系统耦合，使某一个组件出现问题时不会影响整个知识库。

---

# 7. 为什么最终采用模块化架构

最终架构：

```text
Obsidian
    │
    │ Markdown
    ↓
knowledge-base
    │
    ├──────────────→ Git → GitHub
    │
    ↓
AnythingLLM
    │
    ↓
Ollama
    │
    ↓
本地LLM
```

各组件职责清晰：

```text
Obsidian → 管理知识
VSCode   → 开发代码
Git      → 管理版本
Ollama   → 运行本地模型
AnythingLLM → AI知识检索
```

这种架构更加符合：

> **便携、低耦合、可替换、可扩展**

的设计目标。

---

# 8. 项目部署思路

## 第一阶段：清理失败环境

删除：

```text
PortableApps/Python/
khoj/
```

避免旧 Python 环境及 Khoj 依赖残留影响后续部署。

---

## 第二阶段：建立独立 Python 环境

使用便携式 Python 重新建立独立环境。

当前方案规划使用：

```text
Python 3.11
```

作为 AI 应用开发的基础 Python 环境。

Python 与知识库、VSCode 相互独立，避免因为某个 AI 项目的依赖导致整个知识库环境损坏。

---

## 第三阶段：配置 VSCode

使用 VSCode 打开：

```text
knowledge-base/
```

并指定移动硬盘中的便携 Python 解释器。

实现：

```text
VSCode
 ↓
Portable Python
 ↓
.py文件
```

---

## 第四阶段：配置 Obsidian

将：

```text
knowledge-base/
```

作为 Obsidian Vault。

从而实现：

> Markdown知识库 = Git仓库 = Obsidian工作区

---

## 第五阶段：配置 Git

通过 Git 完成三台电脑之间的同步。

典型操作：

```bash
git add .
git commit -m "update"
git push
```

另一台电脑：

```bash
git pull
```

---

## 第六阶段：配置 Ollama

将本地模型与应用层分离。

```text
Ollama
   ↓
本地LLM
   ↓
AnythingLLM
```

避免将模型运行环境与知识库本身强绑定。

---

## 第七阶段：AI知识库

通过 AnythingLLM 读取：

```text
knowledge-base/
```

中的知识资料。

最终形成：

```text
个人知识
   ↓
Markdown
   ↓
知识索引
   ↓
检索
   ↓
本地LLM
   ↓
AI回答
```

---

# 9. 一键启动

设计 `start.bat`，统一启动主要工作环境。

基本思路：

```text
插入移动硬盘
      ↓
运行 start.bat
      ↓
启动 Ollama
      ↓
启动 Obsidian
      ↓
启动 VSCode
      ↓
进入个人AI工作环境
```

减少在不同电脑之间切换时的重复操作。

---

# 10. 项目难点

### 难点一：便携环境与复杂AI依赖冲突

早期 Khoj 部署失败暴露出：

> AI应用不仅依赖 Python，还可能依赖本地 C++ 编译工具链。

因此重新设计环境隔离方案。

---

### 难点二：公司电脑环境限制

不能假设公司电脑始终拥有：

* 管理员权限；
* C++编译环境；
* 完整网络环境；
* 稳定的系统Python。

因此采用移动硬盘便携化设计，尽量降低对主机环境的依赖。

---

### 难点三：多设备同步

普通文件复制无法很好解决：

> 多设备修改、版本记录、冲突及历史追踪。

因此使用 Git 建立统一版本控制机制。

---

### 难点四：知识与代码之间的关联

单纯的笔记系统无法满足 AI 开发学习需求。

因此将：

```text
学习笔记
+
代码
+
项目
+
问题记录
+
AI复盘
```

统一纳入同一个 Git 知识仓库。

---

# 11. 项目成果

目前建立了面向个人 AI 学习与研发的完整工作框架：

```text
学习
 ↓
Markdown知识记录
 ↓
Obsidian整理
 ↓
VSCode代码实践
 ↓
Git版本管理
 ↓
Ollama本地AI
 ↓
AI知识检索
 ↓
问题复盘
 ↓
知识沉淀
```

项目解决了：

* 多设备知识同步；
* 移动硬盘便携运行；
* Python与项目代码统一管理；
* AI学习资料结构化管理；
* 本地AI辅助知识检索；
* 后续 RAG / Agent 学习环境建设。

---

# 12. 后续规划

项目后续将继续向 AI 应用研发方向扩展：

```text
当前
│
├── Python
├── Git
├── Obsidian
├── Ollama
└── AI知识库
        │
        ↓
RAG
        │
        ↓
Embedding / Vector DB
        │
        ↓
LLM API
        │
        ↓
Agent
        │
        ↓
AI应用系统
```

目标是将个人知识库从：

> **“资料存储系统”**

逐步发展为：

> **“个人 AI 工程研发基础设施”。**

---

# 13. 项目总结

本项目的核心价值并不在于简单部署几个软件，而在于针对真实使用环境进行了**需求分析、技术选型、失败方案复盘和架构重构**。

通过放弃对单一知识库软件的强依赖，将系统拆分为：

> **知识管理 + 代码开发 + 版本控制 + 本地模型 + AI检索**

五个独立模块，在满足公司电脑限制、移动硬盘便携以及三端同步需求的同时，为后续 LLM、RAG、Agent 及 AI 应用开发预留扩展空间。

**项目角色：项目负责人｜AI应用研发**

**核心能力体现：需求分析｜技术选型｜系统架构｜环境部署｜Git版本管理｜本地LLM｜AI知识库｜问题定位与方案迭代**
