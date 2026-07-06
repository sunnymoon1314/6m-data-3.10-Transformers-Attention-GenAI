# Before Class — L10 — Transformers & GenAI

**Estimated time: ~30 minutes.** Complete this before class.

This is the simplest version of "show up prepared": watch a short intro, run one notebook, answer a few questions. You'll come to class having seen the idea in action.

| Step | Time | What you do |
|---|---|---|
| **0. Watch** | ~5 min  | Watch the [lesson intro video](https://youtu.be/dJ2zUsszac4) |
| **1. Try it** | ~20 min | Open and run `notebooks/01_monday_morning.ipynb` |
| **2. Reflect** | ~5 min  | Three short questions below |

---

## Step 0 — Watch the intro video (~5 min)

▶️ **[L10 Intro — Transformers, Attention & GenAI](https://youtu.be/dJ2zUsszac4)**

A short orientation to the final week: attention, transformer blocks, and the RAG shopping assistant Sarah builds to close the module. Watch it before opening the notebook.

🕹️ **After the video:** open the [interactive key-concepts page](https://su-ntu-ctp.github.io/6m-data-3.10-Transformers-Attention-GenAI/) and play with it for 10–15 minutes. Drag the sliders, click the buttons — you can't break anything. Arriving in class having *seen* these ideas move makes the session far easier.

---

## Step 1 — Try it (~20 min)

Open **`notebooks/01_monday_morning.ipynb`** in VS Code with the `dsai-m3` kernel. Run every cell top to bottom. Read the markdown between cells. Don't skip any cell.

Marcus's final ask: *"Can we build a shopping assistant? Customer asks 'I need a summer outfit for a beach holiday under £200' — it understands AND recommends actual products."* The notebook explores what pretrained transformers can already do in one line of Python: sentiment, NER, zero-shot classification. You walk into class with a sense of what's possible — and you'll learn how it works.

If this is your first time running a notebook in this repo, see [setup.md](./setup.md) once — you only need to do this for the first lesson.

---

## Step 2 — Quick reflection (~5 min)

Write a sentence or two for each. You can scribble in a notebook, in a journal, or just hold the answer in your head — what matters is that you *tried*.

**Q1. What was the sentiment model actually trained to predict?**

Take a guess. What would its training data have looked like? (Hint: the model is named `distilbert-base-uncased-finetuned-sst-2-english`.)

**Q2. ChatGPT, Claude, and `all-MiniLM-L6-v2` are all transformer-based.**

What's different about them — the architecture, the size, the training data, the objective, or all four?

**Q3. If you had to add ONE GenAI feature to a real product you use, what would it be?**

Write one sentence. We'll see whether a transformer is the right tool for it.

---

## Bring to class

- Your answer to Q3 — your one GenAI feature idea.
- One specific question about how attention or LLMs work that you want answered in class.

---

**Want to go deeper before class?** See **[reference.md](./reference.md) → *Further reading & watching*** at the bottom — videos and recommended readings that used to live in this guide.

**Ran out of time?** Doing just Step 1 (running the notebook) is enough. The class builds on having felt what the lesson teaches; the reflection questions get re-asked live.
