---
name: "learner-profile-analyzer"
description: "基于学习者描述自动构建多维度画像，分析学习动机、约束条件、知识基础和内容偏好，为课程内容适配提供精准依据。当需要分析目标受众特征时使用此技能。"
---

# Learner Profile Analyzer - 学习者画像分析器

## 概述

本技能是微课视频生产线的核心支撑技能，负责从用户描述和课程需求中提取、推断并构建完整的学习者画像。通过对学习者的人口统计特征、学习动机、知识基础、时间约束等多维度分析，为后续的内容适配、脚本撰写和教学设计提供精准依据。

## 核心功能

### 1. 人口统计特征提取

**提取维度：**

| 维度 | 识别内容 | 识别来源 |
|------|---------|---------|
| 年龄范围 | 精确年龄段或模糊描述 | 用户描述、角色推断 |
| 学历层次 | 小学/中学/本科/研究生等 | 明确描述、职业推断 |
| 职业背景 | 具体职业或职业类型 | 用户描述、角色定位 |
| 地域特征 | 地域偏好（可选） | 用户描述 |
| 语言能力 | 母语、学习语言 | 课程语言设置 |

**识别规则库：**

```python
人口统计特征库 = {
    "婴幼儿/儿童家长": {
        "age_range": "25-45",
        "education_level": "本科及以上为主",
        "professional_background": "各行各业",
        "typical_roles": ["全职妈妈", "职场父母", "祖辈"]
    },
    "大学生": {
        "age_range": "18-25",
        "education_level": "高中/大学在读",
        "professional_background": "学生",
        "typical_contexts": ["通识课", "专业课", "考研"]
    },
    "职场新人": {
        "age_range": "22-28",
        "education_level": "本科及以上",
        "professional_background": "初入职场",
        "typical_needs": ["技能入门", "职业素养", "办公技能"]
    },
    "企业管理者": {
        "age_range": "30-50",
        "education_level": "本科及以上",
        "professional_background": "管理岗位",
        "typical_focus": ["战略思维", "团队管理", "决策能力"]
    }
}
```

### 2. 学习动机分析

**动机类型分类：**

| 动机类型 | 特征描述 | 内容偏好 |
|---------|---------|---------|
| 职业发展型 | 晋升、技能提升、职业转型 | 实用性强、证书导向 |
| 兴趣探索型 | 好奇心驱动、个人爱好 | 趣味性强、深度适中 |
| 问题解决型 | 解决实际困难 | 即学即用、案例丰富 |
| 知识积累型 | 系统学习、完善知识体系 | 体系完整、理论扎实 |
| 社交互动型 | 社交需求、社区归属 | 互动设计、讨论空间 |
| 考试认证型 | 获取证书、学分 | 考点覆盖、模拟练习 |

**动机强度评估：**

```python
动机强度评估 = 函数(
    描述文本,
    评估维度 = {
        '明确度': '动机描述的清晰程度',
        '紧迫性': '时间压力或截止日期',
        '投入意愿': '愿意投入的时间/金钱',
        '坚持可能性': '长期学习的可能性'
    }
) -> 动机强度分数
```

**动机推断规则：**

| 描述关键词 | 推断动机 | 强度分数 |
|-----------|---------|---------|
| "升职加薪"、"跳槽"、"转行" | 职业发展型 | 0.9 |
| "感兴趣"、"好奇"、"想了解" | 兴趣探索型 | 0.6 |
| "孩子不听话"、"工作遇到问题" | 问题解决型 | 0.95 |
| "系统学习"、"完善知识" | 知识积累型 | 0.75 |
| "考试"、"考证"、"拿证书" | 考试认证型 | 0.95 |

### 3. 学习约束分析

**约束维度：**

