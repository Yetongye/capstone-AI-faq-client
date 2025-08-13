# 情感识别功能使用指南

## 功能概述

本AI智能客服系统现已集成情感识别功能，能够自动检测用户的情绪状态，并在用户表现出负面情绪（如急躁、不满意、投诉需求）时，自动提供人工客服转接服务。

## 功能特性

### 1. 情感检测类型

系统能够识别以下情感状态：

- **愤怒情绪** (`angry`): 检测关键词如 "angry", "mad", "furious", "pissed", "irritated", "annoyed", "frustrated"
- **急躁情绪** (`impatient`): 检测关键词如 "hurry", "urgent", "asap", "immediately", "quickly", "fast", "now", "waiting too long"
- **不满情绪** (`dissatisfied`): 检测关键词如 "terrible", "awful", "horrible", "useless", "stupid", "worst", "hate", "disappointed"
- **投诉需求** (`complaint`): 检测关键词如 "complain", "complaint", "report", "escalate", "manager", "supervisor"
- **转人工需求** (`transfer`): 检测关键词如 "human", "person", "agent", "representative", "transfer", "speak to someone", "talk to someone"

### 2. 情绪强度检测

系统还会检测以下情绪强度指标：
- **过度标点符号**: 超过2个感叹号表示情绪激动
- **大写字母比例**: 超过50%大写字母表示用户情绪激动

### 3. 自动人工转接

当满足以下条件时，系统会自动触发人工客服转接：
- 检测到投诉或转人工相关关键词
- 情绪评分达到3分或以上（多种负面情绪同时出现）

## 使用示例

### 示例1：愤怒情绪检测

**用户输入**: "I am so frustrated with this system!"

**系统响应**: 
- 检测到愤怒情绪
- 在回答中添加同理心表达
- 提供额外的人工客服选项

### 示例2：投诉需求自动转接

**用户输入**: "I need to complain about something. Can I speak to a manager?"

**系统响应**: 
- 自动触发人工客服转接
- 显示"🔄 Connecting to human agent..."指示器
- 提供专业的转接话术

### 示例3：急躁情绪处理

**用户输入**: "I need help ASAP! This is urgent!"

**系统响应**: 
- 检测到急躁情绪
- 在回答中体现理解和紧迫性
- 提供快速解决方案

## 界面特性

### 1. 视觉指示器

- **人工转接指示器**: 红色背景的"🔄 Connecting to human agent..."动画
- **情感标签**: 显示检测到的具体情感类型
- **特殊样式**: 需要人工转接的消息有特殊的红色边框和背景

### 2. 响应话术

系统提供三种层次的同理心话术：

1. **轻度负面情绪**: "I understand this might be frustrating..."
2. **中度负面情绪**: "I understand your concern..."
3. **重度负面情绪/投诉**: "I understand your concern and I want to make sure you get the best possible help..."

## 技术实现

### 后端API增强

聊天API (`/api/chat`) 现在返回额外的情感分析信息：

```json
{
  "question": "用户问题",
  "answer": "AI回答",
  "source": "faq_match|ai_generated|human_transfer",
  "confidence": "high|medium|low",
  "similarity": 0.85,
  "emotion_analysis": {
    "emotions": ["angry", "frustrated"],
    "emotion_score": 2,
    "needs_human": false,
    "sentiment": "negative"
  },
  "requires_human": false
}
```

### 前端组件更新

- `MessageBubble` 组件支持显示情感指示器和人工转接状态
- `ChatPage` 组件处理人工转接逻辑
- 新增CSS样式支持情感识别界面元素

## 测试功能

可以使用以下测试用例验证情感识别功能：

```bash
# 在后端目录运行测试脚本
cd faq-backend
python test_emotion_recognition.py
```

### 测试用例

1. **愤怒测试**: "I am so frustrated with this system!"
2. **急躁测试**: "I need help ASAP! This is urgent!"
3. **投诉测试**: "This is terrible and useless. I want to complain!"
4. **转人工测试**: "Can I speak to a human agent please?"
5. **大写测试**: "I AM REALLY MAD ABOUT THIS!!!"
6. **正常测试**: "Hello, how can I reset my password?"

## 配置选项

### 情感检测阈值

可以在 `ai_service.py` 中调整以下参数：

- **情感评分阈值**: 默认3分触发人工转接
- **大写字母比例阈值**: 默认50%
- **感叹号数量阈值**: 默认2个

### 关键词库扩展

可以在 `negative_emotion_keywords` 字典中添加更多关键词：

```python
self.negative_emotion_keywords = {
    'angry': ['angry', 'mad', 'furious', ...],
    'impatient': ['hurry', 'urgent', 'asap', ...],
    # 添加更多关键词
}
```

## 最佳实践

1. **及时响应**: 当检测到负面情绪时，系统会优先提供同理心回应
2. **人工备选**: 始终为用户提供转接人工客服的选项
3. **情绪缓解**: 使用温和、理解的语言来缓解用户情绪
4. **快速解决**: 对于急躁用户，优先提供快速解决方案

## 注意事项

1. 情感识别基于关键词匹配，可能存在误判
2. 建议结合人工审核来提高准确性
3. 定期更新关键词库以适应用户表达习惯
4. 监控转接率，避免过度转接影响效率

## 未来扩展

1. **机器学习模型**: 集成更先进的情感分析模型
2. **多语言支持**: 支持中文等其他语言的情感识别
3. **情绪历史**: 记录用户情绪变化趋势
4. **个性化响应**: 根据用户历史情绪调整回应策略