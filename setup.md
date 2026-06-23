# Setup — L10 — Transformers, Attention & GenAI

> One-time environment setup. **If you already set up the `dsai-m3` environment for L01–L09, you're almost done — L10 needs `transformers >= 4.40` and `sentence-transformers >= 3.0`. If you created the environment from any lesson's `environment.yml`, both are already included; otherwise run `pip install transformers sentence-transformers` once inside the activated environment. Then just clone this repo and pick the `dsai-m3` kernel.**

This guide gets you from "fresh machine" to "running the L10 notebooks". It takes about 15 minutes the first time (plus model downloads).

---

## 1. Prerequisites

You need:

- **Git** — to clone this repository
- **Miniconda** (or Anaconda) — to create the Python environment. [Install Miniconda →](https://docs.conda.io/projects/miniconda/en/latest/)
- **VS Code** — with the Python and Jupyter extensions. [Download VS Code →](https://code.visualstudio.com/)

Check git and conda are working:

```bash
git --version
conda --version
```

---

## 2. Clone this repository

```bash
git clone https://github.com/su-ntu-ctp/6m-data-3.10-Transformers-Attention-GenAI.git
cd 6m-data-3.10-Transformers-Attention-GenAI
```

---

## 3. Create the conda environment

From inside the `dsai-m3-l10-learner` folder:

```bash
conda env create -f environment.yml
```

This creates an environment called **`dsai-m3`** with Python, pandas, numpy, scikit-learn, PyTorch, transformers, sentence-transformers, and Jupyter. It takes 5–10 minutes.

> **Already have a `dsai-m3` environment from L01–L09?** You don't need to recreate it. Just confirm the two libraries are present:
>
> ```bash
> conda activate dsai-m3
> python -c "import transformers, sentence_transformers; print(transformers.__version__)"
> ```
>
> If that errors, run `pip install "transformers>=4.40" "sentence-transformers>=3.0"` once.

Activate the environment:

```bash
conda activate dsai-m3
```

---

## 4. First-run downloads (automatic)

- **Data (included in the repo):** `notebooks/data/northstar_catalogue.csv` (reused from L09).
- **Models:** the pre-class notebook (`01_monday_morning.ipynb`) downloads three models from Hugging Face into `~/.cache/huggingface/`: `distilbert-sst-2` (~268 MB), `bert-base-NER` (~436 MB), and `bart-large-mnli` (~1.6 GB, for zero-shot classification). The in-class notebooks then add `SmolLM2-360M-Instruct` (~720 MB). Total cache after the whole lesson: ~3 GB.

> **No GPU?** L10 runs on CPU, but SmolLM2 generation is slow (~5–15 tokens/sec) vs near-instant on a free Colab T4 GPU. Notebooks 03, 04, the assignment, and the extensions each have an **Open in Colab** badge at the top — click it, then **Runtime → Change runtime type → T4 GPU**.

---

## 5. `.env` file — required on macOS

On macOS the `dsai-m3` conda environment can link two copies of the OpenMP runtime (`libomp.dylib`), which crashes the kernel on `import torch`. Create a file named **`.env`** at the repo root with these four lines:

```
OMP_NUM_THREADS=1
MKL_NUM_THREADS=1
TOKENIZERS_PARALLELISM=false
KMP_DUPLICATE_LIB_OK=TRUE
```

VS Code reads it automatically at kernel startup. **After creating or editing `.env`, restart the Jupyter kernel** (Restart button in the notebook toolbar) — it's only read at startup.

---

## 6. Open the project in VS Code

```bash
code .
```

When VS Code opens, it may prompt you to install recommended extensions (Python, Jupyter). Accept.

---

## 7. Select the `dsai-m3` kernel

1. Open `notebooks/01_monday_morning.ipynb`.
2. In the top-right of the notebook, click **Select Kernel**.
3. Choose **Python Environments → dsai-m3**.

If `dsai-m3` doesn't appear, restart VS Code and try again. If it still doesn't appear, run `conda env list` in a terminal to confirm the environment exists.

---

## 8. Smoke test

In `01_monday_morning.ipynb`, run the first cell (the imports). If it completes without errors, you're set.

If you see `ModuleNotFoundError`, the wrong kernel is selected — go back to Step 7. If the kernel **crashes** on `import torch` on macOS, you skipped Step 5.

---

## Troubleshooting

**`conda: command not found`** → Miniconda isn't installed or isn't on your PATH. Reinstall Miniconda and restart your terminal.

**`environment.yml` solver hangs** → Try `conda env create -f environment.yml --solver=libmamba`. If that still hangs, delete the env (`conda env remove -n dsai-m3`) and retry.

**Kernel doesn't appear in VS Code** → Run `python -m ipykernel install --user --name dsai-m3` from inside the activated environment.

**Kernel dies on `import torch` (macOS)** → Create the `.env` file from Step 5 and restart the kernel.

**Hugging Face download fails** → Check your network, then re-run the cell — downloads resume from the cache.

---

Once setup is done, head back to **[README.md](./README.md)** → Phase 1.
