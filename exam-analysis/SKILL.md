---
name: exam-analysis
description: 生成结构完整的中学试卷分析报告。当用户提到"试卷分析"、"帮我分析试卷"、"生成试卷分析"、"做试卷分析"、"分析这套题"、"出一份试卷分析"、"这套题帮我分析一下"、"考完试分析"时必须触发此skill。即使用户只说"帮我分析这套数学题"或提供了试卷数据/Excel文件、想了解学生失分原因，也应主动使用此skill。
---

# 试卷分析 Skill

帮助教师或培训师快速生成专业的试卷分析报告，并导出为PDF文件，涵盖整体概览、难度分布、知识板块分布、失分预警、逐题分析五个模块。

## 工作流程

### 第一步：收集信息

如果用户没有提供完整数据，按以下顺序询问：

1. **试卷基本信息**：
   - 试卷名称（如：2026江阴中考数学二模）
   - 学科和年级
   - 总题数
   - 题型分布（如：选择题1-10、填空题11-15、解答题16-26、综合探究题27-28）
   - 各题型分值（如：选择10题30分、填空5题15分、解答13题105分）

2. **逐题数据**（可让用户粘贴文字、上传Excel/图片）：
   - 每题考察知识点
   - 每题难度（易/中/难）
   - 学生常见错因（如果用户有数据）

如果用户提供了Excel文件，使用Python读取数据（参考下方"读取Excel"说明）。

### 第二步：智能补全分析

对于用户没有提供的内容，根据知识点和难度**主动推断**：

- **常见错因**：基于该知识点的普遍易错点推断
- **解决方案**：给出具体可执行的学习建议
- **知识板块归类**：将每道题归入对应板块
- **失分预警**：归纳典型失误类型

不要因为数据不完整就停下来，能推断的主动补全。

### 第三步：生成完整报告并导出PDF

按以下**固定顺序**输出五模块报告，然后将报告导出为PDF文件（见下方"导出PDF"说明）。

---

## 报告标准模板

按以下**固定顺序**输出五个模块：

---

### 一、试卷整体概览

| 项目 | 内容 |
|------|------|
| 试卷名称 | [名称] |
| 总题数 | [N]题 |
| 题型分布 | 选择题（[题号范围]）、填空题（[题号范围]）、解答题（[题号范围]）、综合探究题（[题号范围]） |
| 分值分布 | 选择[N]题([分]分) + 填空[N]题([分]分) + 解答[N]题([分]分) = [总分]分 |
| 难度比例 | 易:中:难 ≈ [占比] |

---

### 二、难度分布

| 难度 | 题号 | 题数 | 占比 | 说明 |
|------|------|------|------|------|
| 易 | [题号] | [N] | [x%] | |
| 中 | [题号] | [N] | [x%] | 中等题占比最高，符合考试"基础过关+能力提升+选拔冲刺"的设计思路 |
| 难 | [题号] | [N] | [x%] | |

---

### 三、知识点板块分布

根据试卷学科**自动适配**板块，将每道题归入对应板块。实际板块以试卷涵盖的知识点为准，多余的板块可删去，不够的可新增：

**数学：** 数与式 / 方程与不等式 / 函数 / 几何 / 三角函数 / 统计与概率 / 综合探究（压轴）

**语文：** 基础知识（字词句）/ 古诗文阅读 / 现代文阅读 / 写作 / 语言运用

**英语：** 听力 / 单词与语法 / 阅读理解 / 完形填空 / 写作

**物理：** 声与光学 / 热学与物态变化 / 运动与力 / 压强与浮力 / 简单机械与功 / 电路与欧姆定律 / 电功与电热 / 电磁学 / 实验探究

**化学：** 物质结构与性质 / 化学方程式与计算 / 酸碱盐 / 氧化还原反应 / 实验操作

**历史：** 中国古代史 / 中国近现代史 / 世界史 / 综合主题题

**地理：** 自然地理 / 人文地理 / 区域地理 / 地图与工具

**生物：** 细胞与生命活动 / 生物与环境 / 遗传与进化 / 人体生理

| 知识板块 | 对应题号 | 题数 | 占比 |
|---------|---------|------|------|
| [板块1] | [题号] | [N] | [x%] |
| ... | | | |

