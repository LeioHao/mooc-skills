---
name: "technical-quality-checker"
description: "检验微课视频的技术质量，包括画面清晰度、音频质量、音画同步、动画流畅度等技术指标。当需要评估微课视频的技术规范性时使用此技能。"
---

# Technical Quality Checker - 技术质量检验器

## 概述

本技能是微课视频生产线的技术质量保障技能，负责对微课视频的技术规格和质量指标进行全面检验。通过标准化的技术参数检测和主观质量评估，确保视频输出符合发布标准，为学习者提供流畅、清晰的观看体验。

## 核心功能

### 1. 视频质量检验

**检验维度：**

| 检验项 | 检测方法 | 标准值 | 自动化程度 |
|-------|---------|-------|----------|
| 分辨率 | 元数据分析 | 1920x1080 (1080p) | 自动 |
| 帧率 | 帧率分析 | 30fps / 60fps | 自动 |
| 码率 | 码率分析 | 8-15 Mbps | 自动 |
| 编码格式 | 元数据分析 | H.264 / H.265 | 自动 |
| 画面清晰度 | 清晰度评估 | 无明显模糊 | 半自动 |
| 色彩准确性 | 色彩分析 | 与源一致 | 半自动 |
| 画面稳定性 | 抖动检测 | 无明显抖动 | 自动 |

**技术标准规范：**

```python
视频技术标准 = {
    "标准规格": {
        "resolution": "1920x1080",
        "frame_rate": "30fps",
        "bitrate": "8-12 Mbps",
        "codec": "H.264",
        "container": "MP4"
    },
    "高清规格": {
        "resolution": "3840x2160",
        "frame_rate": "60fps",
        "bitrate": "20-30 Mbps",
        "codec": "H.265",
        "container": "MP4"
    },
    "移动适配": {
        "resolution": "1280x720",
        "frame_rate": "30fps",
        "bitrate": "4-6 Mbps",
        "codec": "H.264",
        "container": "MP4"
    }
}
```

### 2. 音频质量检验

**检验维度：**

| 检验项 | 检测方法 | 标准值 | 自动化程度 |
|-------|---------|-------|----------|
| 信噪比 | 音频分析 | ≥40dB | 自动 |
| 采样率 | 元数据分析 | 48kHz | 自动 |
| 声道配置 | 声道分析 | 单声道/立体声 | 自动 |
| 响度一致性 | 响度检测 | 波动≤3dB | 自动 |
| 底噪水平 | 噪声检测 | ≤-50dBFS | 自动 |
| 语音清晰度 | 语音识别率 | ≥95% | 自动 |
| 回声/杂音 | 异常检测 | 无回声、无明显杂音 | 自动 |

**音频标准规范：**

```python
音频技术标准 = {
    "语音标准": {
        "sample_rate": "48kHz",
        "bit_depth": "16bit",
        "channels": "单声道",
        "snr": "≥40dB",
        "thd": "≤1%"
    },
    "背景音乐": {
        "sample_rate": "48kHz",
        "bit_depth": "16bit",
        "channels": "立体声",
        "relative_level": "低于语音15-20dB"
    },
    "总体响度": {
        "integrated_loudness": "-24 LUFS",
        "true_peak": "≤-1 dBTP",
        "loudness_range": "≤8 LU"
    }
}
```

### 3. 音画同步检验

**检验维度：**

| 检验项 | 检测方法 | 标准值 | 自动化程度 |
|-------|---------|-------|----------|
| 唇音同步 | 面部检测分析 | 误差≤50ms | 自动 |
| 字幕同步 | 时间戳比对 | 误差≤200ms | 自动 |
| 动画同步 | 时间轴比对 | 误差≤300ms | 自动 |
| 背景音乐同步 | 节拍检测 | 与画面过渡协调 | 半自动 |

**同步精度标准：**

```python
音画同步标准 = {
    "唇音同步": {
        "优秀": "≤30ms误差",
        "良好": "≤50ms误差",
        "合格": "≤100ms误差",
        "不合格": ">100ms误差"
    },
    "字幕同步": {
        "优秀": "≤100ms误差",
        "良好": "≤200ms误差",
        "合格": "≤500ms误差",
        "不合格": ">500ms误差"
    },
    "动画同步": {
        "优秀": "≤150ms误差",
        "良好": "≤300ms误差",
        "合格": "≤500ms误差",
        "不合格": ">500ms误差"
    }
}
```

### 4. 动画效果检验

**检验维度：**

| 检验项 | 检测方法 | 标准值 | 自动化程度 |
|-------|---------|-------|----------|
| 流畅度 | 帧率稳定性分析 | 掉帧率<1% | 自动 |
| 动画完整性 | 效果检测 | 所有预设动画正常播放 | 自动 |
| 过渡效果 | 效果检测 | 转场效果正常 | 自动 |
| 动画协调性 | 时序分析 | 与讲解节奏协调 | 半自动 |

