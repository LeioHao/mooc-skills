---
name: "natural-language-course-parser"
description: "将用户的自然语言课程需求转换为标准化的结构化课程需求。解析课程主题、目标受众、时长偏好、教学风格等关键参数。当用户用自然语言描述课程设计要求时使用此技能。"
---

# Natural Language Course Parser - 自然语言课程解析器

## 概述

本技能是微课视频生产线的入口技能，负责将用户的自然语言描述转换为标准化的结构化课程需求。通过深度语义理解和意图识别，系统能够准确提取用户表达中的课程主题、目标受众、时长偏好、教学风格等关键参数，为后续的自动化课程生产提供高质量的输入。

## 核心功能

### 1. 课程主题识别

**识别能力：**
- 从用户描述中提取课程名称或主题关键词
- 识别学科领域归属（支持一级学科、二级学科分类）
- 理解课程的核心定位和边界范围
- 识别课程类型（理论课、实践课、混合课）

**识别策略：**
```python
课程主题识别 = 函数(
    输入文本 = 用户自然语言描述,
    识别模式 = {
        '精确匹配': '课程名称直接提取',
        '模糊匹配': '关键词扩展+同义词识别',
        '领域推断': '基于内容推断学科归属'
    }
)
```

**输出映射：**
| 用户描述 | 识别结果 |
|---------|---------|
| "Python编程入门" | course_name: Python编程入门, domain: 计算机科学 |
| "教小孩学数学" | course_name: 儿童数学启蒙, domain: 基础教育, audience_implicit: 儿童 |
| "企业管理培训" | course_name: 企业管理, domain: 管理学, scenario: 企业培训 |

### 2. 目标受众识别

**识别维度：**
- **人口统计特征**：年龄范围、学历层次、职业背景
- **角色定位**：教师、学生、家长、从业者、爱好者
- **知识水平**：零基础、入门、有基础、专业
- **学习动机**：考试、职业发展、兴趣、解决问题

**识别规则库：**

| 关键词模式 | 识别人群 | 画像推断 |
|-----------|---------|---------|
| "给孩子/小孩/孩子" | 家长群体 | 25-40岁, 实用导向, 关注孩子成长 |
| "大学生/大学课程" | 高等教育学习者 | 18-22岁, 系统学习, 学术导向 |
| "职场/员工/培训" | 在职从业者 | 22-45岁, 技能提升, 应用导向 |
| "零基础/入门" | 初学者 | 知识水平=零基础, 需要通俗化讲解 |
| "专业人士/进阶" | 有基础学习者 | 知识水平=有基础, 可深入理论 |

**复合人群识别：**
```
输入: "帮助家长理解孩子心理"
识别结果:
  - primary_audience: 婴幼儿/儿童家长
  - secondary_audience: [家庭教育工作者]
  - knowledge_assumption: 无心理学专业基础
  - learning_focus: [理解孩子心理发展规律, 实际应用]
```

### 3. 时长偏好解析

**时长范围识别：**
- 识别用户表达的具体数字（分钟/节）
- 识别模糊表达（"短一些"、"不要太长"）
- 推断合理性（碎片化学习→短时长，系统学习→标准时长）

**时长标准化映射：**

| 用户描述 | 解析结果 |
|---------|---------|
| "每节课5分钟左右" | min: 4, max: 6, optimal: 5 |
| "不要太长，10分钟以内" | min: 5, max: 10, optimal: 8 |
| "碎片化时间学习" | min: 3, max: 8, optimal: 5 |
| "深入讲解" | min: 10, max: 15, optimal: 12 |

**MOOC标准校验：**
- 低于3分钟：提示可能影响内容完整性
- 超过15分钟：建议拆分为多个微课
- 低于5分钟或超过20分钟：触发需求确认

### 4. 教学风格识别

**风格维度解析：**

| 风格特征 | 识别关键词 | 参数映射 |
|---------|-----------|---------|
| 案例丰富 | "案例多"、"结合实际"、"生活化" | case_ratio: 0.7 |
| 理论深入 | "系统学习"、"理论基础"、"深入理解" | theory_depth: high |
| 通俗易懂 | "简单"、"易懂"、"科普" | conceptual_depth: low |
| 互动性强 | "互动"、"讨论"、"思考" | interactivity: high |
| 动画生动 | "动画效果"、"视觉丰富"、"生动" | animation_level: high |