每道题只归入一个主要板块，以该题核心考点为准。

---

### 四、失分预警与备考建议

**高频失分类型：**

| 失分类型 | 涉及题号 | 典型失误 |
|---------|---------|---------|
| 计算失误 | [题号] | [具体描述] |
| 公式混淆 | [题号] | [具体描述] |
| 分类讨论不全 | [题号] | [具体描述] |
| 读题不仔细 | [题号] | [具体描述] |
| 综合思路断裂 | [题号] | [具体描述] |

**分层备考建议：**

- **基础题保满分**（第[题号]题）：每天限时[X]分钟完成全部基础题，错误率为零是底线
- **中档题做稳做准**（第[题号]题）：专项训练[具体模块]，掌握标准解题流程
- **难题抓分争满分**（第[题号]题）：分小问拿分，第①问必拿；分类讨论列全不漏；最后5分钟回头检查

**总结**：[一句话概括试卷特点和备考重点]

---

### 五、逐题分析

| 题号 | 考察知识点 | 难度 | 常见错因分析 | 解决方案 |
|------|-----------|------|-------------|---------|
| 1 | [知识点] | 易/中/难 | [具体错因] | [具体建议] |
| ... | | | | |

**要求**：
- 常见错因要具体，写出学生实际会犯的错（如"混淆收入/支出的正负号，误将支出50元记作+50"），不写空泛的"粗心"
- 解决方案要可操作（如"令被开方数≥0列不等式，规范解题步骤"），不写"多练习"

---

## 读取Excel文件

如果用户提供了Excel文件，用Python读取：

```python
import zipfile, xml.etree.ElementTree as ET

with zipfile.ZipFile('文件路径.xlsx', 'r') as z:
    shared_strings = []
    if 'xl/sharedStrings.xml' in z.namelist():
        tree = ET.parse(z.open('xl/sharedStrings.xml'))
        root = tree.getroot()
        ns = {'ns': 'http://schemas.openxmlformats.org/spreadsheetml/2006/main'}
        for si in root.findall('ns:si', ns):
            t = si.find('.//ns:t', ns)
            shared_strings.append(t.text if t is not None else '')

    for sheet_file in z.namelist():
        if sheet_file.startswith('xl/worksheets/sheet'):
            tree = ET.parse(z.open(sheet_file))
            root = tree.getroot()
            ns3 = {'ns': 'http://schemas.openxmlformats.org/spreadsheetml/2006/main'}
            for row in root.findall('.//ns:row', ns3):
                row_data = []
                for c in row.findall('ns:c', ns3):
                    t = c.get('t', '')
                    v = c.find('ns:v', ns3)
                    if v is not None:
                        row_data.append(shared_strings[int(v.text)] if t == 's' else v.text)
                    else:
                        row_data.append('')
                print(row_data)
```

---

## 导出PDF

报告内容生成完毕后，**默认导出PDF文件**，无需询问用户。

用Python生成PDF，使用 `reportlab` 库（如未安装先运行 `pip install reportlab`）：

