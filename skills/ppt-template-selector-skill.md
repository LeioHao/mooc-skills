---
name: "ppt-template-selector"
description: "根据课程主题、教学风格和目标受众自动推荐和适配PPT模板，包括配色方案、字体选择和布局建议。当需要为微课选择视觉模板时使用此技能。"
---

# PPT Template Selector - PPT模板选择器

## 概述

本技能是微课视频生产线的视觉设计支撑技能，负责根据课程主题、教学风格和目标受众自动推荐最合适的PPT模板。通过多维度匹配算法和模板参数化配置，确保微课的视觉呈现既符合教学场景需求，又保持专业美观。

## 核心功能

### 1. 模板分类体系

**模板维度分类：**

| 分类维度 | 类型 | 适用场景 | 视觉特征 |
|---------|------|---------|---------|
| **学科领域** | 理工类 | 自然科学、工程技术 | 图表丰富、数据驱动 |
| | 文史类 | 人文社科、语言文学 | 典雅留白、文字优美 |
| | 艺术类 | 音乐美术、设计创意 | 色彩大胆、创意布局 |
| | 经管类 | 商业管理、经济金融 | 专业商务、数据呈现 |
| **教学风格** | 正式学术 | 大学课程、专业培训 | 结构严谨、层次分明 |
| | 活泼生动 | 兴趣课程、儿童教育 | 色彩明快、图形丰富 |
| | 温馨亲和 | 亲子教育、心理辅导 | 暖色调、圆角设计 |
| | 简洁专业 | 职业教育、技能培训 | 扁平设计、重点突出 |
| **受众年龄** | 儿童(3-12) | 启蒙教育 | 大字体、多图形、鲜艳色 |
| | 青少年(12-18) | 中学课程 | 活力设计、适度留白 |
| | 成人(18+) | 高等教育、职业培训 | 专业设计、信息密度适中 |
| | 中老年(45+) | 老年教育、健康养生 | 大字体、高对比、简洁 |
| **学习场景** | 课堂教学 | 教师演示用 | 交互元素、进度提示 |
| | 自主学习 | 在线课程 | 结构清晰、可下载 |
| | 移动学习 | 手机端观看 | 竖版布局、要点精简 |

### 2. 模板属性定义

**模板元数据结构：**

```python
模板结构 = {
    "template_id": "TPL_EDU_02",
    "template_name": "温馨教育风",
    "category": {
        "domain": "教育类",
        "style": "温馨亲和",
        "audience": "成人家长",
        "scenario": "家庭教育"
    },
    "visual_design": {
        "color_palette": {
            "primary": "#4A90D9",
            "secondary": "#F5A623",
            "accent": "#7B68EE",
            "background": "#FFFFFF",
            "text": "#333333",
            "light_bg": "#F8F9FA"
        },
        "typography": {
            "title_font": "思源黑体 Bold",
            "heading_font": "思源黑体 Medium",
            "body_font": "微软雅黑",
            "accent_font": "楷体"
        },
        "layout": {
            "teacher_position": "右下",
            "content_area": "左侧2/3",
            "margin": 40,
            "grid_columns": 12
        }
    },
    "animation_style": {
        "entrance": "柔和淡入",
        "emphasis": "轻微放大",
        "transition": "平滑过渡",
        "intensity": "medium"
    },
    "elements": {
        "icon_style": "扁平线性",
        "image_treatment": "圆角裁切",
        "chart_style": "简约扁平",
        "quote_style": "引号装饰"
    },
    "responsive": {
        "desktop": true,
        "tablet": true,
        "mobile": false
    }
}
```

**预设模板库：**

