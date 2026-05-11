---
name: "course-requirement-validator"
description: "校验课程需求的完整性、一致性和可行性，在正式生产前识别潜在问题并提供改进建议。确保需求符合MOOC标准和生产约束。"
---

# Course Requirement Validator - 课程需求校验器

## 概述

本技能是微课视频生产线的质量门禁技能，负责在正式进入生产流程前，对课程需求进行多维度校验。通过完整性检查、一致性验证和可行性评估，提前识别潜在问题并提供改进建议，避免生产过程中的返工和资源浪费。

## 核心功能

### 1. 完整性校验

**必填字段检查：**

| 字段类型 | 字段名称 | 校验规则 | 缺失处理 |
|---------|---------|---------|---------|
| 基础信息 | course_name | 非空，长度2-50字 | 触发追问 |
| 基础信息 | target_domain | 学科领域分类 | 触发追问 |
| 基础信息 | difficulty_level | 入门/进阶/高级 | 自动推断或追问 |
| 受众信息 | learner_profile | 包含人群描述 | 触发追问 |
| 受众信息 | knowledge_baseline | 零基础/有基础 | 自动推断或追问 |
| 时长信息 | lesson_duration | min/max/optimal | MOOC标准或追问 |
| 场景信息 | application_scenario | 自主学习/课堂教学/培训 | 自动推断或追问 |

**信息完整度评估：**

```python
完整度评分 = 函数(
    已提供字段数 / 必填字段数 * 权重系数
)

完整度等级:
  - >= 95%: 完整，无需追问
  - 80-95%: 基本完整，可选追问
  - < 80%: 不完整，必须追问
```

### 2. 一致性校验

**逻辑一致性检查：**

| 检查项 | 规则描述 | 冲突处理 |
|-------|---------|---------|
| 难度与受众匹配 | 高级课程不能面向零基础 | 警告+建议调整 |
| 时长与内容匹配 | 深入讲解需要更长时长 | 建议拆分或延长时间 |
| 风格与受众匹配 | 学术风格不能面向儿童 | 警告+建议调整 |
| 场景与内容匹配 | 碎片化学习场景需要短时长 | 建议拆分内容 |

**一致性规则库：**

```python
一致性规则 = {
    "difficulty_audience": {
        "高级+零基础": {
            "severity": "error",
            "message": "高级课程内容不适合零基础学习者",
            "suggestion": "建议调整为入门级课程，或明确学习者需要具备的基础知识"
        },
        "入门级+专业人士": {
            "severity": "warning",
            "message": "入门级内容可能无法满足专业人士需求",
            "suggestion": "建议考虑提供进阶补充内容"
        }
    },
    "duration_content": {
        "短时长+深度内容": {
            "severity": "warning",
            "message": "短时长难以深入讲解复杂内容",
            "suggestion": "建议拆分为一组微课系列"
        },
        "超长时长+简单内容": {
            "severity": "warning",
            "message": "简单内容不需要过长时长",
            "suggestion": "建议精简内容或增加知识点"
        }
    },
    "style_audience": {
        "学术风格+儿童": {
            "severity": "error",
            "message": "学术风格不适合儿童学习者",
            "suggestion": "建议调整为活泼、生动的教学风格"
        },
        "游戏化+企业培训": {
            "severity": "warning",
            "message": "游戏化风格可能不适合严肃的企业培训场景",
            "suggestion": "建议调整为正式但不失亲和的风格"
        }
    }
}
```

### 3. 可行性校验

**生产约束检查：**

| 约束类型 | 限制条件 | 校验逻辑 |
|---------|---------|---------|
| 时长约束 | min≥3分钟, max≤20分钟 | 超出范围触发建议 |
| 课程规模 | 总课时≤100节 | 超规模触发拆分建议 |
| 内容密度 | 每分钟≥30字内容 | 低于阈值警告 |
| 案例数量 | 每节≥1个案例 | 低于阈值警告 |
| 互动设计 | 每节≥1个互动点 | 低于阈值警告 |

