---
name: "virtual-teacher-configurator"
description: "根据课程定位和教学风格选择和配置虚拟教师形象，包括外貌特征、性格设定和教学参数。当需要为微课配置虚拟教师时使用此技能。"
---

# Virtual Teacher Configurator - 虚拟教师配置器

## 概述

本技能是微课视频生产线的虚拟形象设计技能，负责根据课程定位、教学风格和目标受众为微课选择和配置最合适的虚拟教师形象。通过标准化的虚拟教师形象库和参数化配置系统，确保虚拟教师的视觉呈现和教学风格与课程内容高度匹配。

## 核心功能

### 1. 虚拟教师类型体系

**预设虚拟教师模板库：**

```python
虚拟教师模板库 = {
    "T001_学者型男": {
        "定位": "学术课程、专业培训",
        "外貌特征": {
            "age_appearance": "中年男性",
            "appearance_style": "戴眼镜、正装风格",
            "expression": "严肃中带有亲和",
            "hair_style": "短发，略显花白"
        },
        "性格特征": {
            "professionalism": 0.9,
            "authoritativeness": 0.85,
            "friendliness": 0.6,
            "humor": 0.3
        },
        "教学风格": {
            "tone": "正式严谨",
            "pacing": "中等",
            "interactivity": "适中",
            "example_usage": "学术理论讲解"
        },
        "适用课程": ["高等教育", "专业认证", "学术研究"]
    },
    "T002_知性女性": {
        "定位": "通识课程、职业教育",
        "外貌特征": {
            "age_appearance": "青年女性",
            "appearance_style": "职业休闲装",
            "expression": "温和亲切",
            "hair_style": "简洁短发或马尾"
        },
        "性格特征": {
            "professionalism": 0.75,
            "authoritativeness": 0.7,
            "friendliness": 0.85,
            "humor": 0.5
        },
        "教学风格": {
            "tone": "友好专业",
            "pacing": "适中",
            "interactivity": "较高",
            "example_usage": "通识知识讲解"
        },
        "适用课程": ["通识教育", "技能培训", "职场学习"]
    },
    "T003_活力讲师": {
        "定位": "技能培训、青少年教育",
        "外貌特征": {
            "age_appearance": "青年男性",
            "appearance_style": "商务休闲",
            "expression": "充满活力",
            "hair_style": "时尚短发"
        },
        "性格特征": {
            "professionalism": 0.65,
            "authoritativeness": 0.6,
            "friendliness": 0.9,
            "humor": 0.7
        },
        "教学风格": {
            "tone": "活泼生动",
            "pacing": "稍快",
            "interactivity": "高",
            "example_usage": "技能实操演示"
        },
        "适用课程": ["技能培训", "青少年教育", "兴趣课程"]
    },
    "T004_资深教授": {
        "定位": "专业课程、深度研讨",
        "外貌特征": {
            "age_appearance": "老年男性",
            "appearance_style": "学术风格正装",
            "expression": "沉稳睿智",
            "hair_style": "白发苍苍"
        },
        "性格特征": {
            "professionalism": 0.95,
            "authoritativeness": 0.95,
            "friendliness": 0.5,
            "humor": 0.4
        },
        "教学风格": {
            "tone": "权威深入",
            "pacing": "稍慢",
            "interactivity": "较低",
            "example_usage": "深度理论讲解"
        },
        "适用课程": ["研究生课程", "专业深化", "学术前沿"]
    },
    "T005_温馨导师": {
        "定位": "亲子教育、心理咨询",
        "外貌特征": {
            "age_appearance": "中年女性",
            "appearance_style": "温和休闲",
            "expression": "温暖关怀",
            "hair_style": "柔和长发或盘发"
        },
        "性格特征": {
            "professionalism": 0.7,
            "authoritativeness": 0.65,
            "friendliness": 0.95,
            "humor": 0.55
        },
        "教学风格": {
            "tone": "温暖鼓励",
            "pacing": "温和",
            "interactivity": "高",
            "example_usage": "家庭教育指导"
        },
        "适用课程": ["亲子教育", "心理咨询", "健康养生"]
    }
}
```

### 2. 配置参数体系

**外貌参数配置：**

