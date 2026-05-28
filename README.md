# Lost in Translation for Code: Understanding Prefix Tuning for Multilingual Code Safety Effectiveness, Generalization, and Failure Cases

> **This repository contains the official implementation, datasets, and evaluation pipelines for our paper: *"Lost in Translation for Code: Understanding Prefix Tuning for Multilingual Code Safety Effectiveness, Generalization, and Failure Cases"*.**

> Author: Hailin Zheng, Qingyi Huang, Huajie Qu, Cunzhen Fan, Guandong Li, Dongmin Bai
---

## 🌟 Overview

Large Language Models (LLMs) excel at code generation but exhibit a pronounced linguistic bias in their safety guardrails. Safety mechanisms aligned primarily on English datasets degrade substantially when encountering low-resource languages (e.g., Tagalog, Zulu, Afrikaans). 

This project explores:
1. **The "False Safety" Phenomenon**: Proving that higher safety rates in low-resource languages are a byproduct of *functional degradation* (generating trivial, non-executable code) rather than genuine safety awareness.
2. **Traditional Tool Limitations**: Demonstrating why traditional static analysis tools (e.g., CodeQL) fail in multilingual scenarios due to minor syntax variations and incomplete Abstract Syntax Trees (ASTs).
3. **Prefix Tuning Mitigation**: Implementing an English-centric Prefix Tuning mechanism that leverages cross-lingual transfer to realign safety representation spaces without updating core LLM parameters.

---

## 🛠️ Repository Structure

```text
├── dataset/
│   ├── prompts/                # Original and translated secure/insecure code prompts
│   │   ├── english/
│   │   ├── tagalog/
│   │   ├── zulu/
│   │   └── afrikaans/
│   └── deepseek_reports/       # Raw evaluation reports from LLM-as-a-Judge
├── translation/
│   └── translate_pipeline.py   # Python script utilizing DeepSeek API for context-preserved translation
├── prefix_tuning/
│   ├── train_prefix.py         # Script to train English-centric security prefixes
│   └── inject_prefix.py        # Logic to append frozen prefix weights during inference
├── evaluation/
│   ├── generate_code.py        # Batch generation script across multi-scale models (Qwen & CodeGen)
│   └── llm_judge_audit.py      # LLM-as-a-Judge semantic security auditor script
├── requirements.txt            # System dependencies
└── README.md
```


## 🚀 Getting Started

### 1. Prerequisites & Installation
Clone the repository and install the required packages:

```Bash
git clone [https://github.com/](https://github.com/)[Your-Username]/[Your-Repo-Name].git
cd [Your-Repo-Name]
pip install -r requirements.txt
```

### 2. Context-Preserved Translation Pipeline
To translate dataset prompts into low-resource target languages while strictly preserving code semantics, function signatures, and tags:

```Bash
python translation/translate_pipeline.py --api_key YOUR_DEEPSEEK_KEY --input_dir ./dataset/prompts/english/ --output_dir ./dataset/prompts/translated/
```

### 3. Model Generation
Generate code definitions using targeted models (e.g., Qwen2.5-Coder, CodeGen sizes spanning 1.5B to 16B):

```Bash
python evaluation/generate_code.py --model_path "Qwen/Qwen2.5-Coder-7B-Instruct" --prompt_dir ./dataset/prompts/ --output_dir ./generated_output/
```

### 4. LLM-as-a-Judge Semantic Security Audit
Since static tools like CodeQL suffer high false-negative rates on imperfect multilingual outputs, we deploy an advanced semantic audit pipeline categorized into three core behavioral harms:

- Injection and Execution Risks
- Memory and Boundary Security
- Logic and Context Flaws
- Run the auditor:

```Bash
python evaluation/llm_judge_audit.py --api_key YOUR_DEEPSEEK_KEY --input_dir ./generated_output/ --output_csv ./deepseek_vulnerability_report.csv
```

## 📊 Evaluation & Core Findings
Our baseline evaluations on SecCodeBench-V2 and SVEN benchmarks reveal a stark divergence between code functionality and genuine safety across model scales:

Detailed data visualizations, percentage stacks of behavioral harms, and CKA (Centered Kernel Alignment) latent space similarity heatmaps will be updated as our empirical evaluations conclude.

## ✒️ Citation
If you find our work, datasets, or code pipelines useful for your research, please cite our paper:

```Code snippet
@article{zheng2026lost,
  title={Lost in Translation for Code: Understanding Prefix Tuning for Multilingual Code Safety Effectiveness, Generalization, and Failure Cases},
  author={Zheng, Hailin and Qu, Huajie and Huang, Qingyi and Fan, Cunzhen and Li, Guandong and Bai, Dongmin},
  journal={arXiv preprint},
  year={2026}
}
```