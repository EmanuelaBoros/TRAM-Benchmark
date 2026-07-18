# TRAM: Benchmarking Temporal Reasoning for Large Language Models

TRAM is a benchmark for evaluating temporal reasoning in large language models. It covers ten complementary task families that test whether models can reason about event order, duration, frequency, temporal arithmetic, temporal relations, causality, ambiguity, typical times, storytelling, and temporal natural language inference.

The benchmark was introduced in [TRAM: Benchmarking Temporal Reasoning for Large Language Models](https://aclanthology.org/2024.findings-acl.382/) (Findings of ACL 2024). It contains **526,668** problems built from existing NLU datasets, human-written templates and questions, web sources, and programmatic generation. Each task includes a test set and a small few-shot development set.

## Repository Contents

```text
.
├── datasets/                         # Released TRAM datasets as zip archives
├── data_processing/                  # Notebooks used to construct task datasets
├── image_sources/                    # Figures used in the repository
├── datasheet_for_TRAM_benchmark.pdf  # Dataset datasheet
├── data_sources.txt                  # External source datasets
├── LICENSE
└── README.md
```

## Dataset Overview

All released datasets are available in [`datasets/`](datasets/). Most tasks are provided as multiple-choice questions (MCQ), and several tasks also include short-answer question (SAQ) versions.

<div align="center">
  <img width="95%" alt="Overview of the TRAM temporal reasoning task families" src="image_sources/dataset_set.png">
</div>

| Task archive | Test files | Few-shot files | Main format | Test instances |
|---|---:|---:|---|---:|
| `ambiguity_resolution.zip` | 1 | 1 | MCQ | 3,624 |
| `arithmetic.zip` | 2 | 2 | MCQ, SAQ | 15,584 |
| `causality.zip` | 2 | 1 | MCQ | 590 original, 600 mirrored |
| `duration.zip` | 2 | 2 | MCQ, SAQ | 7,197 |
| `frequency.zip` | 2 | 2 | MCQ, SAQ | 4,628 |
| `nli_mcq.zip` | 1 | 1 | MCQ | 282,134 |
| `nli_saq.zip` | 1 | 1 | SAQ | 282,134 |
| `ordering.zip` | 2 | 2 | MCQ, SAQ | 29,452 |
| `relation.zip` | 2 | 2 | MCQ, SAQ | 102,457 |
| `storytelling.zip` | 1 | 1 | MCQ | 67,251 |
| `typical_time.zip` | 2 | 2 | MCQ, SAQ | 13,010 |

The MCQ files use answer labels such as `A`, `B`, `C`, or `D`. SAQ files use the natural-language answer directly.

## File Format

Each archive contains CSV files with the following naming pattern:

| Pattern | Meaning |
|---|---|
| `*_mcq.csv` | Test split for multiple-choice evaluation |
| `*_shots_mcq.csv` | Few-shot development examples for MCQ prompting |
| `*_saq.csv` | Test split for short-answer evaluation, where available |
| `*_shots_saq.csv` | Few-shot development examples for SAQ prompting, where available |

Typical MCQ columns:

```text
Question, Option A, Option B, Option C, [Option D], Answer, Category/Source
```

Some tasks include additional context columns, such as `Premise`, `Hypothesis`, or `Story`.

Typical SAQ columns:

```text
Question, Answer, Category/Source
```

For `causality.zip`, `causality_mirrored_mcq.csv` contains mirrored examples corresponding to the original causality instances.

## Quick Start

Clone the repository:

```bash
git clone https://github.com/EmanuelaBoros/TRAM-Benchmark.git
cd TRAM-Benchmark
```

Unzip one dataset:

```bash
mkdir -p data/typical_time
unzip datasets/typical_time.zip -d data/typical_time
```

Load a split with Python:

```python
import pandas as pd

df = pd.read_csv("data/typical_time/typical_time_mcq.csv")
print(df.head())
print(df.columns)
```

Simple MCQ evaluation:

```python
def accuracy(predictions, gold):
    return sum(p == g for p, g in zip(predictions, gold)) / len(gold)

gold = df["Answer"].tolist()
# predictions should contain labels such as "A", "B", "C", or "D"
```

## Evaluation

Most TRAM tasks are evaluated with **accuracy**, since they are formulated as questions with one correct answer. For tasks with fixed class labels and imbalanced distributions, especially **temporal relation** and **temporal NLI**, the paper reports both **accuracy** and **F1**.

The original experiments evaluate:

- LLMs such as GPT-4, GPT-3.5-turbo, PaLM-bison-chat, and Llama-2-13b-chat.
- Zero-shot and 5-shot prompting.
- Standard prompting and chain-of-thought prompting.
- BERT-style baselines using sequence classification or multiple-choice classification heads.

## Source Datasets

TRAM builds on several existing datasets:

- [MCTACO](https://github.com/CogComp/MCTACO)
- [COPA](https://people.ict.usc.edu/~gordon/copa.html)
- [SQuAD](https://huggingface.co/datasets/squad)
- [TempEval-3](https://figshare.com/articles/dataset/TempEval-3_data/9586532)
- [MNLI](https://huggingface.co/datasets/multi_nli)
- [SNLI](https://huggingface.co/datasets/snli)
- [ROCStories / Story Cloze Test](https://cs.rochester.edu/nlp/rocstories/)

See [`data_sources.txt`](data_sources.txt) and the paper appendix for more detail on data construction.

## Datasheet

A dataset datasheet is provided at [`datasheet_for_TRAM_benchmark.pdf`](datasheet_for_TRAM_benchmark.pdf).

## Citation

If you use TRAM, please cite:

```bibtex
@inproceedings{wang-zhao-2024-tram,
    title = "{TRAM}: Benchmarking Temporal Reasoning for Large Language Models",
    author = "Wang, Yuqing  and
      Zhao, Yun",
    editor = "Ku, Lun-Wei  and
      Martins, Andre  and
      Srikumar, Vivek",
    booktitle = "Findings of the Association for Computational Linguistics: ACL 2024",
    month = aug,
    year = "2024",
    address = "Bangkok, Thailand",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2024.findings-acl.382/",
    doi = "10.18653/v1/2024.findings-acl.382",
    pages = "6389--6415"
}
```

## License

This repository is released under the MIT License. See [`LICENSE`](LICENSE) for details.
