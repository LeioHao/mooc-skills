---
name: "course-creator"
description: "微课视频生产线的编排引擎，协调所有子技能完成从需求分析到视频输出的全流程自动化生产。当用户需要创建完整的微课课程或系列视频时使用此技能。"
---

# Course Creator - 微课视频生产线编排技能

## 概述

本技能是微课视频生产线的核心编排引擎，负责协调自然语言解析、学习者分析、知识图谱构建、脚本生成、视觉设计、视频合成和质量检验等全流程技能，实现微课视频的端到端自动化生产。通过模块化的技能编排和质量门禁控制，确保高效、高质量地完成系列微课视频的生产任务。

## 核心架构

### 技能编排层次

```
┌─────────────────────────────────────────────────────────────┐
│                  生产线编排层（Course Creator）                │
│              协调各子技能执行、管理数据流转、控制质量门禁         │
└─────────────────────────────────────────────────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   需求解析域     │ │   内容构建域     │ │   视觉设计域     │
│ • 自然语言解析   │ │ • 知识图谱构建   │ │ • PPT模板选择   │
│ • 需求校验      │ │ • 脚本生成      │ │ • 信息图生成    │
│ • 学习者分析    │ │ • 案例匹配      │ │ • 虚拟教师配置  │
└─────────────────┘ └─────────────────┘ └─────────────────┘
          │                   │                   │
          └───────────────────┼───────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      视频合成与质量保障域                       │
│    • PPT动画同步  • 口型表情生成  • 视频合成  • 质量检验      │
└─────────────────────────────────────────────────────────────┘
```

### 技能协作矩阵

| 上游技能 | 下游技能 | 数据流转 |
|---------|---------|---------|
| natural-language-course-parser | course-requirement-validator | 原始需求→结构化需求 |
| course-requirement-validator | learner-profile-analyzer | 校验后需求→画像输入 |
| learner-profile-analyzer | knowledge-graph-builder | 学习者偏好→内容适配 |
| knowledge-graph-builder | case-library-matcher | 知识点→案例匹配 |
| case-library-matcher | lesson-script-generator | 案例→脚本内容 |
| lesson-script-generator | ppt-template-selector | 脚本→模板参数 |
| ppt-template-selector | infographic-generator | 模板→信息图设计 |
| infographic-generator | microcourse-video-generator | 信息图→视频素材 |
| microcourse-video-generator | content-quality-checker | 视频→质量检验 |
| content-quality-checker | mooc-standard-validator | 质量报告→合规认证 |

## 核心功能

### 1. 自然语言需求解析

**解析能力：**
- 识别课程主题、学科领域
- 提取目标受众、学习场景
- 判断时长偏好、教学风格
- 理解特殊需求和约束条件

**解析示例：**
```
用户输入: "我想做一门关于发展心理学的微课，面向年轻父母，
         帮助他们理解孩子成长规律，每节课8分钟左右，
         希望能用生活化的案例讲解。"

解析结果:
  course_name: "发展心理学"
  target_domain: "教育学-心理学类"
  learner_profile: {
    primary: "年轻父母",
    focus: "儿童教育"
  }
  duration_preference: {
    optimal: 480,
    range: [420, 540]
  }
  teaching_style: {
    case_based: true,
    case_ratio: 0.7
  }
```

### 2. 学习者画像构建

**分析维度：**
- 人口统计特征（年龄、学历、职业）
- 学习动机类型（问题解决、职业发展、兴趣探索）
- 学习约束条件（时间、设备、注意力）
- 知识基线评估（前置知识、误解风险）
- 内容偏好建模（深度、案例、节奏）

**画像输出示例：**
```json
{
  "profile_id": "LP_DEV_PSY_001",
  "learner_type": "积极成长型家长",
  "content_preference": {
    "conceptual_depth": "浅",
    "case_ratio": 0.7,
    "interactivity": "中",
    "tone": "温暖鼓励"
  },
  "pain_points": [
    "孩子情绪不稳定时不知如何应对",
    "不了解孩子各阶段发展特点"
  ]
}
```

### 3. 知识体系构建

**构建流程：**
```
课程大纲 → 知识点提取 → 关系抽取 → 图谱构建 → 模块划分 → 学习路径规划
```

**输出知识图谱结构：**
```json
{
  "knowledge_graph": {
    "total_nodes": 48,
    "total_modules": 6,
    "total_lessons": 24,
    "modules": [
      {
        "module_id": "M_01",
        "module_name": "发展心理学概述",
        "lessons": ["L_01_01", "L_01_02", "L_01_03"]
      }
    ],
    "learning_paths": ["标准路径", "快速路径", "深度路径"]
  }
}
```