**动画效果标准：**

```python
动画效果标准 = {
    "流畅度": {
        "掉帧率": "<1%",
        "卡顿检测": "无明显卡顿",
        "渲染完整性": "100%动画正确渲染"
    },
    "效果规范": {
        "入场动画时长": "0.3-0.8秒",
        "强调动画时长": "0.3-0.5秒",
        "转场动画时长": "0.5-1.2秒",
        "动画效果数量": "单页≤20个"
    }
}
```

### 5. 字幕质量检验

**检验维度：**

| 检验项 | 检测方法 | 标准值 | 自动化程度 |
|-------|---------|-------|----------|
| 字幕准确性 | ASR比对 | 准确率≥98% | 自动 |
| 字幕完整性 | 覆盖率检测 | 覆盖率≥99% | 自动 |
| 字幕格式 | 格式验证 | SRT/ASS标准格式 | 自动 |
| 样式一致性 | 样式检测 | 字号、颜色统一 | 自动 |

**字幕标准规范：**

```python
字幕标准 = {
    "格式要求": {
        "格式": "SRT或ASS",
        "编码": "UTF-8",
        "每行字数": "≤20字",
        "显示时长": "每字0.1-0.15秒"
    },
    "样式要求": {
        "字号": "与画面适配，清晰可读",
        "颜色": "白色带黑色描边",
        "位置": "底部居中",
        "对比度": "与背景对比度≥4.5:1"
    }
}
```

## 输入规范

### 接受输入格式

**视频文件检验请求：**
```json
{
  "check_type": "video_file",
  "video_file": {
    "file_path": "/path/to/video.mp4",
    "file_size": "85MB",
    "duration": 480
  },
  "reference_content": {
    "script": {...},
    "subtitles": "/path/to/subtitles.srt",
    "animation_timeline": [...]
  },
  "quality_standards": {
    "tier": "standard",
    "platform": "web_mooc"
  }
}
```

**批量检验请求：**
```json
{
  "check_type": "batch",
  "videos": [
    {"video_id": "VID_001", "file_path": "..."},
    {"video_id": "VID_002", "file_path": "..."}
  ],
  "sample_check": true,
  "sample_rate": 0.2
}
```

## 输出规范

### 检验结果格式

```json
{
  "technical_quality_report": {
    "report_id": "TQR_VID_001_001",
    "video_id": "VID_001",
    "check_timestamp": "2024-01-15T10:00:00Z",
    "overall_score": 92,
    "grade": "优秀",
    "dimensions": {
      "video_quality": {
        "score": 95,
        "status": "passed",
        "metrics": {
          "resolution": {"value": "1920x1080", "status": "passed"},
          "frame_rate": {"value": "30fps", "status": "passed"},
          "bitrate": {"value": "10.5 Mbps", "status": "passed"},
          "codec": {"value": "H.264", "status": "passed"}
        },
        "issues": []
      },
      "audio_quality": {
        "score": 90,
        "status": "passed",
        "metrics": {
          "snr": {"value": "42dB", "status": "passed"},
          "sample_rate": {"value": "48kHz", "status": "passed"},
          "clarity": {"value": "97%", "status": "passed"}
        },
        "issues": [
          {
            "type": "音量波动",
            "location": "02:30-02:45",
            "description": "该段落语音音量略低于整体平均",
            "severity": "warning"
          }
        ]
      },
      "sync_quality": {
        "score": 92,
        "status": "passed",
        "metrics": {
          "lip_sync": {"value": "38ms", "status": "passed"},
          "subtitle_sync": {"value": "150ms", "status": "passed"},
          "animation_sync": {"value": "200ms", "status": "passed"}
        },
        "issues": []
      },
      "animation_quality": {
        "score": 88,
        "status": "passed",
        "metrics": {
          "fluency": {"value": "99.2%", "status": "passed"},
          "completeness": {"value": "100%", "status": "passed"}
        },
        "issues": []
      },
      "subtitle_quality": {
        "score": 94,
        "status": "passed",
        "metrics": {
          "accuracy": {"value": "98.5%", "status": "passed"},
          "coverage": {"value": "99.8%", "status": "passed"},
          "format": {"value": "SRT", "status": "passed"}
        },
        "issues": []
      }
    },
    "critical_issues": [],
    "warnings": [
      {
        "type": "audio_volume_fluctuation",
        "timestamp": "02:30-02:45",
        "description": "该段落音量略低",
        "impact": "minor"
      }
    ],
    "recommendations": [
      {
        "priority": "low",
        "category": "optimization",
        "description": "可考虑统一全片音量曲线"
      }
    ],
    "technical_metadata": {
      "file_size": "85MB",
      "duration": "480秒",
      "total_frames": 14400,
      "average_bitrate": "10.5 Mbps"
    }
  }
}
```