```python
预设模板库 = {
    "学术严谨型": {
        "template_id": "TPL_ACADEMIC_01",
        "colors": ["#1A365D", "#2B6CB0", "#4299E1"],
        "fonts": ["思源黑体", "Times New Roman"],
        "features": ["结构化标题", "文献引用框", "数据图表区"]
    },
    "温暖亲和型": {
        "template_id": "TPL_WARM_01",
        "colors": ["#E53E3E", "#F6AD55", "#FEB2B2"],
        "fonts": ["思源黑体", "圆体"],
        "features": ["圆角卡片", "温暖图标", "家庭场景图"]
    },
    "清新活力型": {
        "template_id": "TPL_VIVID_01",
        "colors": ["#38A169", "#68D391", "#9AE6B4"],
        "fonts": ["思源黑体", "幼圆"],
        "features": ["渐变背景", "趣味图形", "互动元素"]
    },
    "商务专业型": {
        "template_id": "TPL_BUSINESS_01",
        "colors": ["#2D3748", "#4A5568", "#718096"],
        "fonts": ["思源黑体", "Arial"],
        "features": ["数据仪表盘", "流程图", "对比表格"]
    }
}
```

### 3. 多维度匹配算法

**匹配维度权重：**

| 匹配维度 | 权重 | 说明 |
|---------|------|------|
| 学科领域匹配 | 0.30 | 模板风格与学科特点的一致性 |
| 教学风格匹配 | 0.25 | 模板风格与教学风格的协调性 |
| 目标受众匹配 | 0.20 | 视觉设计对目标受众的吸引力 |
| 学习场景匹配 | 0.15 | 布局设计对学习场景的适应性 |
| 品牌一致性 | 0.10 | 与已有视觉资源的协调性 |

**匹配评分计算：**

```python
def calculate_template_match(
    template: Template,
    course_requirement: dict,
    learner_profile: dict
) -> float:
    # 学科领域匹配度
    domain_score = domain_similarity(
        template.category.domain,
        course_requirement.target_domain
    )
    
    # 教学风格匹配度
    style_score = style_similarity(
        template.visual_design,
        course_requirement.teaching_style
    )
    
    # 受众匹配度
    audience_score = audience_appeal(
        template.category.audience,
        learner_profile.demographic
    )
    
    # 场景适配度
    scenario_score = scenario_fit(
        template.responsive,
        course_requirement.application_scenario
    )
    
    # 品牌一致性（如果有）
    brand_score = brand_consistency(
        template,
        course_requirement.brand
    ) if course_requirement.brand else 1.0
    
    # 加权总分
    total_score = (
        domain_score * 0.30 +
        style_score * 0.25 +
        audience_score * 0.20 +
        scenario_score * 0.15 +
        brand_score * 0.10
    )
    
    return total_score
```

### 4. 智能适配功能

**模板参数化调整：**

```python
模板适配规则 = {
    "配色调整": {
        "深色主题": {
            "条件": "学习者偏好深色模式",
            "操作": "反转配色：背景→#1A1A2E, 文字→#FFFFFF"
        },
        "高对比": {
            "条件": "目标受众为中老年",
            "操作": "增强对比度：文字颜色加深，背景加亮"
        }
    },
    "字体调整": {
        "大字体": {
            "条件": "受众为儿童或中老年",
            "操作": "标题+4pt, 正文+2pt"
        },
        "专业字体": {
            "条件": "学术课程",
            "操作": "标题→Times New Roman, 正文→Georgia"
        }
    },
    "布局调整": {
        "简化布局": {
            "条件": "碎片化学习场景",
            "操作": "减少每页信息量，突出单一要点"
        },
        "交互布局": {
            "条件": "课堂互动场景",
            "操作": "增加问答区域、投票区域"
        }
    }
}
```

**风格迁移建议：**

| 原风格 | 目标风格 | 迁移参数 |
|-------|---------|---------|
| 正式学术 | 活泼生动 | +20%色彩饱和度, 圆角+4px, 图形+30% |
| 商务专业 | 温馨亲和 | 暖色+15%, 圆角+2px, 减少数据图表 |
| 简洁扁平 | 丰富层次 | 增加阴影深度, 添加装饰元素 |
| 传统保守 | 现代简约 | 去除边框, 留白+20%, 扁平图标 |

## 输入规范

### 接受输入格式

