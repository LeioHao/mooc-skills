---
name: "case-library-matcher"
description: "根据知识点特征和学习者画像从案例库中匹配适切案例，自动生成教学案例推荐列表。当需要为微课脚本匹配案例时使用此技能。"
---

# Case Library Matcher - 案例库匹配器

## 概述

本技能是微课视频生产线的内容增强技能，负责根据知识点特征和学习者画像从案例库中匹配适切的案例资源。通过多维度相似度计算和适切性评估，为每个知识点推荐最合适的教学案例，提升微课内容的生动性和实用性。

## 核心功能

### 1. 案例库管理

**案例元数据结构：**

```python
案例结构 = {
    "case_id": "CASE_001",
    "title": "2岁孩子的\"不要\"反抗期",
    "summary": "描述2岁孩子典型反抗行为及家长应对策略",
    "domain": {
        "primary": "儿童发展心理学",
        "secondary": ["家庭教育", "亲职教育"]
    },
    "age_group": "0-6岁",
    "scenario_type": "日常生活",
    "scenario_detail": "孩子吃饭时坚持要自己拿勺子",
    "characters": {
        "protagonist": "孩子（2岁）",
        "secondary": ["家长"]
    },
    "emotional_tags": ["困惑", "焦虑", "接纳", "成长"],
    "relevance_to_concepts": [
        "自主性发展",
        "第一反抗期",
        "自我意识萌芽"
    ],
    "teaching_value": {
        "practical_score": 0.9,
        "relatability_score": 0.95,
        "educational_depth": 0.7
    },
    "content_specs": {
        "duration_estimate": "2-3分钟",
        "suitable_for_voiceover": true,
        "visual_requirement": "生活场景插画或真实视频"
    },
    "sensitivity": {
        "level": "low",
        "concerns": []
    }
}
```

**案例分类体系：**

```python
案例分类 = {
    "按领域": {
        "教育类": ["课堂教学", "家庭教育", "职业教育"],
        "心理类": ["儿童心理", "青少年心理", "职场心理"],
        "技能类": ["操作演示", "方法讲解", "工具使用"],
        "健康类": ["医学常识", "营养健康", "运动健身"]
    },
    "按年龄段": {
        "0-3岁": "婴幼儿",
        "3-6岁": "幼儿",
        "6-12岁": "儿童",
        "12-18岁": "青少年",
        "18-45岁": "成年人",
        "45岁以上": "中老年"
    },
    "按场景": {
        "家庭场景": ["亲子互动", "日常起居", "学习陪伴"],
        "学校场景": ["课堂教学", "课外活动", "师生互动"],
        "职场场景": ["工作技能", "团队协作", "职业发展"],
        "社会场景": ["人际交往", "公共场合", "突发事件"]
    },
    "按情感": {
        "积极正面": ["成功案例", "成长故事", "正向激励"],
        "中性分析": ["现象描述", "对比分析", "方法讲解"],
        "问题导向": ["问题识别", "原因分析", "解决方案"]
    }
}
```

### 2. 多维度匹配算法

**匹配维度体系：**

| 匹配维度 | 计算方式 | 权重 | 说明 |
|---------|---------|------|------|
| 领域相关性 | 概念标签重叠度 | 0.30 | 知识点与案例主题的领域匹配 |
| 受众适切性 | 年龄/背景匹配度 | 0.25 | 案例情境与学习者的适切程度 |
| 情感共鸣度 | 情感标签匹配度 | 0.20 | 案例情感与教学风格的匹配 |
| 教学价值 | 实用/可读/深度评分 | 0.15 | 案例本身的教学质量 |
| 敏感度匹配 | 敏感度匹配度 | 0.10 | 案例敏感度与课程定位的匹配 |

**相似度计算公式：**