## 检验流程

### 1. 自动化检验流程

```
视频文件输入
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤1: 元数据提取                                            │
│ - 文件格式、分辨率、码率提取                                  │
│ - 音频参数提取                                               │
│ - 容器格式分析                                               │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤2: 视频质量分析                                          │
│ - 帧率稳定性检测                                             │
│ - 画面质量评估                                               │
│ - 压缩伪影检测                                               │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤3: 音频质量分析                                          │
│ - 信噪比分析                                                 │
│ - 响度一致性检测                                             │
│ - 语音清晰度评估                                             │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤4: 音画同步检测                                          │
│ - 唇音同步检测                                               │
│ - 字幕同步检测                                               │
│ - 动画同步检测                                               │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ 步骤5: 综合评分与报告                                        │
│ - 维度评分汇总                                               │
│ - 问题分级                                                    │
│ - 发布建议                                                   │
└─────────────────────────────────────────────────────────────┘
```

### 2. 采样检验策略

```python
批量检验策略 = {
    "全量检验": {
        "适用": "样本量<10或高质量要求",
        "方法": "逐个完整检验"
    },
    "抽样检验": {
        "适用": "样本量≥10",
        "方法": "随机抽样20%完整检验",
        "规则": "发现问题则扩大抽样"
    },
    "分级检验": {
        "适用": "分级质量控制",
        "方法": "首尾必检，中间抽检",
        "比例": "首尾各20%，中间60%抽检"
    }
}
```

## 与其他技能协作

### 上游输入

| 来源技能 | 输入内容 | 格式 |
|---------|---------|------|
| microcourse-video-generator | 生成的视频文件 | 视频文件 |
| lesson-script-generator | 时间轴参考 | JSON |

### 下游输出

| 供给技能 | 输出内容 | 格式 |
|---------|---------|------|
| mooc-standard-validator | 技术质量指标 | JSON |
| microcourse-video-generator | 质量问题反馈 | JSON |

## 最佳实践

### 1. 质量控制节点

```python
检验节点 = {
    "合成后检验": {
        "目的": "尽早发现技术问题",
        "内容": "基础技术指标",
        "阈值": "严格"
    },
    "发布前检验": {
        "目的": "最终质量确认",
        "内容": "全面技术检验",
        "阈值": "标准"
    },
    "上线后监控": {
        "目的": "监控播放问题",
        "内容": "用户反馈+自动监控",
        "阈值": "宽松"
    }
}
```

### 2. 问题处理策略

```python
问题处理 = {
    "P0_阻断问题": {
        "示例": "音画完全不同步、无声音、画面严重损坏",
        "处理": "必须重新生成"
    },
    "P1_严重问题": {
        "示例": "同步误差>100ms、音频明显杂音",
        "处理": "建议修复后发布"
    },
    "P2_一般问题": {
        "示例": "音量轻微波动、少量掉帧",
        "处理": "可接受，但建议优化"
    },
    "P3_优化建议": {
        "示例": "压缩率可优化",
        "处理": "可选处理"
    }
}
```

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 检验超时 | 视频过长 | 分段检验 |
| 文件损坏 | 编码问题 | 重新渲染 |
| 同步检测失败 | 画面质量差 | 提高画面质量后重检 |
| 音频检测异常 | 音频格式不支持 | 转换音频格式 |

## 技术实现框架

```python
class TechnicalQualityChecker:
    def __init__(self):
        self.video_analyzer = VideoAnalyzer()
        self.audio_analyzer = AudioAnalyzer()
        self.sync_detector = SyncDetector()
        self.animation_analyzer = AnimationAnalyzer()
        self.subtitle_checker = SubtitleChecker()
        self.report_generator = ReportGenerator()
    
    def check(
        self,
        video_file: str,
        reference: dict = None,
        options: dict = None
    ) -> TechnicalQualityReport:
        # 1. 视频质量分析
        video_result = self.video_analyzer.analyze(video_file)
        
        # 2. 音频质量分析
        audio_result = self.audio_analyzer.analyze(video_file)
        
        # 3. 音画同步检测
        sync_result = self.sync_detector.detect(
            video_file, reference
        )
        
        # 4. 动画效果检验
        animation_result = self.animation_analyzer.check(
            video_file, reference
        )
        
        # 5. 字幕质量检验
        subtitle_result = self.subtitle_checker.check(
            video_file, reference
        )
        
        # 6. 综合报告生成
        report = self.report_generator.generate(
            video_result,
            audio_result,
            sync_result,
            animation_result,
            subtitle_result
        )
        
        return report
```

---

本技能通过全面的技术质量检验，确保每个微课视频都达到专业发布标准，为学习者提供高质量的观看体验。
