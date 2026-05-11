---
name: "mooc-standard-validator"
description: "检验微课视频是否符合MOOC标准规范，包括时长标准、知识点聚焦、逻辑结构、受众适配等教育标准。当需要验证微课是否符合MOOC发布标准时使用此技能。"
---

# MOOC Standard Validator - MOOC标准合规检验器

## 概述

本技能是微课视频生产线的最终质量认证技能，负责对微课内容和技术质量进行MOOC标准的综合验证。通过标准化的MOOC规范库和多维度合规性评估，确保微课满足主流MOOC平台的质量要求，为课程发布提供权威的合规性认证。

## 核心功能

### 1. MOOC标准规范库

**标准分类体系：**

```python
MOOC标准体系 = {
    "时长标准": {
        "min_duration": 300,      # 5分钟
        "max_duration": 900,       # 15分钟
        "optimal_duration": 480,  # 8分钟
        "tolerance": 0.1,         # 10%偏差容忍
        "permissible_variations": {
            "短视频": "3-5分钟（适用于知识点精讲）",
            "标准课": "5-15分钟（常规微课）",
            "长课": "15-20分钟（需分章节，可接受上限）"
        }
    },
    "知识点标准": {
        "max_concepts_per_lesson": 2,
        "max_learning_objectives": 3,
        "concept_clarity": "每节一个核心概念清晰聚焦",
        "depth_requirement": "深入讲解，避免蜻蜓点水"
    },
    "结构标准": {
        "must_have": ["opening", "body", "summary"],
        "recommended": ["preview", "interaction", "reflection"],
        "opening_ratio": "5-10%",
        "body_ratio": "80-85%",
        "summary_ratio": "10-15%"
    },
    "教学标准": {
        "interactivity_min": 1,        # 每节至少1个互动点
        "case_ratio_min": 0.3,        # 案例占比最低30%
        "visual_elements": "每节至少2-3个视觉元素",
        "assessment_type": "形成性或总结性"
    }
}
```

**平台适配标准：**

```python
平台标准适配 = {
    "学堂在线": {
        "时长": "5-15分钟",
        "分辨率": "1920x1080",
        "格式": "MP4/H.264",
        "字幕": "必须",
        "知识点的数量": "≤3个"
    },
    "中国大学MOOC": {
        "时长": "5-20分钟",
        "分辨率": "1280x720起",
        "格式": "MP4",
        "字幕": "建议",
        "知识点": "单一知识点为佳"
    },
    "智慧树": {
        "时长": "10-15分钟",
        "分辨率": "1920x1080",
        "格式": "MP4/H.264",
        "字幕": "必须",
        "交互": "建议有"
    },
    "通用标准": {
        "时长": "5-15分钟",
        "分辨率": "1920x1080",
        "格式": "MP4",
        "字幕": "必须",
        "知识点": "≤2个"
    }
}
```

### 2. 时长合规性检验

**检验规则：**

| 时长范围 | 评估结果 | 处理建议 |
|---------|---------|---------|
| 5-15分钟 | 合规 | 直接通过 |
| 3-5分钟 | 基本合规 | 建议补充内容或作为系列课 |
| 15-20分钟 | 基本合规 | 建议拆分或标注为长课时 |
| <3分钟 | 不合规 | 必须补充内容 |
| >20分钟 | 不合规 | 必须拆分 |

**时长评估算法：**

```python
时长合规评估 = 函数(
    实际时长,
    目标时长,
    课程类型
) -> {
    "合规状态": "passed/warning/failed",
    "偏差比例": 实际时长 / 目标时长,
    "评估详情": {...},
    "建议": [...]
}

评估规则:
  - 偏差≤10%: 优秀
  - 偏差10-20%: 合格
  - 偏差>20%: 不合格
```

### 3. 知识点聚焦检验

**检验维度：**

```python
知识点聚焦检验 = {
    "单一聚焦度": {
        "标准": "每节微课聚焦1个核心知识点",
        "评分": {
            "1个核心": "优秀",
            "2个核心": "良好",
            "3个核心": "合格",
            ">3个": "不合格"
        }
    },
    "学习目标明确度": {
        "标准": "每节有清晰的学习目标",
        "检查项": [
            "开场明确告知学习目标",
            "内容围绕目标展开",
            "结尾回顾目标达成情况"
        ]
    },
    "深度充分性": {
        "标准": "知识点讲解深入透彻",
        "评估": "是否避免蜻蜓点水"
    }
}
```