**MOOC标准校验：**

```python
MOOC标准校验 = {
    "duration_standard": {
        "min": 300,    # 5分钟
        "max": 900,    # 15分钟
        "optimal": 480, # 8分钟
        "tolerance": 0.1  # 10%偏差容忍
    },
    "knowledge_focus": {
        "max_concepts_per_lesson": 2,
        "max_objectives_per_lesson": 3
    },
    "structure_requirement": {
        "must_have": ["opening", "body", "summary"],
        "recommended": ["preview", "interaction"]
    }
}
```

### 4. 风险评估

**风险识别类型：**

| 风险类型 | 识别特征 | 影响程度 | 应对策略 |
|---------|---------|---------|---------|
| 内容风险 | 敏感话题、专业争议 | 高 | 增加审核环节 |
| 版权风险 | 引用未授权内容 | 高 | 使用原创或授权素材 |
| 技术风险 | 特殊动画、交互需求 | 中 | 提前技术验证 |
| 受众风险 | 受众定位模糊 | 中 | 明确受众画像 |
| 时效风险 | 时效性强的内容 | 低 | 标注时效范围 |

**风险评分计算：**

```python
风险评分 = 函数(
    风险类型,
    风险概率,
    影响程度
) -> 风险等级

风险等级阈值:
  - 高风险 (>0.7): 必须解决才能继续
  - 中风险 (0.4-0.7): 建议解决，可继续
  - 低风险 (<0.4): 记录监控，可继续
```

## 输入规范

### 接受输入格式

```json
{
  "course_requirement": {
    "course_name": "儿童发展心理学",
    "target_domain": "教育学-心理学类",
    "difficulty_level": "入门级",
    "learner_profile": {
      "primary": "婴幼儿家长",
      "knowledge_baseline": "零基础"
    },
    "lesson_duration_preference": {
      "min": 5,
      "max": 10,
      "optimal": 8
    },
    "teaching_style": {
      "case_based": true,
      "case_ratio": 0.7
    }
  },
  "validation_context": {
    "course_scale": 20,
    "has_outline": true,
    "has_reference_materials": false
  }
}
```

## 输出规范

### 校验结果格式

```json
{
  "validation_result": {
    "status": "passed_with_suggestions",
    "overall_score": 85,
    "completeness_score": 90,
    "consistency_score": 85,
    "feasibility_score": 80
  },
  "field_validation": [
    {
      "field": "course_name",
      "status": "valid",
      "value": "儿童发展心理学"
    },
    {
      "field": "target_domain",
      "status": "valid",
      "value": "教育学-心理学类"
    }
  ],
  "consistency_issues": [
    {
      "type": "difficulty_audience_mismatch",
      "severity": "warning",
      "message": "入门级内容对婴幼儿家长可能过于简单",
      "details": "建议调整为进阶级别或增加实用性内容",
      "suggestion": "可以考虑在基础概念讲解后，增加实际应用场景"
    }
  ],
  "feasibility_issues": [
    {
      "type": "duration_content_mismatch",
      "severity": "warning",
      "message": "8分钟时长难以完整讲解'埃里克森心理社会发展理论'",
      "suggestion": "建议拆分为2节微课：第一部分理论框架，第二部分各阶段详解"
    }
  ],
  "risk_assessment": {
    "overall_risk_level": "low",
    "risks": [
      {
        "type": "content_sensitivity",
        "level": "low",
        "description": "儿童心理发展属于敏感领域，需确保内容科学准确"
      }
    ]
  },
  "recommendations": [
    "建议增加每节课的案例数量，案例比例提升至70%以上",
    "建议增加亲子互动设计环节"
  ],
  "next_actions": [
    {
      "action": "confirm_difficulty_level",
      "priority": "medium",
      "description": "请确认课程难度级别是否符合预期"
    }
  ]
}
```