### 4. 脚本批量生成

**生成策略：**
- 并行生成多节微课脚本
- 智能分配案例和素材
- 自动控制时长节奏
- 保持风格一致性

**脚本结构：**
```json
{
  "script": {
    "lesson_id": "L_01_01",
    "title": "什么是发展心理学",
    "duration_target": 480,
    "structure": {
      "opening": {"duration": 45, "content": "开场白+学习目标"},
      "body": {"duration": 390, "segments": ["概念讲解", "案例分析", "重点强调"]},
      "summary": {"duration": 45, "content": "回顾+思考题"}
    },
    "visual_guide": "PPT页面和动画说明",
    "voice_cue": "语调、停顿、重音提示"
  }
}
```

### 5. 视觉设计编排

**设计流程：**
```
模板选择 → 配色适配 → 布局设计 → 信息图生成 → 动画效果设计
```

**设计参数：**
```json
{
  "visual_design": {
    "template": "TPL_WARM_01",
    "color_palette": {
      "primary": "#E53E3E",
      "secondary": "#F6AD55"
    },
    "typography": {
      "title_size": 36,
      "body_size": 20
    },
    "infographics": [
      {"type": "concept_map", "position": "slide_3"},
      {"type": "timeline", "position": "slide_5"}
    ]
  }
}
```

### 6. 视频合成编排

**合成流程：**
```
PPT + 脚本 + 音频 + 虚拟教师配置 → 口型同步 → 表情动画 → 视频渲染 → 输出
```

**合成配置：**
```json
{
  "synthesis_config": {
    "video_output": {
      "resolution": "1920x1080",
      "frame_rate": 30,
      "codec": "H.264"
    },
    "teacher_config": {
      "template_id": "T005_温馨导师",
      "position": "bottom_right"
    },
    "audio_config": {
      "voice_type": "female_warm",
      "speed": 1.0
    }
  }
}
```

### 7. 质量控制流水线

**检验节点：**
```
需求校验门禁 → 内容质量门禁 → 技术质量门禁 → MOOC标准门禁
```

**门禁决策：**
```python
门禁状态 = {
    "PASSED": "进入下一阶段",
    "NEEDS_REVISION": "返回修订",
    "BLOCKED": "阻塞，需要人工介入"
}
```

## 输入规范

### 自然语言输入

```json
{
  "input_type": "natural_language",
  "user_input": "我想制作一门关于发展心理学的系列微课，
                主要帮助年轻父母理解孩子从出生到青春期的心理发展规律。
                每节课8分钟左右，希望能用通俗易懂的案例来讲解。
                课程总共有20节左右，覆盖婴儿期、幼儿期、儿童期和青春期四个阶段。"
}
```

### 结构化输入

```json
{
  "input_type": "structured",
  "course_info": {
    "course_name": "发展心理学",
    "target_domain": "教育学-心理学类",
    "difficulty_level": "入门级",
    "total_lessons": 20,
    "modules": 4
  },
  "learner_info": {
    "primary_audience": "年轻父母",
    "knowledge_baseline": "无心理学背景",
    "learning_goal": "理解孩子发展规律，指导家庭教育"
  },
  "production_options": {
    "lesson_duration": 480,
    "style": "案例丰富、通俗易懂",
    "output_format": "MP4"
  }
}
```

### 混合输入

```json
{
  "input_type": "mixed",
  "course_name": "发展心理学",
  "user_requirements": "主要面向年轻父母，每节课8分钟，案例丰富一些",
  "provided_materials": {
    "outline": "/path/to/outline.pdf",
    "reference_materials": ["/path/to/ref1.pdf"]
  }
}
```

## 输出规范

### 完整课程生产包