### 4. 结构完整性检验

**结构检查项：**

| 结构部分 | 必须要素 | 建议要素 | 评分权重 |
|---------|---------|---------|---------|
| 开场 | 学习目标说明、课程引入 | 互动提问、激发兴趣 | 15% |
| 主体 | 核心内容讲解、重点强调 | 案例分析、互动讨论 | 70% |
| 结尾 | 知识点回顾、总结 | 思考问题、下节预告 | 15% |

**结构比例规范：**

```python
结构比例规范 = {
    "开场部分": {
        "标准比例": "5-10%",
        "可接受范围": "3-15%",
        "不足处理": "补充学习目标说明",
        "过长处理": "精简引入内容"
    },
    "主体部分": {
        "标准比例": "80-85%",
        "可接受范围": "70-90%",
        "不足处理": "补充核心内容",
        "过长处理": "考虑拆分或精简"
    },
    "结尾部分": {
        "标准比例": "10-15%",
        "可接受范围": "5-20%",
        "不足处理": "补充总结回顾",
        "过长处理": "精简收尾内容"
    }
}
```

### 5. 受众适配性检验

**适配性评估维度：**

| 评估维度 | 检验方法 | 标准 | 自动化程度 |
|---------|---------|------|----------|
| 难度适切性 | 与受众知识基线比对 | 难度与受众匹配 | 自动 |
| 语言适切性 | 术语解释检测 | 首次出现术语有解释 | 自动 |
| 案例适切性 | 案例与受众场景匹配 | 匹配度≥80% | 半自动 |
| 节奏适切性 | 语速与注意力匹配 | 符合目标受众 | 自动 |
| 风格适切性 | 风格与受众偏好匹配 | 匹配度≥75% | 自动 |

**适配性评分算法：**

```python
适配性评分 = {
    "难度匹配": {
        "权重": 0.30,
        "评估": "内容难度与受众知识水平匹配程度"
    },
    "语言适配": {
        "权重": 0.20,
        "评估": "专业术语解释充分性，语言通俗易懂程度"
    },
    "案例适配": {
        "权重": 0.25,
        "评估": "案例情境与受众生活场景的贴近程度"
    },
    "节奏适配": {
        "权重": 0.15,
        "评估": "语速、内容密度与受众注意力特点的匹配程度"
    },
    "风格适配": {
        "权重": 0.10,
        "评估": "整体教学风格与受众偏好的一致程度"
    }
}
```

### 6. 综合认证评估

**认证等级划分：**

```python
MOOC认证等级 = {
    "优秀": {
        "总分": "≥90分",
        "条件": "所有维度≥80分",
        "认证标识": "MOOC认证·优秀"
    },
    "良好": {
        "总分": "80-89分",
        "条件": "所有维度≥70分",
        "认证标识": "MOOC认证·良好"
    },
    "合格": {
        "总分": "70-79分",
        "条件": "所有维度≥60分",
        "认证标识": "MOOC认证·合格"
    },
    "待改进": {
        "总分": "<70分",
        "条件": "任一维度<60分",
        "认证标识": "需要改进"
    }
}
```

## 输入规范

### 接受输入格式

**完整检验请求：**
```json
{
  "validation_type": "full_validation",
  "lesson_info": {
    "lesson_id": "L_01_01",
    "title": "什么是发展心理学",
    "course_name": "儿童发展心理学",
    "module": "第一章 概述"
  },
  "lesson_content": {
    "duration": 520,
    "target_duration": 480,
    "knowledge_points": ["KN_001"],
    "learning_objectives": ["理解发展心理学定义", "识别研究对象"],
    "structure": {
      "opening_duration": 45,
      "body_duration": 420,
      "summary_duration": 55
    }
  },
  "quality_reports": {
    "content_quality": {...},
    "technical_quality": {...}
  },
  "target_platform": "通用MOOC标准",
  "learner_profile": {
    "target_audience": "婴幼儿家长",
    "knowledge_baseline": "零基础"
  },
  "validation_options": {
    "strict_mode": false,
    "include_platform_specific": false
  }
}
```