**风格组合推断：**

```python
if "家长" in audience and "孩子" in topic:
    inferred_style = {
        "tone": "亲切温和",
        "case_ratio": 0.7,
        "theory_depth": "low",
        "interactivity": "medium"
    }
elif "大学生" in audience or "系统学习" in requirement:
    inferred_style = {
        "tone": "正式专业",
        "case_ratio": 0.4,
        "theory_depth": "medium-high",
        "interactivity": "low"
    }
```

### 5. 特殊需求提取

**识别类型：**
- **明确需求**：引号内容、强调词后的需求
- **隐性需求**：基于人群和场景的合理推断
- **约束条件**：时长限制、设备要求、语言偏好

**提取规则：**
```python
特殊需求提取 = 函数(
    输入文本 = 用户描述,
    提取策略 = {
        '引号提取': '匹配"..."内容',
        '关键词强化': '识别"重点"、"必须"、"一定"等强调词',
        '条件识别': '识别"除了...之外"、"不要..."等约束',
        '隐含推断': '基于上下文推断未明确表达的需求'
    }
)
```

## 输入规范

### 1. 接受输入格式

**自然语言自由描述：**
```
"我想做一门关于人工智能的微课，主要是给职场人士看的，
想帮助他们了解AI在工作中的实际应用。每节课8分钟左右，
希望有丰富的案例和实操演示。"
```

**半结构化描述：**
```
课程: Python数据分析
受众: 有一定编程基础的职场人士
时长: 每节10-15分钟
风格: 理论与实践结合，注重实操
要求: 包含Jupyter Notebook演示
```

### 2. 输入参数

| 参数名 | 类型 | 必填 | 说明 |
|-------|------|------|------|
| user_input | string | 是 | 用户自然语言描述 |
| context | object | 否 | 上下文信息（历史对话、用户画像） |
| language | string | 否 | 目标语言，默认zh-CN |

## 输出规范

### 标准输出格式

```json
{
  "parsed_requirement": {
    "course_name": "人工智能职场应用",
    "target_domain": {
      "primary": "计算机科学",
      "secondary": ["人工智能", "职业发展"],
      "discipline_code": "0807"
    },
    "difficulty_level": "入门-进阶",
    "application_scenario": "职业培训",
    "learner_profile": {
      "primary": "职场从业者",
      "secondary": ["技术管理者"],
      "knowledge_prerequisite": "基本计算机操作能力",
      "explicit_description": "有一定编程基础的职场人士"
    },
    "lesson_duration_preference": {
      "min": 7,
      "max": 10,
      "optimal": 8,
      "confidence": 0.95
    },
    "teaching_style": {
      "case_based": true,
      "case_ratio": 0.6,
      "theory_depth": "medium",
      "practical_focused": true,
      "animation_rich": false,
      "interactivity": "medium"
    },
    "special_requirements": [
      "Jupyter Notebook演示",
      "实操演示",
      "职场应用场景"
    ],
    "implicit_needs": [
      "易于跟学的代码示例",
      "可下载的学习资料"
    ]
  },
  "parsing_metadata": {
    "confidence_scores": {
      "course_name": 0.98,
      "audience": 0.92,
      "duration": 0.95,
      "style": 0.88
    },
    "missing_fields": [],
    "needs_clarification": [],
    "raw_text": "用户原始输入..."
  }
}
```

## 解析决策引擎

### 1. 多轮澄清策略

**自动触发澄清的场景：**

| 场景 | 缺失信息 | 澄清问题 |
|------|---------|---------|
| 受众模糊 | 目标人群不明确 | "请问这门课主要面向谁学习？" |
| 时长冲突 | 矛盾表达 | "您提到要深入讲解又希望5分钟内，是否考虑拆分为多节？" |
| 风格模糊 | 无法判断教学风格 | "您偏好理论深入还是案例丰富的讲解方式？" |
| 课程范围模糊 | 边界不清晰 | "您希望这门课覆盖哪些具体内容？" |

