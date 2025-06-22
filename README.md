````markdown
# Text Quality Evaluator  
文本质量评估器

> Lightweight, rule-based text quality evaluator that flags noisy documents and tells you whether they’re ready for clustering, LDA, BERTopic, and other NLP tasks.

---

## 📑 项目简介 | Project Overview

- **中文**  
  这是一个开箱即用的文本质量评估器，专门帮你在「聚类 / LDA / BERTopic / 情感分析」等 NLP 任务之前过滤低质文档。它通过一系列启发式规则（信息熵、重复度、符号比例、TTR 等）给每篇文章打 0-100 分，并输出是否适合各类算法。

- **English**  
  A plug-and-play tool that runs a quick “health-check” on any plain-text file. It scores each document (0-100) using heuristics such as entropy, duplication, punctuation balance, and lexical richness, then labels whether the text is suitable for clustering, LDA, BERTopic, sentiment analysis, and more.

---

## ✨ 主要特性 | Key Features

| 指标 / Metric                | 作用 / Purpose                              |
|------------------------------|---------------------------------------------|
| Shannon Entropy              | 信息量 & 信息密度 / Information density     |
| Duplicate Line / Word / 3-gram| 检测模板化 & 日志噪声 / Boilerplate filter |
| TTR (Type-Token Ratio)       | 词汇丰富度 / Lexical variety               |
| Punctuation & Symbol Ratio   | 标点/符号噪声 / Punctuation noise           |
| Paragraph Entropy            | 结构合理性 / Paragraph structure           |
| Suitability Flags            | 聚类、LDA、BERTopic、NER、情感、摘要等      |

---

## 🚀 快速开始 | Quick Start

```bash
pip install -r requirements.txt   # 只需要 chardet（可选）
````

```python
from evaluate_text_quality import evaluate_text_quality

report = evaluate_text_quality("sample.txt")
print(report["score"], report["verdict"])
# {'score': 82, 'verdict': 'good', 'suitable_for_lda': True, ...}
```

### 返回字段 | Returned Fields (partial)

| 字段 / Key                | 含义 / Meaning                          |
| ----------------------- | ------------------------------------- |
| `score`                 | 综合得分 0-100 / Overall score            |
| `verdict`               | good / fair / poor                    |
| `duplicate_ngram_ratio` | 3-gram 重复比例 / 3-gram duplication rate |
| `suitable_for_lda`      | 是否适合 LDA / Ready for LDA?             |
| `penalties`             | 扣分原因列表 / Reasons for deductions       |

---

## 🛠️ 配置 | Configuration

所有阈值集中在 `CFG` 字典中：

```python
CFG = {
    "min_ttr": 0.20,
    "max_duplicate_ngram_ratio": 0.60,
    "min_chars_clustering": 1000,
    ...
}
```

---

## 📈 典型流程 | Typical Pipeline

```
Raw TXT/OCR → evaluate_text_quality → 过滤不合格 → NLP (LDA / BERTopic / Sentiment)
```

---

## 🗺️ Roadmap

* [ ] 多语言停用词支持
* [ ] IDF-weighted 信息量指标
* [ ] Web 前端拖拽上传实时评分
* [ ] 模型（LLM）辅助的语义噪声检测