**快速检验请求：**
```json
{
  "validation_type": "quick_check",
  "lesson_id": "L_01_01",
  "duration": 520,
  "knowledge_count": 1,
  "has_opening": true,
  "has_body": true,
  "has_summary": true
}
```

## 输出规范

### 验证结果格式

```json
{
  "mooc_validation_result": {
    "validation_id": "MOOC_VAL_001",
    "lesson_id": "L_01_01",
    "validation_timestamp": "2024-01-15T10:00:00Z",
    "target_platform": "通用MOOC标准",
    "overall_score": 88,
    "certification_level": "良好",
    "certification_status": "passed"
  },
  "dimension_scores": {
    "duration_compliance": {
      "score": 95,
      "status": "passed",
      "actual_duration": 520,
      "target_duration": 480,
      "deviation": 0.083,
      "grade": "优秀"
    },
    "knowledge_focus": {
      "score": 90,
      "status": "passed",
      "knowledge_point_count": 1,
      "focus_assessment": "单一知识点聚焦优秀"
    },
    "structure_completeness": {
      "score": 85,
      "status": "passed",
      "opening_ratio": 0.087,
      "body_ratio": 0.808,
      "summary_ratio": 0.106,
      "structure_assessment": "结构比例合理"
    },
    "learner_adaptation": {
      "score": 88,
      "status": "passed",
      "difficulty_fit": 0.90,
      "language_fit": 0.85,
      "case_fit": 0.92,
      "pace_fit": 0.88
    },
    "teaching_standards": {
      "score": 82,
      "status": "passed",
      "interactivity_count": 2,
      "case_ratio": 0.65,
      "visual_elements": 3
    }
  },
  "compliance_checklist": {
    "duration_standard": {
      "status": "passed",
      "details": "5-15分钟标准范围内"
    },
    "knowledge_focus": {
      "status": "passed",
      "details": "聚焦单一核心知识点"
    },
    "structure_standard": {
      "status": "passed",
      "details": "包含开场-主体-结尾"
    },
    "subtitle_standard": {
      "status": "passed",
      "details": "字幕准确率98.5%"
    },
    "technical_standard": {
      "status": "passed",
      "details": "1080p分辨率，H.264编码"
    }
  },
  "issues": [],
  "warnings": [
    {
      "type": "duration_deviation",
      "severity": "low",
      "description": "实际时长比目标时长多8%，在可接受范围内"
    }
  ],
  "certification": {
    "certification_id": "MOOC_CERT_2024_001",
    "level": "良好",
    "issued_date": "2024-01-15",
    "valid_until": "2025-01-15",
    "standards_version": "MOOC_STD_V2.0",
    "qr_code": "..."
  },
  "recommendations": [
    {
      "priority": "low",
      "category": "optimization",
      "description": "可考虑在结尾部分增加下节预告"
    }
  ]
}
```

### 批量验证报告

```json
{
  "batch_validation_result": {
    "course_id": "DEV_PSY_001",
    "course_name": "儿童发展心理学",
    "total_lessons": 24,
    "validation_summary": {
      "passed": 22,
      "passed_with_warning": 2,
      "failed": 0,
      "pass_rate": 1.0
    },
    "average_scores": {
      "duration_compliance": 92,
      "knowledge_focus": 88,
      "structure_completeness": 85,
      "learner_adaptation": 86,
      "teaching_standards": 83
    },
    "overall_course_score": 86.8,
    "course_certification_level": "良好"
  },
  "detailed_results": [
    {...每节课详细结果...}
  ],
  "course_recommendations": [
    {
      "priority": "medium",
      "category": "consistency",
      "description": "建议统一全课程的开场和结尾风格"
    }
  ]
}
```

## 验证决策流程

### 1. 验证执行流程

```
接收验证请求
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤1: 基础标准检验                                          │
│ - 时长合规性检测                                             │
│ - 分辨率/格式等技术指标                                       │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤2: 内容标准检验                                          │
│ - 知识点聚焦度检验                                           │
│ - 学习目标明确性检验                                         │
│ - 内容深度评估                                               │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤3: 结构标准检验                                          │
│ - 结构完整性检测                                             │
│ - 结构比例分析                                               │
│ - 过渡衔接评估                                               │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤4: 适配性检验                                           │
│ - 难度适切性评估                                             │
│ - 语言适切性评估                                             │
│ - 案例适切性评估                                             │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤5: 综合评分与认证                                        │
│ - 维度评分汇总                                               │
│ - 认证等级判定                                               │
│ - 认证证书生成                                               │
└─────────────────────────────────────────────────────────────┘
```

