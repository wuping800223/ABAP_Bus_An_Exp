---
name: ABAP代码业务逻辑分析和注释工具(中文)|ABAP Code Business Logic Analysis and Annotation Tool (Chinese)
description: |
  专业的ABAP代码业务逻辑分析工具，帮助SAP开发者深入理解代码的业务含义和逻辑结构。
  本SKILL的审计过程是执行本地PYTHON实现的，不是通过大模型实现的，所以可以大量节省TOKEN，并且速度很快
  本SKILL适用于外部审计ABAP代码的情况 
  本SKILL需要Python 3.13以上环境
  触发词：ABAP业务分析、ABAP+分析、业务逻辑分析、代码注释、代码说明、逻辑注释、ABAP解读、代码解析、ABAP代码分析、分析ABAP、给代码加注释、ABAP逻辑、代码逐行说明
  命名规范检查、SQL注入检测、硬编码检测、ABAP代码分析、代码来源识别、
  ABAP版本选择、指定版本审计、多版本审计
  出具TXT/HTML/PDF/JSON的各种中文格式的审计报告
---
# ABAP_Bus_An_Exp - ABAP代码业务逻辑分析和注释工具

## 概述

专业的ABAP代码业务逻辑分析工具，帮助SAP开发者深入理解代码的业务含义和逻辑结构。
支持多种输入源，自动识别代码来源，对每行代码进行详细说明，并对代码块结构（LOOP/IF/WHILE/BEGIN OF-END OF等）进行整体说明，生成多格式分析报告。

**触发词**：ABAP业务分析、ABAP+分析、业务逻辑分析、代码注释、代码说明、逻辑注释、ABAP解读、代码解析、ABAP代码分析、分析ABAP、给代码加注释、ABAP逻辑、代码逐行说明

---

## 核心功能

### 1. 输入源支持

| 输入类型 | 支持格式 | 说明 |
|----------|----------|------|
| 文本文件 | `.txt` | 单个TXT文件 |
| Markdown | `.md` | 自动提取代码块 |
| 压缩包 | `.zip` / `.rar` | 解压后批量处理 |
| 目录 | 文件夹 | 递归处理目录下所有文件 |
| 代码片段 | 直接粘贴 | 用户输入的代码 |

### 2. 分析内容

#### 抬头：整体业务逻辑分析
在报告顶部以注释形式输出整个程序的业务逻辑分析，格式：
```
*ABAP_Bus_An_Exp业务逻辑分析:
*  本程序实现了XXX业务功能...
*  主要包含以下处理步骤：
*  1. ...
*  2. ...
```

#### 行级：代码说明注释
在每行/每段代码后添加业务说明，格式：
```
SELECT * FROM mara ...
"ABAP_Bus_An_Exp代码说明: 从物料主数据表MARA中查询物料基本信息，...
```

#### 代码块结构说明
对以下代码块结构添加专项说明：
- `LOOP AT ... ENDLOOP` - 循环块开始/结束含义
- `IF ... ENDIF` - 条件判断块含义
- `WHILE ... ENDWHILE` - While循环块含义
- `BEGIN OF ... END OF` - 结构体定义块
- `FORM ... ENDFORM` - 子程序块
- `FUNCTION ... ENDFUNCTION` - 函数模块块
- `METHOD ... ENDMETHOD` - 方法块
- `CLASS ... ENDCLASS` - 类定义/实现块
- `TRY ... CATCH ... ENDTRY` - 异常处理块
- `SELECT ... ENDSELECT` - 带循环的SELECT块

### 3. 输出格式

| 格式 | 说明 |
|------|------|
| 📄 **TXT** | 纯文本，原始代码+注释交织 |
| 🌐 **HTML** | 美观网页报告（⭐默认） |
| 📋 **JSON** | 结构化JSON数据 |
| 📕 **PDF** | PDF格式（需要reportlab） |
| 📦 **ALL** | 保存所有格式 |

### 4. 用户提示增强

分析时用户可输入自己对该程序的了解（业务背景、已知问题等），这些信息会融入业务逻辑分析并在报告中标注来源。

```
提示：这是一个SD发票校验程序，用于月末对账，已知问题是FORY字段有时取不到值
```

多条提示用逗号分隔，直接回车跳过。在命令行模式下也可通过 `--hints` 参数传入。

---

## 使用示例

### 示例1：分析代码片段
```
帮我分析以下ABAP代码的业务逻辑：

LOOP AT lt_ekpo INTO ls_ekpo.
  SELECT SINGLE * FROM mara WHERE matnr = ls_ekpo-matnr.
  ...
ENDLOOP.
```

### 示例2：分析文件
```
分析 D:/temp/zsd_order.abap 的业务逻辑
```

### 示例3：分析ZIP包
```
分析 SAP_CODES.zip 中所有代码的业务逻辑
```

### 示例4：指定输出格式
```
分析 zreport.txt，输出 HTML 格式的业务逻辑报告
```

---

## 报告结构

```
【*ABAP_Bus_An_Exp业务逻辑分析】         <- 抬头注释（程序整体逻辑）
原始代码...
"ABAP_Bus_An_Exp代码说明: xxx           <- 行注释（每行/每块说明）
...更多代码...

---------- 报告结尾（Footer） ----------
报告生成时间: YYYY-MM-DD HH:MM:SS
Author: 吴平 | 联系方式: 13574840501 | 微信号：wuping800223
```

---

## 架构设计

```
ABAP_Bus_An_Exp/
├── scripts/
│   ├── frm_bae_main.py            # 主入口（交互式格式选择）
│   ├── frm_bae_file_handler.py    # 文件输入处理器
│   ├── frm_bae_analyzer.py        # 业务逻辑分析引擎
│   ├── frm_bae_report_generator.py # 报告生成器 (TXT/HTML/JSON)
│   └── frm_bae_pdf_builder.py     # PDF报告构建器
├── config/
│   └── (预留配置目录)
├── token.json                     # 词元配置文件（自定义报告用词）
└── SKILL.md                       # 本文档
```

---

## 输出目录

所有分析报告默认输出到：`ABAP_AnExp_Reports`

路径规则：
- 优先使用当前工作区下的 `ABAP_AnExp_Reports` 目录
- 若无法创建，则使用用户主目录下 `ABAP_AnExp_Reports`
- 用户也可在分析时通过 `--output-dir` 参数或交互方式自行指定其他输出目录

---

## 格式选择交互

分析完成后，自动弹出交互界面：

```
============================================================
💾 分析报告已生成！请选择保存格式：
============================================================
  1. TXT  - 纯文本格式，代码与注释交织显示
  2. HTML - 网页格式，样式美观（⭐推荐，默认）
  3. JSON - JSON格式，适合程序处理
  4. PDF  - PDF文档，适合打印存档
  5. ALL  - 保存所有格式

请输入选项 [1-5] (默认2):
```

---

## 依赖要求

```bash
pip install reportlab    # 用于PDF生成（可选）
pip install rarfile      # 支持RAR压缩包（可选）
```

---

*Last Updated: 2026-04-22*
*Version: 1.1.0*
*Author: 吴平*
*联系方式: 13574840501 | 微信号：wuping800223*