```json
{
  "production_package": {
    "package_id": "PKG_DEV_PSY_001",
    "course_name": "发展心理学",
    "generation_timestamp": "2024-01-15T10:00:00Z",
    "total_lessons": 24,
    "total_duration_minutes": 192,
    "status": "completed"
  },
  "course_structure": {
    "modules": [
      {
        "module_id": "M_01",
        "title": "发展心理学概述",
        "lessons": 3,
        "duration_minutes": 24
      }
    ]
  },
  "production_artifacts": {
    "knowledge_graph": "kg_dev_psy_001.json",
    "learner_profile": "profile_dev_psy_001.json",
    "scripts": [
      {"lesson_id": "L_01_01", "file": "scripts/L_01_01.md"},
      {"lesson_id": "L_01_02", "file": "scripts/L_01_02.md"}
    ],
    "ppt_packages": [
      {"lesson_id": "L_01_01", "file": "ppt/L_01_01.pptx"}
    ],
    "videos": [
      {"lesson_id": "L_01_01", "file": "videos/L_01_01.mp4"}
    ]
  },
  "quality_reports": {
    "content_quality": "reports/content_quality.pdf",
    "technical_quality": "reports/technical_quality.pdf",
    "mooc_certification": "reports/certification.pdf"
  }
}
```

### 生产进度报告

```json
{
  "production_progress": {
    "course_id": "DEV_PSY_001",
    "overall_progress": 0.75,
    "stages": {
      "requirement_parsing": {"status": "completed", "progress": 1.0},
      "learner_analysis": {"status": "completed", "progress": 1.0},
      "knowledge_construction": {"status": "completed", "progress": 1.0},
      "script_generation": {"status": "in_progress", "progress": 0.8, "completed": 19, "total": 24},
      "visual_design": {"status": "pending", "progress": 0.0},
      "video_synthesis": {"status": "pending", "progress": 0.0},
      "quality_assurance": {"status": "pending", "progress": 0.0}
    },
    "estimated_completion": "2024-01-15T18:00:00Z"
  }
}
```

## 生产流程详解

### 阶段一：需求解析与学习者分析

```
用户自然语言输入
    ↓
┌─────────────────────────────────────────────────────────────┐
│ natural-language-course-parser                              │
│ - 解析课程主题、目标受众、时长偏好、教学风格                  │
│ - 输出结构化课程需求                                         │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ course-requirement-validator                               │
│ - 校验需求完整性、一致性、可行性                              │
│ - 识别潜在风险，提供改进建议                                  │
│ - 门禁决策：PASSED → 进入下一阶段                            │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ learner-profile-analyzer                                    │
│ - 构建学习者多维画像                                          │
│ - 分析学习动机、约束条件、内容偏好                             │
│ - 输出画像数据供后续技能使用                                  │
└─────────────────────────────────────────────────────────────┘
```

### 阶段二：知识体系构建

```
课程大纲/知识点输入
    ↓
┌─────────────────────────────────────────────────────────────┐
│ knowledge-graph-builder                                     │
│ - 提取知识点，构建节点                                        │
│ - 识别关系，生成边                                            │
│ - 评估难度，规划学习路径                                       │
│ - 划分模块，规划微课单元                                       │
└─────────────────────────────────────────────────────────────┘
    ↓
输出：完整知识图谱，包含所有模块和微课的知识点结构
```

### 阶段三：脚本批量生成

```
知识图谱 + 学习者画像 + 案例库
    ↓
┌─────────────────────────────────────────────────────────────┐
│ case-library-matcher                                        │
│ - 为每个知识点匹配合适案例                                     │
│ - 考虑受众适切性、情感共鸣度                                   │
│ - 输出案例推荐列表                                            │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ lesson-script-generator                                     │
│ - 并行生成多节微课脚本                                        │
│ - 自动分配案例，控制时长节奏                                   │
│ - 生成视觉说明和音频提示                                       │
│ - 输出完整脚本包                                              │
└─────────────────────────────────────────────────────────────┘
```

### 阶段四：视觉设计编排

```
脚本包 + 学习者画像 + 模板库
    ↓
┌─────────────────────────────────────────────────────────────┐
│ ppt-template-selector                                        │
│ - 根据课程定位选择PPT模板                                      │
│ - 适配配色、字体、布局参数                                     │
│ - 输出模板配置                                                │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ infographic-generator                                        │
│ - 根据知识点类型生成信息图                                     │
│ - 适配视觉风格和复杂度                                        │
│ - 输出信息图SVG文件                                           │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ virtual-teacher-configurator                                 │
│ - 根据教学风格选择虚拟教师模板                                  │
│ - 配置外貌、性格、教学参数                                      │
│ - 输出虚拟教师配置                                            │
└─────────────────────────────────────────────────────────────┘
```

### 阶段五：视频合成

```
PPT设计 + 脚本 + 虚拟教师配置 + 音频
    ↓
┌─────────────────────────────────────────────────────────────┐
│ ppt-animation-synchronizer                                   │
│ - 同步PPT动画与脚本时间轴                                      │
│ - 配置入场动画、强调动画、转场效果                              │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ microcourse-video-generator                                   │
│ - 虚拟教师口型同步                                             │
│ - 表情和手势动画生成                                           │
│ - 视频渲染输出                                                 │
│ - 输出MP4视频文件                                              │
└─────────────────────────────────────────────────────────────┘
```

