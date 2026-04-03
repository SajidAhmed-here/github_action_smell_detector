# Configuration Quality Failures in LLM-Generated GitHub Actions Workflows

> **Accepted at CI/CD Industry Workshop (CCIW) 2026** — co-located with ICST 2026, South Korea (22nd May 2026)
>
> **Paper ID:** cciw-p7-p

This repository is the official replication package for the paper:

> *"Configuration Quality Failures in LLM-Generated GitHub Actions Workflows: A Comparison with Human-Validated Baselines"*  
> Sajid Ahmed, Samia Rahman, Kazi Tasnia Farin Ifa, Proma Chowdhury  
> East West University & University of Dhaka, Bangladesh

---

## What This Paper Is About

GitHub Actions workflows automate software build, test, and deployment pipelines. With LLMs increasingly generating these workflows from natural language prompts, we investigated whether LLM-generated workflows contain more **configuration smells** — structural anti-patterns that reduce security, reliability, or performance — compared to human-validated baselines.

We analyzed **1,318 paired workflows** across **six LLMs** using a static detector with **15 rule-based smell checks**.

**Key Findings:**
- All six LLMs produced more smells than the human baseline, averaging **1.8× higher density**
- `MissingTimeout` was the most frequent smell (67–82% of all smells)
- `UnpinnedAction` showed the largest gap — LLMs produced up to **12× more instances**
- `InsecureScript`, `PlaintextSecret`, `AllowFailure`, and `RedundantSteps` appeared **exclusively** in LLM outputs
- The **Build stage** had the largest difference — CodeLlama-7B reached 40.79/KLOC vs. 6.78/KLOC in references

---

## Repository Structure

```
project-root/
├── notebooks/
│   └── smell_detection_analysis.ipynb   # Google Colab notebook (smell detection + analysis)
├── results/
│   ├── smell_results.csv                # All detected smells with type, job, and stage
│   └── summary_statistics.csv          # Per-model smell count and density breakdown
└── README.md                           # You are here
```

---

## Dataset

We used the paired workflow dataset published by Ghaleb and Rathnayake (ICSME 2025).

📁 **Original Dataset:** [https://github.com/Taher-Ghaleb/ICSME25-LLM4CI/tree/main/data](https://github.com/Taher-Ghaleb/ICSME25-LLM4CI/tree/main/data)

The dataset contains 1,318 paired workflows per model — one human-validated baseline and one LLM-generated workflow for the same task:

| CSV File | Model |
|----------|-------|
| `GitHubActions_gpt-4.1_output.csv` | GPT-4.1 |
| `GitHubActions_gpt-4o_output.csv` | GPT-4o |
| `GitHubActions_gemma3-12b_output.csv` | Gemma3-12B |
| `GitHubActions_llama3.1-8b_output.csv` | LLaMA3.1-8B |
| `GitHubActions_codellama-7b_output.csv` | CodeLlama-7B |
| `GitHubActions_codegemma-7b_output.csv` | CodeGemma-7B |

> Download the CSV files from the link above. You will upload them manually into the Colab session when running the notebook (instructions inside the notebook).

---

## How to Run

All experiments were conducted using **Google Colab**. No local setup is required.

### Step 1 — Open the Notebook

Open the notebook directly in Google Colab:

📓 [`smell_detection_analysis.ipynb`](notebooks/Github_Smell_detector.ipynb)


### Step 2 — Upload the Dataset

When prompted in the notebook, upload the CSV files downloaded from the dataset link above. The notebook includes a file upload cell for this.

### Step 3 — Run All Cells

Run all cells in order. The notebook will:
1. Apply all **15 smell checks** to each workflow file
2. Record each smell with its **type**, **job name**, and **pipeline stage**
3. Compute **smell count**, **density per KLOC**, and **stage-wise breakdowns**
4. Generate result CSVs and summary tables

### Step 4 — Download Results

The notebook will automatically save results as CSV files, which you can download from the Colab session at the end.

---

## Smell Catalog

The detector implements **15 smell types** across three categories, adapted from prior CI/CD smell catalogs for GitHub Actions YAML syntax:

| Category | Smell | Description |
|----------|-------|-------------|
| **Security** | UnpinnedAction | Action not pinned to a commit SHA |
| | BroadPermissions | Workflow uses overly broad permissions |
| | MissingPermissionsBlock | No permissions block declared |
| | PlaintextSecret | Secret or token written in plaintext |
| | HardcodedEnvValue | Sensitive value hardcoded in env block |
| | InsecureScript | Remote script executed via pipe (e.g., `curl \| bash`) |
| | SelfHostedRunner | Uses self-hosted runner without justification |
| | UnpinnedDockerImage | Container image not pinned to a digest |
| **Reliability** | ContinueOnError | Step uses `continue-on-error: true` |
| | AllowFailure | Job configured to allow failure |
| | MissingTimeout | No timeout set for job or step |
| | MissingCheckout | Workflow missing a checkout step |
| | ManualOnlyWorkflow | Workflow only triggered manually |
| **Performance** | MissingCache | No dependency caching configured |
| | RedundantSteps | Duplicate steps across jobs |
| | LargeMonolithicJob | Single oversized job with no parallelism |

---

## Results Summary

| Model | Total Smells | Density (/KLOC) | Avg / Workflow |
|-------|-------------|-----------------|----------------|
| Human-Validated | 616 | 42.17 | 0.467 |
| GPT-4.1 | 1,543 | 84.53 | 1.171 |
| GPT-4o | 1,333 | 87.82 | 1.011 |
| Gemma3-12B | 816 | 60.54 | 0.619 |
| LLaMA3.1-8B | 1,140 | 63.57 | 0.865 |
| CodeLlama-7B | 1,641 | 84.10 | 1.245 |
| CodeGemma-7B | 1,386 | 73.88 | 1.052 |

---

## How to Cite

If you use this replication package, please cite our paper:

```bibtex
@inproceedings{ahmed2026configsmells,
  title     = {Configuration Quality Failures in LLM-Generated GitHub Actions Workflows: 
               A Comparison with Human-Validated Baselines},
  author    = {Sajid Ahmed, Samia Rahman, Kazi Tasnia Farin Ifa, and Proma Chowdhury},
  booktitle = {Proceedings of the CI/CD Industry Workshop (CCIW), ICST 2026},
  year      = {2026},
  organization = {IEEE}
}
```


## License

Code is licensed under the **MIT License** — see `LICENSE` for details.

Data files are licensed under **CC BY 4.0** — free to share and adapt with credit.