```python
外貌参数 = {
    "性别": {
        "类型": "enum",
        "选项": ["male", "female", "neutral"],
        "默认值": "根据课程定位自动选择"
    },
    "年龄外观": {
        "类型": "enum",
        "选项": ["young_20s", "young_30s", "middle_40s", "senior_60s"],
        "默认值": "根据受众和课程深度选择"
    },
    "着装风格": {
        "类型": "enum",
        "选项": ["formal", "smart_casual", "casual", "academic"],
        "默认值": "根据教学场景选择"
    },
    "面部特征": {
        "类型": "object",
        "属性": {
            "glasses": "bool",
            "hairstyle": "string",
            "expression": "enum[neutral, friendly, serious]"
        }
    },
    "肤色": {
        "类型": "enum",
        "选项": ["light", "medium", "tan", "dark"],
        "说明": "支持多样化Representation"
    }
}
```

**教学参数配置：**

```python
教学参数 = {
    "教学风格": {
        "类型": "enum",
        "选项": ["friendly", "formal", "enthusiastic"],
        "说明": "影响语音语调和表达方式"
    },
    "活力水平": {
        "类型": "enum",
        "选项": ["low", "moderate", "high"],
        "说明": "影响肢体动作频率和幅度"
    },
    "手势频率": {
        "类型": "enum",
        "选项": ["low", "normal", "high"],
        "说明": "影响虚拟教师的手势使用"
    },
    "表情强度": {
        "类型": "enum",
        "选项": ["slight", "medium", "obvious"],
        "说明": "影响表情变化的明显程度"
    },
    "目光接触": {
        "类型": "bool",
        "说明": "是否模拟与学习者的目光接触"
    }
}
```

**语音参数配置：**

```python
语音参数 = {
    "语言": {
        "类型": "enum",
        "选项": ["zh-CN", "en-US", "mixed"],
        "默认值": "zh-CN"
    },
    "音色类型": {
        "类型": "enum",
        "选项": ["male_deep", "male_clear", "male_warm", 
                  "female_soft", "female_professional", "female_vivid"],
        "默认值": "根据性别和风格自动选择"
    },
    "语速": {
        "类型": "float",
        "范围": [0.5, 2.0],
        "默认值": 1.0,
        "说明": "1.0为正常语速"
    },
    "音调": {
        "类型": "float",
        "范围": [-2.0, 2.0],
        "默认值": 0,
        "说明": "正值提升音调，负值降低"
    },
    "音量": {
        "类型": "float",
        "范围": [-12, 12],
        "默认值": 0,
        "说明": "单位为分贝"
    }
}
```

### 3. 智能匹配算法

**匹配维度权重：**

| 匹配维度 | 权重 | 说明 |
|---------|------|------|
| 课程领域匹配 | 0.30 | 虚拟教师风格与课程领域的契合度 |
| 受众适切性 | 0.25 | 虚拟教师形象对目标受众的吸引力 |
| 教学风格协调 | 0.25 | 虚拟教师与课程教学风格的匹配度 |
| 场景适配度 | 0.20 | 虚拟教师与学习场景的协调性 |

**自动匹配规则：**

```python
匹配规则库 = {
    "课程领域规则": {
        "心理学/教育学": ["T002_知性女性", "T005_温馨导师"],
        "理工科/技术": ["T001_学者型男", "T003_活力讲师"],
        "商业/管理": ["T001_学者型男", "T002_知性女性"],
        "艺术/创意": ["T003_活力讲师"],
        "医学/健康": ["T002_知性女性", "T004_资深教授"]
    },
    "受众规则": {
        "婴幼儿家长": "T005_温馨导师",
        "儿童(3-12)": "T003_活力讲师",
        "青少年(12-18)": "T003_活力讲师",
        "大学生": "T002_知性女性",
        "职场人士": "T002_知性女性",
        "中老年": "T004_资深教授"
    },
    "风格规则": {
        "案例丰富": "T005_温馨导师",
        "理论深入": "T004_资深教授",
        "实操导向": "T003_活力讲师",
        "学术严谨": "T001_学者型男"
    }
}
```

### 4. 配置自定义能力

**自定义选项：**

```python
自定义能力 = {
    "形象定制": {
        "上传参考图": "支持上传参考图片生成类似形象",
        "特征描述": "通过文字描述自定义外貌特征",
        "微调参数": "在模板基础上微调具体参数"
    },
    "服装定制": {
        "服装风格": "正式/休闲/学术/商务",
        "服装颜色": "从配色方案中选择或自定义",
        "配饰选择": "眼镜/领带/胸针等"
    },
    "动画定制": {
        "动作库选择": "选择不同的动作风格库",
        "手势类型": "选择强调型/解释型/互动型",
        "微表情设置": "调整眨眼频率和微表情"
    }
}
```