### 阶段六：质量检验与认证

```
生成的视频
    ↓
┌─────────────────────────────────────────────────────────────┐
│ content-quality-checker                                      │
│ - 检验知识点准确性、逻辑连贯性、语言规范性                      │
│ - 评估案例适切性、时长合规性                                    │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ technical-quality-checker                                    │
│ - 检验视频质量、音频质量、音画同步                              │
│ - 评估动画流畅度、字幕准确性                                    │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ mooc-standard-validator                                      │
│ - 综合验证MOOC标准合规性                                       │
│ - 颁发认证等级和证书                                           │
│ - 门禁决策：PASSED → 发布                                     │
└─────────────────────────────────────────────────────────────┘
```

## 最佳实践

### 1. 生产模式选择

```python
生产模式 = {
    "快速生产": {
        "适用": "时间紧迫，质量要求适中",
        "策略": "并行处理，优化冗余环节",
        "预计时间": "1小时/节"
    },
    "标准生产": {
        "适用": "常规课程生产",
        "策略": "标准流程，全质量检验",
        "预计时间": "2小时/节"
    },
    "精品生产": {
        "适用": "重要课程或品牌课程",
        "策略": "人工审核环节，多轮优化",
        "预计时间": "4小时/节"
    }
}
```

### 2. 质量优先策略

```python
质量策略 = {
    "高风险场景": {
        "触发条件": "涉及敏感内容、专业争议、复杂理论",
        "策略": "增加人工审核环节",
        "额外检验": ["专家内容审核", "事实核查"]
    },
    "长课程策略": {
        "触发条件": "总课时>20节",
        "策略": "分模块生产，模块级质量把控",
        "优势": "问题早发现、早修复"
    }
}
```

### 3. 资源优化策略

```python
优化策略 = {
    "并行处理": "多个独立环节并行执行",
    "模板复用": "同一课程使用统一模板",
    "素材共享": "案例库、信息图库共享复用",
    "智能调度": "根据技能依赖关系优化执行顺序"
}
```

## 与其他技能协作

### 子技能调用关系

```python
子技能调用 = {
    "需求解析": {
        "调用技能": ["natural-language-course-parser", "course-requirement-validator"],
        "输入": "用户自然语言",
        "输出": "结构化需求"
    },
    "学习者分析": {
        "调用技能": ["learner-profile-analyzer"],
        "输入": "需求信息",
        "输出": "学习者画像"
    },
    "内容构建": {
        "调用技能": ["knowledge-graph-builder", "case-library-matcher"],
        "输入": ["课程大纲", "学习者画像"],
        "输出": "知识图谱+案例列表"
    },
    "脚本生成": {
        "调用技能": ["lesson-script-generator"],
        "输入": ["知识图谱", "案例", "画像"],
        "输出": "脚本包"
    },
    "视觉设计": {
        "调用技能": ["ppt-template-selector", "infographic-generator", "virtual-teacher-configurator"],
        "输入": ["脚本", "模板库"],
        "输出": "设计包"
    },
    "视频合成": {
        "调用技能": ["microcourse-video-generator"],
        "输入": ["设计包", "音频", "虚拟教师"],
        "输出": "视频文件"
    },
    "质量检验": {
        "调用技能": ["content-quality-checker", "technical-quality-checker", "mooc-standard-validator"],
        "输入": "视频内容",
        "输出": "质量报告+认证"
    }
}
```

## 使用示例

### 示例一：快速创建单节微课

```json
{
  "request": {
    "type": "single_lesson",
    "input": "我想做一节关于'第一反抗期'的微课，面向2岁孩子的家长",
    "target_duration": 480
  },
  "execution": {
    "step_1": "解析需求 → course_name=发展心理学, topic=第一反抗期, audience=2岁孩子家长",
    "step_2": "构建知识点 → KN_反抗期定义, KN_典型表现, KN_应对策略",
    "step_3": "匹配案例 → CASE_不要说不、CASE_自己吃饭",
    "step_4": "生成脚本 → 包含定义+案例+策略+总结",
    "step_5": "生成视频 → 输出MP4文件"
  },
  "output": {
    "video": "/output/第一反抗期.mp4",
    "script": "/output/第一反抗期_script.md"
  }
}
```