**澄清对话示例：**
```
用户: "我想做一门心理学的微课"
系统: "了解了！为更好地设计课程，请补充以下信息：
      1. 这门课主要面向谁学习？（如：学生、家长、职场人士）
      2. 每节课您希望多长时间？（如：5-10分钟）
      3. 主要用途是什么？（如：自主学习、课堂教学、培训）"

用户: "主要是给年轻妈妈看的，帮助她们了解孩子成长"
系统: "很好！已识别：
      - 目标受众：年轻妈妈/儿童家长
      - 学习目标：了解孩子成长规律
      - 建议方向：亲子教育、成长阶段分析
      请问还有哪些具体需求或偏好？"
```

### 2. 置信度评估

```python
置信度计算 = 函数(
    识别结果,
    原始输入,
    上下文信息
) -> 置信度分数

评估维度:
  - 实体识别准确率
  - 语义理解一致性
  - 上下文依赖度
  - 歧义消除效果

置信度阈值:
  - >= 0.9: 直接使用结果
  - 0.7-0.9: 确认后使用
  - < 0.7: 触发澄清
```

## 与其他技能协作

### 上游依赖

| 依赖技能 | 依赖内容 | 调用方式 |
|---------|---------|---------|
| 无 | - | 本技能为入口技能 |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| course-requirement-validator | 完整需求结构 | JSON |
| learner-profile-analyzer | 目标受众画像 | JSON |
| knowledge-graph-builder | 课程主题边界 | 文本描述 |

## 最佳实践

### 1. 输入优化建议

**提供完整信息的描述示例：**
```
"我想制作一门《儿童行为心理学》的微课系列，
目标受众是3-12岁儿童的家长，帮助他们理解孩子行为背后的心理机制。
每节课8分钟左右，以生活案例为主，弱化专业术语，
最后能给出实用的应对建议。总共计划20节左右。"
```

**简化描述的识别示例：**
```
用户: "Python入门"
系统: "了解了！我来为您补充以下信息以完善需求：
      [自动识别] 课程: Python编程入门
      [请确认] 目标受众是：
        A. 零基础初学者
        B. 有其他语言基础的开发者
        C. 数据分析从业者
      [请选择] 学习场景是：
        A. 自主学习
        B. 课堂教学
        C. 职业培训"
```

### 2. 歧义处理策略

**场景1：受众重叠**
```
输入: "给老师和学生看的编程课"
处理: 识别为双受众，优先按专业度较低的群体（学生）设计，
      同时为教师提供教学补充材料
```

**场景2：风格矛盾**
```
输入: "既要深入理论又要通俗易懂"
处理: 识别为"双轨设计"，理论部分提供进阶阅读，
      讲解部分保持通俗化
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 无法识别课程主题 | 描述过于抽象 | 引导用户提供具体课程名称或关键词 |
| 受众识别错误 | 多义词干扰 | 增加上下文确认步骤 |
| 时长推断不合理 | 缺乏上下文 | 基于MOOC标准提供建议值 |
| 风格识别偏离 | 用户表达与真实意图不符 | 增加澄清环节 |

## 技术实现框架

```python
class NaturalLanguageCourseParser:
    def __init__(self):
        self.entity_recognizer = EntityRecognizer()
        self.intent_classifier = IntentClassifier()
        self.style_analyzer = StyleAnalyzer()
        self.requirement_builder = RequirementBuilder()
    
    def parse(self, user_input: str, context: dict = None) -> ParsedRequirement:
        # 1. 实体识别
        entities = self.entity_recognizer.extract(user_input)
        
        # 2. 意图分类
        intents = self.intent_classifier.classify(user_input)
        
        # 3. 风格分析
        style = self.style_analyzer.analyze(user_input, entities)
        
        # 4. 需求构建
        requirement = self.requirement_builder.build(
            entities, intents, style, context
        )
        
        # 5. 置信度评估
        requirement.confidence = self.assess_confidence(requirement)
        
        # 6. 缺失识别
        requirement.missing_fields = self.identify_missing(requirement)
        
        return requirement
```

---

本技能是微课视频生产线的智能入口，通过自然语言理解技术实现"一句话开课"的便捷体验，同时确保需求的完整性和准确性。