## 校验决策流程

### 1. 校验执行顺序

```
输入需求
    ↓
┌─────────────────────────────────────┐
│ 步骤1: 完整性检查                     │
│ - 提取所有必填字段                    │
│ - 计算完整度得分                      │
│ - 识别缺失字段                        │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ 步骤2: 一致性检查                     │
│ - 执行所有一致性规则                  │
│ - 识别冲突项                          │
│ - 评估影响程度                        │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ 步骤3: 可行性检查                     │
│ - 验证生产约束                        │
│ - 评估技术可行性                      │
│ - 识别风险因素                        │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ 步骤4: 综合评估                       │
│ - 计算综合得分                        │
│ - 制定改进建议                        │
│ - 确定是否可以进入生产                │
└─────────────────────────────────────┘
```

### 2. 门禁决策

| 综合状态 | 含义 | 后续动作 |
|---------|------|---------|
| passed | 校验通过 | 直接进入生产流程 |
| passed_with_suggestions | 通过，有优化建议 | 进入生产，可选采纳建议 |
| needs_revision | 需要修订 | 返回上游修订后重新校验 |
| needs_clarification | 需要澄清 | 触发用户追问 |
| blocked | 阻塞，无法继续 | 列出必须解决的问题 |

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|------|
| natural-language-course-parser | 解析后的课程需求 | JSON |
| 用户直接输入 | 半结构化需求描述 | JSON/文本 |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| learner-profile-analyzer | 校验后的需求 | JSON |
| knowledge-graph-builder | 校验通过的需求 | JSON |
| 生产线编排器 | 校验结果状态 | 状态码 |

## 最佳实践

### 1. 校验时机

**强制校验节点：**
- 自然语言解析完成后
- 用户确认需求前
- 知识体系构建前
- 批量生产启动前

**建议校验节点：**
- 每完成一轮追问后
- 课程大纲调整后
- 目标受众变更后

### 2. 校验结果处理

**用户可接受的低严重度问题：**
- warning级别的风格匹配
- 可选字段的缺失
- 建议性优化

**必须解决的高严重度问题：**
- error级别的一致性冲突
- 必填字段缺失
- 生产约束违反

### 3. 渐进式校验

```python
渐进式校验流程 = {
    "快速预检": "只检查最关键字段（course_name, learner_profile）",
    "标准校验": "完整校验所有字段和一致性规则",
    "深度校验": "增加风险评估和技术可行性验证"
}
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 校验超时 | 规则库过大 | 优化规则匹配算法 |
| 误判一致性问题 | 规则过于严格 | 调整阈值或增加例外规则 |
| 校验结果不一致 | 多版本规则库 | 统一规则版本管理 |
| 无法识别特殊需求 | 规则库缺失 | 扩展规则库或人工介入 |

## 技术实现框架

```python
class CourseRequirementValidator:
    def __init__(self):
        self.completeness_checker = CompletenessChecker()
        self.consistency_validator = ConsistencyValidator()
        self.feasibility_evaluator = FeasibilityEvaluator()
        self.risk_assessor = RiskAssessor()
        self.rule_engine = RuleEngine()
    
    def validate(
        self,
        requirement: dict,
        validation_level: str = "standard"
    ) -> ValidationResult:
        # 1. 完整性检查
        completeness = self.completeness_checker.check(requirement)
        
        # 2. 一致性验证
        consistency = self.consistency_validator.validate(requirement)
        
        # 3. 可行性评估
        feasibility = self.feasibility_evaluator.evaluate(requirement)
        
        # 4. 风险评估
        risk = self.risk_assessor.assess(requirement)
        
        # 5. 综合决策
        result = self.make_decision(
            completeness, consistency, feasibility, risk
        )
        
        return result
```

---

本技能作为生产线的质量门禁，确保进入生产的课程需求都是完整、一致且可行的，从源头把控微课视频的生产质量。