### 示例二：创建完整系列课程

```json
{
  "request": {
    "type": "full_course",
    "input": "我想创建一门'儿童发展心理学'课程，面向0-6岁儿童家长，共20节",
    "options": {
      "parallel_lessons": 4,
      "quality_tier": "standard"
    }
  },
  "execution": {
    "step_1": "需求解析 → 校验完整性，识别4个模块",
    "step_2": "知识构建 → 构建包含48个知识点的图谱",
    "step_3": "并行生成 → 4节/批，共5批完成20节",
    "step_4": "视觉设计 → 统一模板，分模块微调",
    "step_5": "视频合成 → 并行渲染",
    "step_6": "质量认证 → 全检验，颁发MOOC证书"
  },
  "output": {
    "course_package": "/output/儿童发展心理学_course_package.zip",
    "videos": "/output/videos/",
    "certification": "/output/MOOC_certification.pdf"
  }
}
```

## 故障排除

| 问题类型 | 具体问题 | 解决策略 |
|---------|---------|---------|
| 需求解析失败 | 无法识别课程主题 | 引导用户提供更具体的描述 |
| 知识构建困难 | 课程大纲不完整 | 补充输入或使用默认结构 |
| 脚本生成超时 | 内容过于复杂 | 拆分知识点或精简内容 |
| 视频合成失败 | 技术资源不足 | 降低并行度或优化配置 |
| 质量检验不通过 | 多项指标不达标 | 根据报告逐一修复问题 |

## 技术实现框架

```python
class CourseCreator:
    def __init__(self):
        self.nlp_parser = NaturalLanguageCourseParser()
        self.requirement_validator = CourseRequirementValidator()
        self.learner_analyzer = LearnerProfileAnalyzer()
        self.kg_builder = KnowledgeGraphBuilder()
        self.case_matcher = CaseLibraryMatcher()
        self.script_generator = LessonScriptGenerator()
        self.ppt_selector = PPTTemplateSelector()
        self.infographic_gen = InfographicGenerator()
        self.teacher_configurator = VirtualTeacherConfigurator()
        self.video_generator = MicrocourseVideoGenerator()
        self.content_checker = ContentQualityChecker()
        self.technical_checker = TechnicalQualityChecker()
        self.mooc_validator = MOOCStandardValidator()
    
    def create_course(
        self,
        user_input: dict,
        options: dict = None
    ) -> ProductionPackage:
        # 阶段1: 需求解析
        course_req = self.nlp_parser.parse(user_input)
        validation = self.requirement_validator.validate(course_req)
        if validation.status != "PASSED":
            return validation.feedback
        
        # 阶段2: 学习者分析
        learner_profile = self.learner_analyzer.analyze(
            course_req, validation
        )
        
        # 阶段3: 知识构建
        kg = self.kg_builder.build(
            course_req.outline,
            learner_profile
        )
        
        # 阶段4: 脚本生成（可并行）
        scripts = []
        for lesson in kg.lessons:
            case = self.case_matcher.match(lesson, learner_profile)
            script = self.script_generator.generate(lesson, case, learner_profile)
            scripts.append(script)
        
        # 阶段5: 视觉设计（可并行）
        template = self.ppt_selector.select(course_req, learner_profile)
        for script in scripts:
            infographics = self.infographic_gen.generate(script, template)
            teacher_config = self.teacher_configurator.configure(script, learner_profile)
            script.visual_package = {"template": template, "infographics": infographics, "teacher": teacher_config}
        
        # 阶段6: 视频合成
        videos = []
        for script in scripts:
            video = self.video_generator.generate(script)
            videos.append(video)
        
        # 阶段7: 质量检验
        quality_reports = []
        for video in videos:
            content_report = self.content_checker.check(video)
            tech_report = self.technical_checker.check(video)
            mooc_cert = self.mooc_validator.validate(video, content_report, tech_report)
            quality_reports.append({
                "content": content_report,
                "technical": tech_report,
                "certification": mooc_cert
            })
        
        return ProductionPackage(
            course_id=course_req.course_id,
            scripts=scripts,
            videos=videos,
            quality_reports=quality_reports,
            status="completed" if all(r.certification.status == "PASSED" for r in quality_reports) else "needs_revision"
        )
```

---

本技能作为微课视频生产线的核心编排引擎，通过模块化的技能协作和标准化的质量门禁，实现从自然语言需求到MOOC认证视频的端到端自动化生产，大幅提升微课制作效率和质量稳定性。