```python
from reportlab.lib.pagesizes import A4
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.units import mm
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, HRFlowable
from reportlab.lib import colors
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
import os

# 注册中文字体（使用系统字体）
font_paths = [
    '/System/Library/Fonts/STHeiti Light.ttc',        # macOS
    '/System/Library/Fonts/Supplemental/Arial Unicode MS.ttf',
    '/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc',   # Linux
    'C:/Windows/Fonts/msyh.ttc',                       # Windows
]
for fp in font_paths:
    if os.path.exists(fp):
        pdfmetrics.registerFont(TTFont('Chinese', fp))
        break

# 输出路径：与用户工作目录同级，或保存到桌面
output_path = os.path.expanduser('~/Desktop/试卷分析报告.pdf')

doc = SimpleDocTemplate(output_path, pagesize=A4,
    leftMargin=20*mm, rightMargin=20*mm,
    topMargin=20*mm, bottomMargin=20*mm)

styles = getSampleStyleSheet()
cn_style = ParagraphStyle('Chinese', fontName='Chinese', fontSize=10, leading=16)
title_style = ParagraphStyle('Title', fontName='Chinese', fontSize=16, leading=24, alignment=1, spaceAfter=6)
h1_style = ParagraphStyle('H1', fontName='Chinese', fontSize=13, leading=20, spaceBefore=12, spaceAfter=6, textColor=colors.HexColor('#1a56db'))
h2_style = ParagraphStyle('H2', fontName='Chinese', fontSize=11, leading=18, spaceBefore=8, spaceAfter=4)

# 表格内单元格文字样式（支持自动换行）
cell_style = ParagraphStyle('Cell', fontName='Chinese', fontSize=9, leading=13, wordWrap='CJK')
cell_bold  = ParagraphStyle('CellBold', fontName='Chinese', fontSize=9, leading=13, wordWrap='CJK', textColor=colors.HexColor('#1e3a8a'))

def P(text, style=None):
    """把文字包装成可换行的Paragraph，所有表格单元格内容都必须用此函数包装"""
    return Paragraph(text, style or cell_style)

def make_table(data, col_widths=None):
    # data 中每个单元格必须是 P(...) 返回的 Paragraph，否则长文本不会自动换行
    t = Table(data, colWidths=col_widths, repeatRows=1)
    t.setStyle(TableStyle([
        ('FONTNAME', (0,0), (-1,-1), 'Chinese'),
        ('FONTSIZE', (0,0), (-1,-1), 9),
        ('BACKGROUND', (0,0), (-1,0), colors.HexColor('#dbeafe')),
        ('TEXTCOLOR', (0,0), (-1,0), colors.HexColor('#1e3a8a')),
        ('ALIGN', (0,0), (-1,-1), 'LEFT'),
        ('VALIGN', (0,0), (-1,-1), 'TOP'),
        ('GRID', (0,0), (-1,-1), 0.4, colors.HexColor('#9ca3af')),
        ('ROWBACKGROUNDS', (0,1), (-1,-1), [colors.white, colors.HexColor('#f0f9ff')]),
        ('TOPPADDING', (0,0), (-1,-1), 5),
        ('BOTTOMPADDING', (0,0), (-1,-1), 5),
        ('LEFTPADDING', (0,0), (-1,-1), 5),
        ('RIGHTPADDING', (0,0), (-1,-1), 5),
    ]))
    return t

story = []

# 标题
story.append(Paragraph('[试卷名称] 试卷分析报告', title_style))
story.append(HRFlowable(width='100%', thickness=1, color=colors.HexColor('#1a56db')))
story.append(Spacer(1, 6*mm))

# 一、整体概览（用表格）
story.append(Paragraph('一、试卷整体概览', h1_style))
overview_data = [['项目', '内容'], ...]  # 填入实际数据
story.append(make_table(overview_data, col_widths=[50*mm, 120*mm]))
story.append(Spacer(1, 4*mm))

# 二、难度分布
story.append(Paragraph('二、难度分布', h1_style))
# ... 同上，用 make_table 生成表格

# 三、知识点板块分布
story.append(Paragraph('三、知识点板块分布', h1_style))
# ...

# 四、失分预警与备考建议
story.append(Paragraph('四、失分预警与备考建议', h1_style))
# 失分类型表格 + 分层建议段落文字

# 五、逐题分析（题目多时自动分页）
story.append(Paragraph('五、逐题分析', h1_style))
# 每题一行，表格较宽时使用 landscape 或缩小字号

doc.build(story)
print(f'PDF已保存至：{output_path}')
```

**注意**：
- PDF保存路径默认为桌面：`~/Desktop/[试卷名称]_试卷分析.pdf`，生成后告知用户文件位置
- 中文字体使用 `/System/Library/Fonts/STHeiti Light.ttc`（macOS已内置，无需安装）
- 逐题分析表格列较多，col_widths 参考：`[12, 28, 12, 60, 58]*mm`，字号用9pt
- reportlab 安装命令：`pip3 install reportlab --break-system-packages`

---

## 注意事项

- 报告风格：**专业严谨 + 通俗易懂**，结论先行，避免堆砌术语
- 错因分析必须具体，写出学生实际犯的错，不写空洞描述
- 建议必须可操作，有具体步骤或练习方向
- 如果用户只提供了知识点没有错因，根据该知识点的普遍难点自主推断，不要停下来反复追问
