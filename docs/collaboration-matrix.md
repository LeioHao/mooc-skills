# 技能协作矩阵

本文档定义了微课视频制作系统中各技能之间的协作关系和数据流转规范。

## 技能依赖关系图

```
用户自然语言输入
    ↓
natural-language-course-parser
    ↓
course-requirement-validator
    ↓
learner-profile-analyzer
    ↓
┌─────────────────────────────────────────────────────────────┐
│  knowledge-graph-builder ← case-library-matcher             │
│         ↓                                                   │
│  lesson-script-generator                                    │
│         ↓                                                   │
│  infographic-generator ← virtual-teacher-configurator       │
│         ↓                                                   │
│  microcourse-video-generator                               │
│         ↓                                                   │
│  content-quality-checker ← technical-quality-checker       │
│         ↓                                                   │
│  mooc-standard-validator                                  │
└─────────────────────────────────────────────────────────────┘
```

## 数据流转规范

### 阶段一：需求解析

| 上游技能 | 下游技能 | 数据格式 | 说明 |
|---------|---------|---------|------|
| 用户输入 | natural-language-course-parser | 文本 | 用户自然语言描述 |
| natural-language-course-parser | course-requirement-validator | JSON | 结构化课程需求 |
| course-requirement-validator | learner-profile-analyzer | JSON | 校验后需求 |

### 阶段二：内容构建

| 上游技能 | 下游技能 | 数据格式 | 说明 |
|---------|---------|---------|------|
| learner-profile-analyzer | knowledge-graph-builder | JSON | 学习者画像 |
| knowledge-graph-builder | case-library-matcher | JSON | 知识图谱 |
| knowledge-graph-builder | lesson-script-generator | JSON | 知识点结构 |
| case-library-matcher | lesson-script-generator | JSON | 案例推荐 |
| lesson-script-generator | ppt-template-selector | JSON | 脚本内容 |

### 阶段三：视觉设计

| 上游技能 | 下游技能 | 数据格式 | 说明 |
|---------|---------|---------|------|
| ppt-template-selector | infographic-generator | JSON | 模板配置 |
| lesson-script-generator | infographic-generator | JSON | 知识点信息 |
| lesson-script-generator | virtual-teacher-configurator | JSON | 教学风格 |
| infographic-generator | microcourse-video-generator | 文件 | SVG信息图 |

### 阶段四：视频合成

| 上游技能 | 下游技能 | 数据格式 | 说明 |
|---------|---------|---------|------|
| microcourse-video-generator | content-quality-checker | 文件 | 视频文件 |
| microcourse-video-generator | technical-quality-checker | 文件 | 视频文件 |
| content-quality-checker | mooc-standard-validator | JSON | 质量报告 |
| technical-quality-checker | mooc-standard-validator | JSON | 技术报告 |

## 输入输出规范

### 标准JSON格式

所有技能之间的数据交换应遵循JSON格式规范。

#### 课程需求格式

```json
{
  "course_name": "课程名称",
  "target_domain": "所属学科领域",
  "difficulty_level": "难度级别",
  "learner_profile": {
    "primary": "主要受众",
    "knowledge_baseline": "知识基线"
  },
  "duration_preference": {
    "min": 300,
    "max": 900,
    "optimal": 480
  },
  "teaching_style": {
    "case_based": true,
    "case_ratio": 0.7
  }
}
```

## 错误处理规范

### 错误代码体系

| 错误代码 | 说明 | 处理方式 |
|---------|------|---------|
| E001 | 输入解析失败 | 返回原始输入，请求澄清 |
| E002 | 校验未通过 | 返回详细错误信息和修改建议 |
| E003 | 资源不存在 | 返回错误，建议提供必要资源 |
| E004 | 超时错误 | 返回部分结果，请求重试 |
| E005 | 格式错误 | 返回期望格式示例 |

## 版本兼容性

| 技能版本 | 兼容性说明 |
|---------|-----------|
| 1.0.x | 初始版本 |
| 1.1.x | 新增可选字段，向后兼容 |
| 2.0.x | 可能破坏兼容性，需明确升级说明 |

## 更新日志

- 2024-01-15: 初始版本发布