## 输入规范

### 接受输入格式

**完整配置请求：**
```json
{
  "course_info": {
    "course_name": "儿童发展心理学",
    "target_domain": "教育学-心理学类",
    "teaching_style": {
      "tone": "温暖亲和",
      "case_ratio": 0.7,
      "interactivity": "high"
    },
    "difficulty_level": "入门级"
  },
  "learner_profile": {
    "primary_audience": "婴幼儿/儿童家长",
    "age_range": "25-40",
    "preferences": ["温暖", "亲和", "实用"]
  },
  "configuration_options": {
    "template_source": "auto_recommend",
    "customization_level": "template_adjustment",
    "priority": "audience_fit"
  }
}
```

**简化请求：**
```json
{
  "course_theme": "心理学",
  "audience": "家长",
  "style": "亲和"
}
```

## 输出规范

### 配置结果格式

```json
{
  "virtual_teacher_config": {
    "config_id": "VTC_DEV_PSY_001",
    "generation_timestamp": "2024-01-15T10:00:00Z",
    "source": "template",
    "template_id": "T005_温馨导师",
    "template_name": "温馨导师",
    "match_score": 0.92
  },
  "visual_parameters": {
    "gender": "female",
    "age_appearance": "middle_40s",
    "clothing_style": "warm_casual",
    "expression": "warm_friendly",
    "hair_style": "soft_long_hair",
    "accessories": [],
    "skin_tone": "medium"
  },
  "teaching_parameters": {
    "teaching_style": "friendly",
    "energy_level": "moderate",
    "gesture_frequency": "normal",
    "expression_intensity": "medium",
    "eye_contact": true
  },
  "voice_parameters": {
    "language": "zh-CN",
    "voice_type": "female_warm",
    "speed": 1.0,
    "pitch": 0.2,
    "volume": 0,
    "style": "温暖鼓励"
  },
  "customization_notes": [
    "建议使用柔和的暖色调背景",
    "案例讲述时表情更加丰富",
    "手势多使用开放性姿势"
  ],
  "model_files": {
    "base_model": "models/T005_base.glb",
    "texture_package": "models/T005_warm_textures.zip",
    "animation_library": "models/T005_gestures.pkg"
  }
}
```

### 批量配置输出

```json
{
  "batch_config": {
    "course_id": "DEV_PSY_001",
    "total_lessons": 24,
    "configurations": [
      {
        "module": "模块1-3",
        "recommended_template": "T005_温馨导师",
        "rationale": "面向家长的基础概念讲解"
      },
      {
        "module": "模块4-6",
        "recommended_template": "T002_知性女性",
        "rationale": "需要更专业视角的进阶内容"
      }
    ],
    "consistency_setting": {
      "use_same_teacher": true,
      "template_id": "T005_温馨导师",
      "reason": "保持课程风格统一"
    }
  }
}
```

## 配置决策流程

### 1. 配置流程

```
输入课程信息和受众画像
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤1: 模板初步筛选                                          │
│ - 基于课程领域筛选候选模板                                    │
│ - 基于受众类型筛选候选模板                                    │
│ - 取交集得到候选集                                            │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤2: 多维度评分                                            │
│ - 计算课程领域匹配度                                          │
│ - 计算受众适切性                                              │
│ - 计算教学风格协调度                                          │
│ - 计算场景适配度                                              │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤3: 推荐与确认                                            │
│ - 选择得分最高的模板                                          │
│ - 生成参数配置建议                                            │
│ - 如需自定义，生成调整选项                                    │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤4: 批量课程配置                                          │
│ - 分析不同模块的需求差异                                      │
│ - 决定统一配置或分模块配置                                    │
│ - 生成一致性建议                                              │
└─────────────────────────────────────────────────────────────┘
```

### 2. 模板对比视图