```python
def calculate_match_score(case, knowledge_point, learner_profile):
    # 领域相关性 (0-1)
    domain_score = len(
        set(case.domain) & set(knowledge_point.domains)
    ) / max(len(set(case.domain)), len(set(knowledge_point.domains)))
    
    # 受众适切性 (0-1)
    audience_score = audience_similarity(
        case.age_group, learner_profile.age_range
    ) * 0.6 + scenario_relevance(
        case.scenario_type, learner_profile.preferred_scenarios
    ) * 0.4
    
    # 情感共鸣度 (0-1)
    emotion_score = emotional_match(
        case.emotional_tags, learner_profile.learning_motivation
    )
    
    # 教学价值 (0-1)
    teaching_score = (
        case.teaching_value.practical_score * 0.4 +
        case.teaching_value.relatability_score * 0.4 +
        case.teaching_value.educational_depth * 0.2
    )
    
    # 敏感度匹配 (0-1)
    sensitivity_score = sensitivity_match(
        case.sensitivity.level, learner_profile.sensitivity_preference
    )
    
    # 加权总分
    total_score = (
        domain_score * 0.30 +
        audience_score * 0.25 +
        emotion_score * 0.20 +
        teaching_score * 0.15 +
        sensitivity_score * 0.10
    )
    
    return total_score
```

### 3. 智能推荐策略

**推荐策略库：**

```python
推荐策略 = {
    "精准匹配": {
        "条件": "领域和受众高度匹配",
        "输出": "返回top1推荐",
        "阈值": 0.85
    },
    "多元推荐": {
        "条件": "需要多角度讲解",
        "输出": "返回top3推荐，覆盖不同侧面",
        "阈值": 0.70
    },
    "补充推荐": {
        "条件": "精准匹配不足",
        "输出": "返回最接近的案例+相似案例",
        "阈值": 0.60
    },
    "兜底推荐": {
        "条件": "无合适匹配",
        "输出": "返回通用性最强的案例",
        "阈值": 0.40
    }
}
```

**案例组合策略：**

```python
案例组合规则 = {
    "概念讲解类": {
        "数量": 1-2,
        "类型": "现象描述型 + 问题分析型",
        "比例": "7:3"
    },
    "原理分析类": {
        "数量": 2-3,
        "类型": "理论验证型 + 应用示范型",
        "比例": "5:5"
    },
    "技能传授类": {
        "数量": 2-3,
        "类型": "步骤演示型 + 错误示范型",
        "比例": "6:4"
    },
    "综合应用类": {
        "数量": 3,
        "类型": "情境体验型 + 方法总结型 + 延伸思考型",
        "比例": "4:3:3"
    }
}
```

### 4. 案例增强生成

**案例扩展能力：**

| 扩展类型 | 输入 | 输出 |
|---------|------|------|
| 背景补充 | 案例核心情节 | 完整的场景描述 |
| 对话生成 | 案例人物 | 自然对话脚本 |
| 续写建议 | 案例结局 | 延伸思考问题 |
| 变体生成 | 基础案例 | 不同情境的变体案例 |

**自动生成提示：**

```python
案例增强生成 = {
    "背景扩展": {
        "prompt": "基于'{scenario}'场景，补充完整的时间、地点、人物背景...",
        "parameters": {
            "length": "medium",
            "detail_level": "high",
            "tone": "教学导向"
        }
    },
    "对话生成": {
        "prompt": "为'{scenario}'场景中的'{character}'生成自然对话...",
        "parameters": {
            "style": "口语化",
            "length": "2-3轮对话",
            "emotional_tone": "{emotion}"
        }
    },
    "问题生成": {
        "prompt": "基于案例'{case_title}'，生成3个引导学习者思考的问题...",
        "parameters": {
            "difficulty": "{difficulty_level}",
            "type": ["理解性", "应用性", "分析性"]
        }
    }
}
```

## 输入规范

### 接受输入格式

**单知识点匹配请求：**
```json
{
  "request_type": "single_knowledge_point",
  "knowledge_point": {
    "id": "KN_005",
    "name": "第一反抗期的表现",
    "type": "concept",
    "domains": ["儿童发展心理学", "家庭教育"],
    "teaching_context": {
      "difficulty": 2,
      "learner_age_group": "0-6岁家长",
      "teaching_objective": "帮助家长理解孩子反抗行为的正常性"
    }
  },
  "learner_profile": {
    "age_range": "25-40",
    "preferred_scenarios": ["亲子互动", "日常生活"],
    "sensitivity_preference": "low"
  },
  "match_options": {
    "max_results": 3,
    "min_score_threshold": 0.6
  }
}
```