| 约束类型 | 识别内容 | 推断策略 |
|---------|---------|---------|
| 时间约束 | 可用学习时间、时段偏好 | 场景推断+时长要求 |
| 设备约束 | 终端类型、网络条件 | 场景描述、设备说明 |
| 环境约束 | 学习场所、干扰因素 | 场景推断 |
| 经济约束 | 预算范围、付费意愿 | 明确描述 |
| 注意力约束 | 专注时长、注意力特点 | 年龄特征+场景 |

**时间可用性模式：**

```python
时间可用性分析 = {
    "碎片化": {
        "特征": "单次10-30分钟",
        "典型场景": ["通勤", "午休", "带娃间隙"],
        "内容策略": "短小精悍、独立完整"
    },
    "半连续": {
        "特征": "单次30-60分钟",
        "典型场景": ["晚间学习", "周末"],
        "内容策略": "模块化设计、可分段学习"
    },
    "连续性": {
        "特征": "单次1-2小时以上",
        "典型场景": ["脱产培训", "集中学习"],
        "内容策略": "深度讲解、系统学习"
    }
}
```

### 4. 知识基线评估

**知识水平分类：**

| 水平等级 | 特征描述 | 教学策略 |
|---------|---------|---------|
| 零基础 | 完全没有接触过 | 从零开始、慢节奏、多重复 |
| 入门级 | 了解基本概念 | 基础概念+简单应用 |
| 初级 | 有过实践经历 | 巩固基础+提升技巧 |
| 中级 | 能独立完成任务 | 深入原理+拓展应用 |
| 高级 | 专业水平 | 前沿知识+复杂案例 |

**前置知识识别：**

```python
前置知识识别 = 函数(
    课程主题,
    目标水平,
    知识图谱库
) -> {
    'required_prerequisites': [前置知识列表],
    'user_has': [用户已具备],
    'user_missing': [用户可能缺失],
    'gap_fill_strategy': [补齐策略]
}
```

**常见误解风险识别：**

| 领域 | 常见误解 | 识别关键词 |
|------|---------|---------|
| 心理学 | "孩子不听话就是有问题" | 正常发展阶段行为 |
| 编程 | "编程就是写代码" | 计算思维、问题分解 |
| 理财 | "省钱就是理财" | 资产配置、风险管理 |
| 营养 | "吃得少就健康" | 营养均衡、个体差异 |

### 5. 内容偏好建模

**偏好维度：**

| 偏好类型 | 选项 | 适用场景 |
|---------|------|---------|
| 概念深度 | 浅/中/深 | 理论讲解深度 |
| 案例比例 | 30%/50%/70% | 理论与案例配比 |
| 互动程度 | 低/中/高 | 提问、思考、练习 |
| 视觉风格 | 简约/丰富/专业 | PPT设计 |
| 节奏快慢 | 慢/中/快 | 语速、内容密度 |
| 幽默程度 | 严肃/适中/活泼 | 表达风格 |

**偏好推断矩阵：**

```python
内容偏好推断 = {
    "婴幼儿家长": {
        "conceptual_depth": "浅",
        "case_ratio": 0.7,
        "interactivity": "中",
        "visual_style": "温馨",
        "pace": "中",
        "humor": "适中"
    },
    "大学生": {
        "conceptual_depth": "中-深",
        "case_ratio": 0.4,
        "interactivity": "中",
        "visual_style": "专业",
        "pace": "中-快",
        "humor": "适中"
    },
    "职场从业者": {
        "conceptual_depth": "中",
        "case_ratio": 0.5,
        "interactivity": "低-中",
        "visual_style": "商务",
        "pace": "快",
        "humor": "低"
    },
    "企业管理者": {
        "conceptual_depth": "深",
        "case_ratio": 0.4,
        "interactivity": "中",
        "visual_style": "高端",
        "pace": "中",
        "humor": "低"
    }
}
```

## 输入规范

### 接受输入格式