**完整需求输入：**
```json
{
  "course_requirement": {
    "course_name": "儿童发展心理学",
    "target_domain": "教育学-心理学类",
    "teaching_style": {
      "case_based": true,
      "case_ratio": 0.7,
      "tone": "温馨亲和"
    },
    "difficulty_level": "入门级"
  },
  "learner_profile": {
    "demographic": {
      "age_range": "25-40",
      "family_status": "有适龄子女"
    },
    "content_preference": {
      "visual_style": "温馨"
    }
  },
  "application_scenario": "家庭教育",
  "options": {
    "include_alternatives": true,
    "max_recommendations": 3,
    "custom_branding": {
      "logo_url": "https://...",
      "primary_color": "#4A90D9"
    }
  }
}
```

**简化输入：**
```json
{
  "course_theme": "心理学",
  "audience": "家长",
  "style_preference": "亲和"
}
```

## 输出规范

### 推荐结果格式

```json
{
  "template_recommendation": {
    "primary_recommendation": {
      "template_id": "TPL_WARM_01",
      "template_name": "温馨教育风",
      "match_score": 0.92,
      "match_reasons": [
        "教育心理学学科高度匹配",
        "温馨亲和风格适合家长群体",
        "配色方案契合家庭教育场景"
      ],
      "visual_preview": "template_preview_url",
      "price": "free"
    },
    "alternatives": [
      {
        "template_id": "TPL_EDU_02",
        "template_name": "清新教育风",
        "match_score": 0.85,
        "differences": "色彩更清新，适合年轻家长"
      }
    ]
  },
  "template_config": {
    "color_palette": {
      "primary": "#E53E3E",
      "secondary": "#F6AD55",
      "accent": "#7B68EE",
      "background": "#FFF5F5",
      "text": "#2D3748"
    },
    "typography": {
      "title_size": 36,
      "heading_size": 28,
      "body_size": 20,
      "title_font": "思源黑体 Bold",
      "body_font": "微软雅黑"
    },
    "layout": {
      "teacher_position": "右下",
      "content_width": "66%",
      "margins": 40
    },
    "animation": {
      "intensity": "medium",
      "entrance_effect": "淡入上浮",
      "transition_effect": "平滑"
    }
  },
  "customization_suggestions": [
    {
      "element": "标题页背景",
      "suggestion": "添加柔和渐变效果",
      "implementation": "使用#FFF5F5到#FFE4E1的线性渐变"
    },
    {
      "element": "案例展示区",
      "suggestion": "使用圆角卡片设计",
      "implementation": "border-radius: 12px, 添加浅色阴影"
    }
  ],
  "export_settings": {
    "format": "PPTX",
    "resolution": "1920x1080",
    "include_guidelines": true
  }
}
```

## 匹配决策流程

### 1. 模板选择流程

```
输入课程需求和学习者画像
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤1: 初步筛选                                              │
│ - 过滤学科领域不匹配的模板                                    │
│ - 过滤受众年龄不适配的模板                                    │
│ - 过滤技术规格不支持的模板                                    │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤2: 多维度评分                                            │
│ - 计算学科匹配度                                              │
│ - 计算风格匹配度                                              │
│ - 计算受众匹配度                                              │
│ - 计算场景适配度                                              │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤3: 排序与推荐                                            │
│ - 按总分排序                                                  │
│ - 选择Top推荐                                                │
│ - 提供备选方案                                                │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤4: 参数适配                                              │
│ - 调整配色方案                                                │
│ - 调整字体参数                                                │
│ - 调整布局配置                                                │
│ - 生成自定义建议                                              │
└─────────────────────────────────────────────────────────────┘
```

### 2. 模板对比视图

```python
模板对比格式 = {
    "template_a": "TPL_WARM_01",
    "template_b": "TPL_VIVID_01",
    "comparison": {
        "学科匹配": {"A": "★★★★★", "B": "★★★★☆"},
        "受众吸引": {"A": "★★★★☆", "B": "★★★★★"},
        "制作难度": {"A": "★★☆☆☆", "B": "★★★☆☆"},
        "专业度": {"A": "★★★★☆", "B": "★★★☆☆"}
    },
    "recommendation": {
        "教学为主": "TPL_WARM_01",
        "趣味为主": "TPL_VIVID_01"
    }
}
```

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|------|
| course-requirement-validator | 课程需求、教学风格 | JSON |
| learner-profile-analyzer | 受众特征、视觉偏好 | JSON |
| knowledge-graph-builder | 模块主题、难度级别 | JSON |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| lesson-script-generator | 模板参数（用于脚本视觉说明） | JSON |
| microcourse-video-generator | 模板文件+配置参数 | 文件+JSON |
| infographic-generator | 配色方案（统一信息图风格） | JSON |