**批量匹配请求：**
```json
{
  "request_type": "batch_matching",
  "knowledge_points": [
    {"id": "KN_005", "name": "第一反抗期"},
    {"id": "KN_006", "name": "自主性发展"}
  ],
  "learner_profile": {...},
  "strategy": "diversified",
  "max_cases_per_point": 2
}
```

## 输出规范

### 匹配结果格式

```json
{
  "matching_result": {
    "knowledge_point_id": "KN_005",
    "matching_strategy": "精准匹配",
    "total_cases_found": 5,
    "recommended_cases": [
      {
        "rank": 1,
        "case_id": "CASE_001",
        "match_score": 0.92,
        "case_title": "2岁孩子的\"不要\"反抗期",
        "match_reasons": [
          "领域完全匹配（儿童发展心理学）",
          "年龄群体高度匹配（0-6岁家长）",
          "情感标签与教学目标一致"
        ],
        "usage_recommendation": {
          "position": "核心案例",
          "duration": "完整讲述，约2-3分钟",
          "focus_point": "强调这是正常发展现象"
        },
        "enhanced_content": {
          "expanded_summary": "完整展开的案例描述...",
          "suggested_questions": [
            "您的孩子是否也有类似的'反抗'表现？",
            "当时您是如何应对的？"
          ]
        }
      },
      {
        "rank": 2,
        "case_id": "CASE_002",
        "match_score": 0.78,
        "case_title": "不肯刷牙的小明",
        "match_reasons": [
          "场景典型（日常行为管理）",
          "受众高度匹配"
        ],
        "usage_recommendation": {
          "position": "补充案例",
          "duration": "简略带过，约1分钟",
          "focus_point": "展示自主性发展的日常表现"
        }
      }
    ],
    "alternative_cases": [
      {
        "case_id": "CASE_003",
        "match_score": 0.65,
        "title": "选择困难的小美",
        "fallback_reason": "主题略有差异但可作为对比"
      }
    ]
  }
}
```

### 批量匹配结果格式

```json
{
  "batch_result": {
    "total_knowledge_points": 2,
    "total_cases_matched": 5,
    "redundancy_check": {
      "CASE_001": {
        "used_for": ["KN_005", "KN_006"],
        "recommendation": "可在KN_006中引用而非重复讲述"
      }
    },
    "coverage_analysis": {
      "all_covered": true,
      "unmatched_points": [],
      "coverage_rate": 1.0
    }
  }
}
```

## 匹配决策流程

### 1. 匹配流程

```
输入知识点和学习者画像
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤1: 候选案例检索                                          │
│ - 基于领域标签初步检索                                        │
│ - 基于年龄群体筛选                                            │
│ - 初步过滤敏感度不匹配的案例                                   │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤2: 多维度评分                                            │
│ - 计算领域相关性得分                                          │
│ - 计算受众适切性得分                                          │
│ - 计算情感共鸣度得分                                          │
│ - 计算教学价值得分                                            │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤3: 综合排序                                              │
│ - 加权计算总分                                                │
│ - 应用阈值过滤                                                │
│ - 去重处理（同一案例匹配多个知识点）                          │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤4: 智能推荐                                              │
│ - 选择最佳匹配策略                                            │
│ - 生成案例组合                                                │
│ - 提供使用建议                                                │
└─────────────────────────────────────────────────────────────┘
```

### 2. 去重与复用策略