```python
对比视图格式 = {
    "template_a": "T005_温馨导师",
    "template_b": "T002_知性女性",
    "evaluation": {
        "课程领域匹配": {"A": 0.95, "B": 0.85},
        "受众吸引力": {"A": 0.98, "B": 0.88},
        "教学风格协调": {"A": 0.92, "B": 0.80},
        "综合得分": {"A": 0.94, "B": 0.84}
    },
    "visual_difference": {
        "A": "更温暖、更具亲和力，适合家庭教育场景",
        "B": "更专业、知性，适合职业教育场景"
    }
}
```

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|------|
| course-requirement-validator | 课程定位、教学风格 | JSON |
| learner-profile-analyzer | 受众特征、风格偏好 | JSON |
| knowledge-graph-builder | 模块教学风格需求 | JSON |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| microcourse-video-generator | 虚拟教师配置参数 | JSON |
| digital-human-generator | 形象定制需求（如需自定义） | JSON |
| lesson-script-generator | 教师风格指导 | JSON |

## 最佳实践

### 1. 模板选择策略

```python
选择策略 = {
    "统一性优先": {
        "适用": "系列微课（多节课）",
        "建议": "全程使用同一虚拟教师",
        "理由": "保持品牌形象一致，增强学习者熟悉感"
    },
    "灵活性优先": {
        "适用": "不同模块差异大",
        "建议": "不同模块可使用不同虚拟教师",
        "条件": "风格过渡自然，有明确角色说明"
    },
    "受众适配优先": {
        "适用": "受众群体复杂",
        "建议": "优先选择受众接受度最高的形象",
        "理由": "提升学习者亲和感和信任度"
    }
}
```

### 2. 参数调优建议

**常见场景的参数调整：**

| 场景 | 参数调整 | 说明 |
|-----|---------|------|
| 面向家长 | 活力水平+1档, 表情强度+1档 | 增强亲和感 |
| 面向儿童 | 手势频率+2档, 表情强度+2档 | 增强吸引力 |
| 学术课程 | 活力水平-1档, 表情强度-1档 | 保持严谨 |
| 实操演示 | 手势频率+2档, 互动性+高 | 引导注意力 |

### 3. 形象一致性维护

```python
一致性维护 = {
    "课程内一致": {
        "规则": "同一课程使用同一虚拟教师",
        "例外": "不同角色（如专家访谈）可引入客串形象"
    },
    "系列课程一致": {
        "规则": "同一讲师的系列课程使用同一形象",
        "升级策略": "可微调参数（如服装变化）避免单调"
    },
    "品牌一致性": {
        "规则": "虚拟教师风格与品牌调性一致",
        "检查项": ["配色协调", "语气一致", "专业度匹配"]
    }
}
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 无合适模板 | 课程领域特殊 | 基于相近模板深度定制 |
| 形象与内容风格冲突 | 匹配规则不当 | 调整教学参数或更换模板 |
| 批量配置不统一 | 缺乏整体规划 | 制定统一的配置策略 |
| 自定义形象效果差 | 技术限制 | 使用成熟的预设模板 |

## 技术实现框架

```python
class VirtualTeacherConfigurator:
    def __init__(self):
        self.template_library = TemplateLibrary()
        self.similarity_engine = SimilarityEngine()
        self.parameter_adjuster = ParameterAdjuster()
        self.consistency_checker = ConsistencyChecker()
    
    def configure(
        self,
        course_info: dict,
        learner_profile: dict,
        options: dict = None
    ) -> VirtualTeacherConfig:
        # 1. 初步筛选
        candidates = self.template_library.filter(
            domain=course_info.target_domain,
            audience=learner_profile.demographic
        )
        
        # 2. 多维度评分
        scored_templates = []
        for template in candidates:
            score = self.similarity_engine.calculate(
                template, course_info, learner_profile
            )
            scored_templates.append((template, score))
        
        # 3. 选择最优模板
        best_template = max(scored_templates, key=lambda x: x[1])[0]
        
        # 4. 参数适配
        config = self.parameter_adjuster.adjust(
            template=best_template,
            course_info=course_info,
            learner_profile=learner_profile
        )
        
        # 5. 一致性检查
        consistency = self.consistency_checker.check(
            config, options.get('course_series')
        )
        
        return VirtualTeacherConfig(
            template=best_template,
            parameters=config,
            consistency=consistency
        )
```

---

本技能通过标准化的虚拟教师形象库和参数化配置系统，为每个微课选择最合适的虚拟教师，确保教学效果和视觉呈现的专业性和亲和力。