## 最佳实践

### 1. 模板选择策略

**场景化选择建议：**

```python
场景选择策略 = {
    "新手教师": {
        "推荐策略": "选择结构清晰的模板",
        "模板特征": ["固定位置", "预设动画", "少自定义"],
        "理由": "降低制作难度，确保输出质量"
    },
    "资深教师": {
        "推荐策略": "选择可定制性强的模板",
        "模板特征": ["灵活布局", "丰富元素", "支持自定义"],
        "理由": "充分发挥创作空间"
    },
    "批量生产": {
        "推荐策略": "选择风格统一的模板包",
        "模板特征": ["多页类型", "组件复用", "快速替换"],
        "理由": "保证系列课程风格一致，提高效率"
    }
}
```

### 2. 品牌一致性

```python
品牌模板适配 = {
    "品牌色融入": {
        "方法": "提取品牌主色作为模板强调色",
        "比例": "主色不超过30%，辅色70%"
    },
    "Logo位置": {
        "标准位置": "每页右下角或标题页左上角",
        "尺寸": "不超过页面宽度的15%"
    },
    "字体统一": {
        "要求": "选择品牌字体或系统默认字体",
        "建议": "最多使用2种字体系列"
    }
}
```

### 3. 模板维护

**模板更新策略：**

```python
模板生命周期管理 = {
    "版本控制": {
        "major": "重大视觉改版（1-2年）",
        "minor": "配色/元素优化（季度）",
        "patch": "bug修复和兼容更新（月度）"
    },
    "质量监控": {
        "使用率": "监控各模板使用频率",
        "反馈评分": "收集用户满意度评分",
        "问题报告": "跟踪技术问题和改进需求"
    },
    "淘汰机制": {
        "条件": "使用率<5%且评分<3.5",
        "处理": "移至归档库，可申请恢复使用"
    }
}
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 无合适模板 | 模板库不覆盖该领域 | 通用模板+深度定制，或新建模板 |
| 多模板评分相近 | 区分度不足 | 提供对比视图，让用户选择 |
| 模板风格冲突 | 与品牌不符 | 调整配色和元素，或更换模板 |
| 导出格式问题 | 兼容性问题 | 选择通用格式，推荐PPTX |

## 技术实现框架

```python
class PPTTemplateSelector:
    def __init__(self):
        self.template_index = TemplateIndex()
        self.similarity_engine = SimilarityEngine()
        self.adaptation_engine = AdaptationEngine()
        self.customization_generator = CustomizationGenerator()
    
    def select(
        self,
        course_requirement: dict,
        learner_profile: dict,
        options: dict = None
    ) -> TemplateRecommendation:
        # 1. 初步筛选
        candidates = self.template_index.filter(
            domain=course_requirement.target_domain,
            audience=learner_profile.demographic.age_range
        )
        
        # 2. 多维度评分
        scored_templates = []
        for template in candidates:
            score = self.similarity_engine.calculate(
                template, course_requirement, learner_profile
            )
            scored_templates.append((template, score))
        
        # 3. 排序推荐
        ranked_templates = sorted(
            scored_templates,
            key=lambda x: x[1],
            reverse=True
        )
        recommendations = ranked_templates[:options.max_recommendations]
        
        # 4. 参数适配
        adapted_configs = []
        for template, score in recommendations:
            config = self.adaptation_engine.adapt(
                template, course_requirement, learner_profile
            )
            adapted_configs.append({
                "template": template,
                "score": score,
                "config": config
            })
        
        return TemplateRecommendation(
            primary=adapted_configs[0],
            alternatives=adapted_configs[1:]
        )
```

---

本技能通过智能化的模板选择和参数适配，确保每个微课都能获得专业、美观且适切的视觉呈现，大幅提升微课的制作效率和视觉质量。