```python
案例复用决策 = {
    "场景": "同一案例可匹配多个知识点",
    "策略": {
        "首次使用": "作为核心案例完整呈现",
        "后续引用": "以'回顾前文案例'方式简略带过",
        "组合优化": "优先选择不同案例覆盖不同知识点"
    },
    "输出标记": {
        "core": "核心案例，完整呈现",
        "reference": "引用案例，简短提及",
        "alternative": "备选案例，可替换使用"
    }
}
```

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|------|
| knowledge-graph-builder | 知识点信息、领域标签 | JSON |
| learner-profile-analyzer | 学习者画像、受众特征 | JSON |
| lesson-script-generator | 脚本上下文、教学目标 | JSON |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| lesson-script-generator | 匹配好的案例列表 | JSON |
| ppt-template-selector | 案例类型信息（影响模板选择） | JSON |

## 最佳实践

### 1. 案例库建设

**案例质量标准：**

| 质量维度 | 标准要求 | 检验方法 |
|---------|---------|---------|
| 真实性 | 来自真实场景或经过验证 | 来源标注 |
| 适切性 | 与目标受众匹配 | 试讲反馈 |
| 教学性 | 有明确的教学价值 | 专家评审 |
| 版权合规 | 无侵权风险 | 法律审核 |
| 时效性 | 内容不过时 | 定期更新 |

**案例扩充策略：**

```python
案例扩充计划 = {
    "基础库": "专家设计的标准化案例（50-100个）",
    "领域库": "按学科领域扩充（每个领域20-30个）",
    "场景库": "按教学场景扩充（每场景10-20个）",
    "用户贡献": "用户反馈+优化的UGC案例"
}
```

### 2. 匹配优化

**提升匹配精度：**

```python
精度提升策略 = {
    "标签优化": "定期维护和扩展标签体系",
    "评分校准": "基于使用反馈调整权重",
    "冷启动处理": "新案例优先推荐试用意愿高的用户",
    "长尾覆盖": "为低频知识点储备专项案例"
}
```

### 3. 案例使用建议

**使用场景示例：**

```
知识点: "埃里克森心理社会发展阶段"
├─ 核心案例: CASE_010 "小红在幼儿园的社交挑战"
│   └─ 使用: 完整讲述约2分钟，配合阶段理论讲解
│
├─ 对比案例: CASE_011 "小强处理冲突的方式"
│   └─ 使用: 简略提及，作为对比分析
│
└─ 延伸案例: CASE_012 "青春期Identity vs Role Confusion"
    └─ 使用: 课后思考题引导学习者自行查阅
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 无合适案例 | 案例库不足 | 触发案例生成或降低匹配阈值 |
| 多个高分案例 | 边界模糊 | 返回多个推荐，让脚本生成器选择 |
| 案例与脚本冲突 | 风格不匹配 | 提供案例变体或调整脚本风格 |
| 敏感案例被推荐 | 过滤规则不足 | 加强敏感度过滤，调整权重 |

## 技术实现框架

```python
class CaseLibraryMatcher:
    def __init__(self):
        self.case_index = CaseIndex()
        self.similarity_engine = SimilarityEngine()
        self.recommendation_strategy = RecommendationStrategy()
        self.case_enhancer = CaseEnhancer()
    
    def match(
        self,
        knowledge_point: dict,
        learner_profile: dict,
        options: dict = None
    ) -> MatchResult:
        # 1. 候选检索
        candidates = self.case_index.search(
            domain=knowledge_point.domains,
            age_group=learner_profile.age_range
        )
        
        # 2. 多维度评分
        scored_cases = []
        for case in candidates:
            score = self.similarity_engine.calculate(
                case, knowledge_point, learner_profile
            )
            scored_cases.append((case, score))
        
        # 3. 排序过滤
        ranked_cases = sorted(
            scored_cases,
            key=lambda x: x[1],
            reverse=True
        )
        filtered_cases = self.apply_threshold(ranked_cases, options)
        
        # 4. 策略推荐
        recommended = self.recommendation_strategy.recommend(
            filtered_cases, knowledge_point, options
        )
        
        # 5. 案例增强
        enhanced = self.case_enhancer.enhance(recommended)
        
        return MatchResult(
            knowledge_point_id=knowledge_point.id,
            recommended_cases=enhanced
        )
```

---

本技能通过智能化的案例匹配，确保每个知识点都能获得适切、生动、富有教学价值的案例支撑，大幅提升微课内容的吸引力和学习效果。