**完整输入：**
```json
{
  "source": "user_description",
  "raw_text": "我想做一门帮助家长理解孩子心理的课程...",
  "explicit_audience": "婴幼儿/儿童家长",
  "course_requirement": {
    "course_name": "儿童发展心理学",
    "application_scenario": "家庭教育"
  }
}
```

**简化输入：**
```json
{
  "source": "natural_language_parser",
  "learner_description": "年轻妈妈，孩子3-6岁，希望了解孩子心理"
}
```

## 输出规范

### 标准输出格式

```json
{
  "learner_profile": {
    "profile_id": "LP_DEV_PSY_001",
    "generation_timestamp": "2024-01-15T10:00:00Z",
    "demographic": {
      "age_range": "25-40",
      "education_level": "本科及以上",
      "professional_background": "职场人士为主",
      "family_status": "有适龄子女"
    },
    "learning_motivation": {
      "primary": "问题解决型",
      "secondary": ["亲子关系改善", "孩子教育能力提升"],
      "intensity": "high",
      "urgency": "medium",
      "commitment_level": "willing"
    },
    "learning_constraints": {
      "time_available": {
        "pattern": "碎片化",
        "typical_session": "15-30分钟",
        "preferred_times": ["晚间", "周末"]
      },
      "device_preference": "mobile",
      "attention_span": "medium",
      "interruption_risk": "high"
    },
    "knowledge_baseline": {
      "domain_knowledge": "零基础",
      "prerequisite_knowledge": ["基本育儿常识"],
      "misconceptions_risk": [
        "发展阶段行为正常化",
        "避免过度解读孩子行为"
      ],
      "preferred_example_domain": ["亲子场景", "日常生活"]
    },
    "content_preference": {
      "conceptual_depth": "浅",
      "case_to_theory_ratio": "7:3",
      "interactivity_level": "medium",
      "visual_style": "温馨亲切",
      "pacing": "medium",
      "tone": "温暖鼓励"
    },
    "pain_points": [
      "孩子情绪不稳定时不知如何应对",
      "不了解孩子各阶段发展特点",
      "亲子沟通困难"
    ],
    "success_criteria": [
      "能理解孩子行为背后的心理需求",
      "掌握有效的亲子沟通方法",
      "能够识别正常发展与异常信号"
    ]
  },
  "profile_confidence": {
    "overall": 0.88,
    "demographic": 0.95,
    "motivation": 0.85,
    "constraints": 0.80,
    "preference": 0.82
  },
  "recommendations": [
    "建议增加实操性强的亲子互动技巧",
    "案例选择贴近3-6岁幼儿家庭场景",
    "提供可下载的练习工具"
  ]
}
```

## 学习者分群模型

### 1. 核心学习者类型

```python
学习者类型库 = {
    "焦虑型家长": {
        "特征": ["孩子问题突出", "急切寻求解决方案", "时间压力大"],
        "内容策略": ["快速见效技巧", "实操工具", "案例丰富"],
        "忌讳": ["过多理论基础", "解决方案复杂"]
    },
    "积极成长型家长": {
        "特征": ["主动学习", "注重系统知识", "愿意投入时间"],
        "内容策略": ["完整知识体系", "深度讲解", "拓展资源"],
        "忌讳": ["过于浅显", "缺乏深度"]
    },
    "职场充电型学员": {
        "特征": ["技能提升导向", "时间有限", "实用主义"],
        "内容策略": ["即学即用", "案例实操", "效率优先"],
        "忌讳": ["理论堆砌", "内容冗长"]
    },
    "学术探索型学员": {
        "特征": ["好奇心强", "追求理解", "享受学习过程"],
        "内容策略": ["完整推导", "多种视角", "思考问题"],
        "忌讳": ["跳过原理", "只给结论"]
    }
}
```

### 2. 类型自动识别

