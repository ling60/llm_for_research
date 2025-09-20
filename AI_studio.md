# AI Studio 使用分享（研究向）

- 2025-09更新（yxiaohan）

---
## 1. 为什么选择 AI Studio + Gemini Pro
### 丰富的背景知识
基本没有对手。这一点对交叉学科以及前沿创新问题尤为重要。GPT5接近。
### 1M长上下文
基本没有对手。目前最接近的是通义qwen3。[LongBench](https://longbench2.github.io/)
### 推理深度
有各种排名，但实测旗舰模型差距不大，包括deepseek。
### 免费无限次使用
同等质量下没有对手。
### AI Studio无提示词污染

- 绝大部分大模型平台都会在用户输入的提示词前后，自动添加一些“辅助提示词”，这些提示词会影响模型的回答，且无法查看和修改。AI Studio没有这个问题，可以完全自定义提示词（理论上）。原理参见上下文部分。

- 实测：AI Studio的回答质量远超同模型的gemini cli（后者也可能受推理窗口限制）。

---

---
## 2. 功能介绍
### 网址：https://aistudio.google.com/
### 界面
#### 模型选择：Gemini Pro(目前为2.5版本)
#### 系统提示词
有意义，但不必过于复杂。越好的模型，提示词就可以越简单。
#### 温度：0.5 - 1.0
平时使用1，代码生成时可向下调整（个人使用0.7-0.8）
#### Thinking budget: 最大（32k）
建议使用最大值
#### Grounding with Google Search
事实核查时需打开，但会影响思考质量；平时可关闭
#### URL context
同上

### 上传类型

- 文本 （包括代码）
- 图片
- PDF转换
- 批量文件上传
- 同时可上传音视频等

### 生成类型

- 文本 (markdown格式)
- 图片（svg代码）， 或nano banana直接生成图片

---
## 3. 使用技巧 （通用于其他旗舰模型）
### 大模型的生成＝f（模型特性，随机性，上下文）

- 模型特性：选择模型后固定
- 随机性：通过温度、top-p等参数调节
- **上下文：调节大模型的核心手段**
在当前请求中被送入模型、用于决定下一 token 的一切**对模型可见**的信息（包括但不限于：系统/开发者指令、当前输入、保留下来的对话历史、外部资料与工具结果等）。

### 自动生成系统提示词
You are an experienced prompt engineer with excellent knowledge of writing prompts for LLM agents. Given users' descriptions, you will thoughtfully analyze their needs, and create fine-grained, well-structured and concise system instructions for the LLM.
### 上下文大小
模型准确率和已使用上下文大小成反比。

- 256k (推荐) （如果使用非gemini模型，建议不超过128k）
- 500k 以上(不推荐)
- 提示总结对话历史，另开新对话
- 注：上下文大小是以token计数的，1 token 约等于 4 个英文字符或 1 个汉字。

### 上下文管理

- 不同主题要另开新对话
- branch（分支）功能 (AI Studio独有)

### 同一问题，多次尝试（抽卡）
LLM随机性问题
### 语言

- 中文非常优秀，可以直接使用
- 个人习惯：熟悉的领域使用英文，不熟悉的领域使用中文。
- 原因：在自己熟悉的专业领域，使用英文提问可以确保关键术语准确无误地匹配模型训练语料（毕竟很多专业资料是英文的）；而在不熟悉领域，用中文提问可以让模型回答得更详细易懂些，方便自己理解。

---
## 4. 大模型通病
### 幻觉

- 降低温度
- 生成引用
- 自我搜索验证
- 事实核查

### 讨好型人格

- 提示词加入客观、批判类指令，如：请在你的回答中，扮演一个严谨的、持怀疑态度的领域专家。请指出我请求中可能存在的逻辑漏洞或不合理之处，并优先考虑事实的准确性而非礼貌。

### 过于乐观
### 辅助思考，不能代替思考

---
## 5. Gemini Research （免费版）
### 网址：https://gemini.google/overview/deep-research/
### 功能

- 文献综述
- 事实核查
- 更新背景知识
  
### 不足

- 免费次数过少
- 基模型较差（flash，而非pro，氪金可升为pro）
  
### 免费平替
#### 具备深度研究功能的旗舰大模型

- Grok
- 通义（[Tongyi](https://www.tongyi.com/)）
- 豆包

---
## 6. Q&A
### AI Studio 和 Gemini Research 的区别

- AI Studio 适合讨论对话，Gemini Research 适合文献综述和事实核查。
- 推荐流程：先在 AI Studio 讨论，得到初步方案后，再用 Gemini Research 做文献综述和背景知识更新。然后再回到 AI Studio 继续讨论和完善方案。
  
### Temperature（温度）是什么？默认设置是多少？怎么调？

- 作用：控制生成的“发散度/创意度”。数值越低越稳健，越高越多样。
- 范围与默认（Gemini 2.5 Pro）：0.0–2.0，默认 1.0。
- 快速建议
事实问答/代码修复：0.5–0.8（更严谨）
综合问答/说明文：0.8–1.0（通用，默认就好）
头脑风暴/创作：1.0–1.2（更有想象力，也更易跑题）

### Top-P（核采样）是什么？默认设置是多少？怎么调？

- 作用：只在“累计概率达到 p 的候选词集合”里采样，切掉长尾不太可能的词。
- 范围与默认（Gemini 2.5 Pro）：0.0–1.0，默认 0.95。

- 快速建议
想更稳：在不改温度的前提下，把 Top-P 从 0.95 降到 0.9–0.8（更少冷门词）。
想更活：把 Top-P 提到 0.97–1.0（更开放，但更易发散）。
一般先调温度，必要时再小幅微调 Top-P。

### 如何提示 Gemini 总结对话历史，方便“另开新对话”继续？

- 请帮我总结我们到目前为止的对话。我需要一个简洁的摘要，包含以下几点：1. 我们讨论的核心问题；2. 已经确定的关键信息和数据；3. 尚未解决的开放性问题。请用要点格式列出，方便我开启一个新的对话继续讨论。
- Please provide a concise summary of our entire conversation. The goal is to capture all the essential context so I can use this for a new chat and continue our work.
Please structure the summary with the following sections:
Main Goal/Objective: What was the primary problem or topic we were discussing?
Key Information & Decisions: What were the most important facts, findings, or decisions we made?
Next Steps / Open Questions: What are the remaining questions or the immediate next steps we identified?
Please use a bullet-point format for clarity.

### 什么是‘Token’，和字符的区别是什么？

- 简单来说，token 是模型处理文本的基本单位。近似于一个汉字或一个英文单词，但并不完全等价。经验估计，1 token 大约等于 4 个英文字符，或 1 个汉字。具体取决于模型采用的分词方法。

### 为什么不推荐ChatGPT？

- 免费版质量太差，且为了通用性，加入过多辅助提示词。感觉不比豆包强太多。
- 其新推出的GPT5 codex编程能力极强，但不在本文主题。

### Gemini 2.5 Pro 的缺点？

- Agent （指令遵循、工具调用）能力太低。在目前（2025年9月）阶段，是同类旗舰模型中最差的。但不影响讨论。

---
#### 感谢ChatGPT和Gemini提供的事实核查，以及Q&A部分的撰写帮助