### 2. 认证决策矩阵

```python
认证决策 = {
    "完全通过": {
        "条件": "所有维度≥80分，无阻断问题",
        "结果": "直接颁发认证证书"
    },
    "有条件通过": {
        "条件": "所有维度≥60分，存在可修复问题",
        "结果": "颁发证书，问题记录在案"
    },
    "待改进": {
        "条件": "任一维度<60分",
        "结果": "不颁发证书，返回改进"
    },
    "需复审": {
        "条件": "高风险问题或评分置信度低",
        "结果": "人工专家复审"
    }
}
```

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|------|
| content-quality-checker | 内容质量维度得分 | JSON |
| technical-quality-checker | 技术质量维度得分 | JSON |
| lesson-script-generator | 脚本结构和时长信息 | JSON |
| learner-profile-analyzer | 学习者画像信息 | JSON |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| 生产线编排器 | 认证结果和证书 | JSON |
| 课程发布系统 | 认证状态 | 状态码 |

## 最佳实践

### 1. 验证策略

```python
验证策略 = {
    "单课验证": {
        "适用": "单节微课发布前",
        "流程": "全面验证，颁发单课证书"
    },
    "模块验证": {
        "适用": "一个模块完成后",
        "流程": "模块内一致性检查+单课验证"
    },
    "课程验证": {
        "适用": "整门课程完成后",
        "流程": "全课程验证+一致性检查+课程证书"
    }
}
```

### 2. 平台适配建议

```python
平台适配建议 = {
    "发布多平台": {
        "策略": "以最严格标准制作",
        "适用": "计划发布到多个平台"
    },
    "单一平台": {
        "策略": "按目标平台标准制作",
        "方法": "参考平台标准适配库"
    },
    "灵活适配": {
        "策略": "制作基础版本+平台定制",
        "适用": "有定制化需求的课程"
    }
}
```

### 3. 持续质量监控

```python
持续监控 = {
    "证书有效期": "1年",
    "年度复核": "每年复核内容时效性",
    "问题追溯": "保留原始验证数据",
    "标准更新": "随MOOC标准更新同步更新"
}
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 验证超时 | 视频过长 | 分段验证 |
| 评分异常 | 引用数据缺失 | 补充数据后重验 |
| 认证失败 | 多项不达标 | 根据问题反馈逐一修复 |
| 标准冲突 | 多平台标准不一致 | 选择最严格标准 |

## 技术实现框架

```python
class MOOCStandardValidator:
    def __init__(self):
        self.duration_checker = DurationChecker()
        self.knowledge_focus_checker = KnowledgeFocusChecker()
        self.structure_analyzer = StructureAnalyzer()
        self.adaptation_evaluator = AdaptationEvaluator()
        self.certification_engine = CertificationEngine()
        self.standard_library = StandardLibrary()
    
    def validate(
        self,
        lesson_info: dict,
        quality_reports: dict,
        learner_profile: dict,
        options: dict = None
    ) -> MOOCValidationResult:
        # 1. 获取适用的MOOC标准
        standards = self.standard_library.get_standards(
            options.get('target_platform', 'generic')
        )
        
        # 2. 时长合规性检验
        duration_result = self.duration_checker.check(
            lesson_info, standards
        )
        
        # 3. 知识点聚焦检验
        knowledge_result = self.knowledge_focus_checker.check(
            lesson_info, quality_reports
        )
        
        # 4. 结构完整性检验
        structure_result = self.structure_analyzer.analyze(
            lesson_info, standards
        )
        
        # 5. 适配性检验
        adaptation_result = self.adaptation_evaluator.evaluate(
            quality_reports, learner_profile
        )
        
        # 6. 综合认证
        certification = self.certification_engine.certify(
            duration_result,
            knowledge_result,
            structure_result,
            adaptation_result,
            standards
        )
        
        return MOOCValidationResult(
            scores={...},
            certification=certification,
            issues={...}
        )
```

---

本技能作为MOOC标准的最终验证关卡，确保每个微课都达到专业MOOC平台的质量要求，为学习者提供符合教育标准的高质量学习内容。