```python
def identify_learner_type(profile: LearnerProfile) -> str:
    scores = {}
    
    # 焦虑型检测
    if profile.motivation.primary == "问题解决型":
        if profile.motivation.urgency == "high":
            scores["焦虑型家长"] = 0.9
        else:
            scores["积极成长型家长"] = 0.7
    
    # 时间约束检测
    if profile.constraints.time_available.pattern == "碎片化":
        if profile.content_preference.interactivity_level == "low":
            scores["职场充电型学员"] = 0.8
    
    # 知识深度检测
    if profile.content_preference.conceptual_depth == "深":
        scores["学术探索型学员"] = 0.85
    
    return max(scores, key=scores.get)
```

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|---------|
| natural-language-course-parser | 学习者描述、受众定位 | JSON |
| course-requirement-validator | 校验后的需求结构 | JSON |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|---------|
| knowledge-graph-builder | 学习者类型、偏好权重 | JSON |
| lesson-script-generator | 内容深度、案例比例、风格参数 | JSON |
| ppt-template-selector | 视觉风格偏好 | JSON |
| virtual-teacher-configurator | 教学风格、互动程度 | JSON |

## 最佳实践

### 1. 画像完善策略

**最低信息量要求：**
- 必须明确：目标受众描述
- 推荐提供：学习动机、时间约束
- 可选推断：详细内容偏好

**画像渐进完善：**
```
初始画像（用户描述） → 推断画像（规则推理） → 验证画像（用户确认） → 最终画像
```

### 2. 多受众处理

**策略选择：**

| 策略 | 适用场景 | 处理方式 |
|------|---------|---------|
| 优先级排序 | 明确主次受众 | 按primary设计，secondary提供补充 |
| 双轨并行 | 受众差异大 | 制作双版本或提供切换 |
| 折中适配 | 差异可调和 | 取中间偏好值 |

**处理示例：**
```
输入: "给老师和学生看的编程课"
处理:
  - 识别primary: 学生（知识水平较低）
  - 识别secondary: 教师（需要教学补充）
  - 内容策略: 以学生为准设计基础内容，
              为教师提供教学指南和拓展材料
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 受众识别模糊 | 描述过于笼统 | 触发澄清对话 |
| 动机推断错误 | 描述有歧义 | 请求用户明确 |
| 偏好不一致 | 群体内差异大 | 分群处理或提供选项 |
| 画像置信度低 | 信息不足 | 引导补充信息或使用默认画像 |

## 技术实现框架

```python
class LearnerProfileAnalyzer:
    def __init__(self):
        self.demographic_extractor = DemographicExtractor()
        self.motivation_analyzer = MotivationAnalyzer()
        self.constraint_identifier = ConstraintIdentifier()
        self.preference_modeler = PreferenceModeler()
        self.type_classifier = LearnerTypeClassifier()
    
    def analyze(
        self,
        source_data: dict,
        context: dict = None
    ) -> LearnerProfile:
        # 1. 人口统计提取
        demographic = self.demographic_extractor.extract(source_data)
        
        # 2. 动机分析
        motivation = self.motivation_analyzer.analyze(source_data)
        
        # 3. 约束识别
        constraints = self.constraint_identifier.identify(source_data)
        
        # 4. 知识基线评估
        baseline = self.assess_knowledge_baseline(source_data)
        
        # 5. 内容偏好建模
        preference = self.preference_modeler.model(source_data, demographic)
        
        # 6. 类型分类
        learner_type = self.type_classifier.classify(
            demographic, motivation, preference
        )
        
        # 7. 画像组装
        profile = LearnerProfile(
            demographic=demographic,
            motivation=motivation,
            constraints=constraints,
            knowledge_baseline=baseline,
            content_preference=preference,
            learner_type=learner_type
        )
        
        # 8. 置信度评估
        profile.confidence = self.assess_confidence(profile, source_data)
        
        return profile
```

---

本技能通过多维度分析构建精准的学习者画像，为微课内容的个性化适配提供坚实基础，确保教学设计真正服务于目标受众的学习需求。
