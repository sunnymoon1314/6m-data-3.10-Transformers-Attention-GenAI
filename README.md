# L10 — Transformers, Attention & GenAI

> **Module 3 · Machine Learning & GenAI · Lesson 10 of 10 — THE FINAL LESSON**

## The story so far

Sarah has been on a nine-week journey at NorthStar Retail:

- **L01-L03** — built her first models, learnt to evaluate them honestly, learnt to make them work in production
- **L04-L06** — gradient boosting, unsupervised methods, time series
- **L07** — neural networks, the PyTorch training loop
- **L08** — convolutions, transfer learning, computer vision
- **L09** — embeddings and semantic search

After L09 the catalogue search ships. Customers are happier. Marcus is happy. But on Friday afternoon he asks one more question — the question that's been sitting in everyone's inbox since ChatGPT launched:

> *"Can we build a **shopping assistant**? Customer asks 'I need a summer outfit for a beach holiday under £200' — it understands the question AND recommends actual products from our catalogue."*

Sarah's last lesson is about what makes that possible.

## What you'll learn

This is the **closer** of the module. By the end of L10 you will be able to:

1. **Explain attention** — the mechanism that lets a model look at every other word in the sentence when processing each word
2. **Walk through a transformer block** — attention + feed-forward + residuals + layer-norm. The Lego brick of modern NLP
3. **Use a small instruction-tuned LLM** end-to-end — tokenisation, chat templating, sampling, generation
4. **Build a RAG pipeline** — retrieval-augmented generation: combine L09's embeddings with an LLM to answer questions about your data
5. **Know when to use what** — prompt the API, fine-tune your own, build a RAG system, or just write a regex

This is the lesson that connects classical ML to GenAI. By Friday you'll understand the basics of the architecture behind ChatGPT, Claude, Gemini, and Llama.

## Phase 1 — Pre-class (≈ 30 min)

- Watch the [lesson intro video](https://youtu.be/dJ2zUsszac4) (~5 min)
- Read [pre-class.md](pre-class.md)
- Watch the linked 3Blue1Brown attention video
- Explore the [**interactive key-concepts page**](https://su-ntu-ctp.github.io/6m-data-3.10-Transformers-Attention-GenAI/) after the videos (GitHub Pages)
- Run [notebooks/01_monday_morning.ipynb](notebooks/01_monday_morning.ipynb) — feel the power of pretrained pipelines

## Phase 2 — In-class (≈ 45–60 min slide recap + 90 min code-along + 15 min exit survey)

- Concept recap with slides (~45–60 min): instructor recaps the key concepts — you already explored the [interactive key-concepts walkthrough](https://su-ntu-ctp.github.io/6m-data-3.10-Transformers-Attention-GenAI/) pre-class (revisit any time)
- Short reference & review → [**lesson.md**](./lesson.md) (overview, key takeaways, RAG-design checklist, 8-question review, course-closing reflection)
- Code-along notebooks (in order):
  - [02_attention_intuition.ipynb](notebooks/02_attention_intuition.ipynb) — attention by hand
  - [03_using_an_llm.ipynb](notebooks/03_using_an_llm.ipynb) — tokenisation + generation
  - [04_rag_pipeline.ipynb](notebooks/04_rag_pipeline.ipynb) — retrieval-augmented generation

> **Don't have a GPU?** L10 runs on CPU but SmolLM2 generation is slow (~5-15 tokens/sec). On a T4 GPU in Colab it's near-instant. Each notebook has an **Open in Colab** badge at the top — click it, then **Runtime → Change runtime type → T4 GPU**:

- Class exit survey (~15 min): quick feedback to close the session

## Phase 3 — Post-class (self-study, optional)

- [assignment.ipynb](notebooks/assignment.ipynb) — Build a RAG-powered shopping assistant for NorthStar
- [optional_extensions.ipynb](notebooks/optional_extensions.ipynb) — prompt engineering, sampling parameters, when to fine-tune

## Files

| Path | Purpose |
|------|---------|
| `README.md`              | This file — orientation |
| `setup.md`               | Environment install + first-time model downloads |
| `pre-class.md`           | ~30-min self-study before class |
| `lesson.md`              | Short reference: overview, takeaways, RAG-design checklist, review Q&A, course-closing reflection |
| `reference.md`           | Glossary of 25 transformer/GenAI terms |
| `environment.yml`        | Conda env spec |
| `docs/index.html`        | Interactive key-concepts page — explore during pre-class (GitHub Pages) |
| `mcq_assessment.md`      | Multiple-choice self-check quiz |
| `notebooks/`             | 4 in-class NBs + assignment + extensions + data |

## Module 3 close

This is the last lesson. After L10 you'll have built — across nine weeks — a complete toolkit:

- Classical ML (LR, trees, boosting, clustering, time series)
- Deep learning (MLPs, CNNs, NLP transformers)
- Production patterns (transfer learning, RAG, hybrid retrieval, human-in-the-loop)

You can ship real ML systems. The next steps depend on you — domain specialisation, fine-tuning, MLOps, evaluation. The foundations are all here.

Welcome to the last week.
