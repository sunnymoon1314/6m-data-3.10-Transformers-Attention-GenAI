# Lesson — L10 Transformers, Attention & GenAI

> **Chapter 10 of the NorthStar Retail story.** *Sarah Chen · Customer Experience Analyst · Week 11.*
> The L09 catalogue search has shipped — customers can type "blue summer dress" and find one. Friday afternoon, Marcus walks over with one last brief: *"Can a customer ask 'I need a summer outfit for a beach holiday under £200' and get an actual product recommendation, in natural language, from our catalogue?"*
> This is the closer. The answer is a transformer, an LLM, and the pattern that ties them to L09's embeddings — RAG.

This document is a **short reference** — the lesson itself is taught in the notebooks. Read it for orientation before class, then come back to it for the takeaways, the RAG-design checklist, the review questions, and the arc-closing reflection.

---

## How L10 is taught

| Stage | Where to go |
|---|---|
| **Pre-class** | `pre-class.md` + `notebooks/01_monday_morning.ipynb` |
| **In-class — Part 1: Attention intuition** | `notebooks/02_attention_intuition.ipynb` |
| **In-class — Part 2: Using a small LLM** | `notebooks/03_using_an_llm.ipynb` |
| **In-class — Part 3: RAG pipeline** | `notebooks/04_rag_pipeline.ipynb` |
| **Self-study** | `notebooks/assignment.ipynb` + `notebooks/optional_extensions.ipynb` |
| **Reference & review** | This document |

The notebooks are the spine. Run them in order. Come back here for the consolidated takeaways and the review questions.

---

## Overview

Marcus's shopping-assistant brief sits on top of three ideas that build on each other. **Attention** is the inductive bias that finally lets a model treat a sentence as a sentence rather than a stream of disconnected tokens — every token gets to look at every other token and decide who matters. **An LLM** is what you get when you stack that mechanism into a deep autoregressive next-token predictor and pretrain it on internet-scale text — it produces fluent language and general reasoning, but it has never seen NorthStar's catalogue, so asked about it cold it will hallucinate plausible-sounding products that don't exist. **RAG** is the pattern that fixes this: take L09's embedding retrieval, use it to pull the relevant catalogue rows for a query, paste them into the LLM's prompt, and ask the LLM to answer using only what was retrieved. That is the modern GenAI stack, and it is what ships behind Marcus's assistant.

---

## Key takeaways

1. **Attention solves the context problem that RNNs and CNNs cannot.** RNNs cram everything into a single hidden state and forget across long sequences; CNNs only see a local window. Attention lets every token look at every other token directly, in parallel, and weight by relevance — that is why "it" can resolve to "trophy" or "suitcase" depending on the rest of the sentence.
2. **Query, key, value is the whole mechanism.** Each token projects to three vectors: a query (what am I looking for?), a key (what do I offer?), a value (what content do I contribute?). Dot-product the queries against keys, softmax to get weights, take a weighted sum of values. One matmul plus a softmax — and a transformer is just this block, stacked.
3. **Scale changes capability, not architecture.** The same block runs in `all-MiniLM-L6-v2` (6 layers, 22M params), BERT-base (12 layers, 110M), GPT-3 (96 layers, 175B), and the frontier models. Pretraining data and compute pick what the heads learn; the recipe doesn't change.
4. **An LLM is a next-token predictor wrapped in a sampling loop.** Given tokens, it outputs a probability distribution over the vocabulary; greedy or temperature/top-p sampling picks one; you append it and run again. Tokenisation and the chat template are part of the contract — apply the right one or the model produces garbage.
5. **Hallucination is the default behaviour, not a bug.** Ask an LLM about your product catalogue with no context and it will invent confident-sounding product names and prices. The model is doing what it was trained to do — produce plausible text — with no access to ground truth.
6. **RAG grounds the LLM in your data: retrieve, augment, generate.** Embed the catalogue once (L09), embed the query, pull the top-K most similar documents, paste them into the prompt, and instruct the model to recommend only what is in the list. The LLM brings fluency and reasoning; retrieval brings facts and an audit trail.
7. **Prompt design carries weight on small models.** On a 360M model, where you place the catalogue (user turn vs system prompt), how strictly you phrase the "do not invent products" rule, and `repetition_penalty` all materially change output quality. Larger hosted models are more forgiving but not exempt.
8. **Evaluating generative output needs more than accuracy.** A RAG system needs a held-out evaluation set of (query → expected products / acceptable answer) pairs, plus checks for hallucinated product names not in the retrieved set. "Looks good in the demo" is not evidence it works.

---

## Designing a RAG pipeline before you ship it — a checklist

Before you put a RAG system in front of a customer, run it through this three-step check:

1. **Is retrieval pulling the right documents?** The LLM can only recommend what retrieval surfaces. Evaluate retrieval *on its own* first — for a held-out set of realistic queries, do the top-K results actually contain the products a human would pick? If retrieval is weak, no amount of prompt engineering at the generation step will save you.
2. **Is the prompt forcing the model to stay grounded?** The system prompt must say, explicitly, "recommend only from the products listed below; if nothing fits, say so." Add a safety check that flags any product name in the response that is not in the retrieved set. Without grounding rules plus a check, the model will eventually invent.
3. **Do you have an evaluation set, not just a demo?** Write down 20–50 (query → acceptable answer) pairs before you tune anything. Measure on that set when you change the retriever, the K, the prompt, or the model. A change that looks better on one example often breaks five others.

Skip any of these and the assistant will demo beautifully and fail in production — the most expensive way to launch a GenAI feature.

---

## Key concepts — plain-English review

Use this as a self-check before the review questions: read each concept, and if any feels fuzzy, jump back to the notebook or section that teaches it.

**Transformer** — The architecture behind modern AI language models: layers of attention stacked deep. The same recipe powers the tiny embedding model from L09 and the largest chatbots — only the scale changes.
*Real-world use:* The engine inside translation apps, ChatGPT, and the autocomplete in your email.

**Attention** — The mechanism that lets every word in a sentence look at every other word and decide which ones matter for its meaning — so "it" knows whether it refers to "trophy" or "suitcase".
*Real-world use:* Voice assistants resolving "book *it* for Tuesday" by attending back to "the dentist appointment" earlier in the conversation.

**Tokenisation** — Chopping text into the pieces (tokens) the model actually reads — roughly words or fragments. Get the model's expected tokeniser or chat template wrong and it produces garbage.
*Real-world use:* Why AI pricing is "per token" — a legal firm summarising contracts pays by the piece, not the page.

**LLM / text generation** — A large language model is a next-word predictor in a loop: predict a probability for every possible next token, pick one, append it, repeat. Fluent paragraphs are just this loop running fast.
*Real-world use:* Customer-service chatbots at banks and airlines drafting replies one predicted token at a time.

**Prompt** — The full text you hand the model — instructions, context, question. It's the model's *only* window into your world, so wording and placement genuinely change the output.
*Real-world use:* A retailer's assistant behaves completely differently with "recommend only from the list below" versus no rule at all.

**Temperature** — The randomness dial for generation. Low = predictable, safe, repetitive; high = creative but erratic.
*Real-world use:* A hospital discharge-summary tool runs cold (low temperature); a marketing-slogan brainstormer runs warm.

**Hallucination** — The model confidently inventing plausible-sounding facts — the default when it lacks real information, not a malfunction. It was trained to produce *plausible* text, not *true* text.
*Real-world use:* The infamous court filing citing legal cases that never existed — and NorthStar's assistant inventing products until RAG grounded it.

**RAG (retrieval-augmented generation)** — Fix hallucination by fetching the relevant real documents first (using L09's semantic search), pasting them into the prompt, and telling the model to answer only from those.
*Real-world use:* An insurer's policy chatbot that quotes from the customer's actual policy documents instead of guessing.

**Embeddings (recap)** — Meaning-as-numbers vectors from L09, doing the "R" in RAG: they find which catalogue rows or documents are relevant before the LLM ever speaks.
*Real-world use:* An internal HR bot embedding the staff handbook so "how many days off for a new baby?" retrieves the parental-leave page.

---

## Check your understanding

Work through these after finishing the three Part notebooks. Attempt each question on your own first.

### Part 1 — Attention

**Q1 — Why attention beats RNNs.** A teammate proposes building the shopping assistant on a vanilla RNN: "It reads the customer's question one token at a time and produces a recommendation." Give two distinct reasons this is a poor architectural choice for modern NLP.

> **Sample answer:** (1) **Long-range forgetting.** An RNN's hidden state has limited capacity; by the time it has consumed a 30-token question, the early words ("summer", "beach") may be diluted in the state. Attention lets every output position look directly at every input token — no information bottleneck. (2) **No parallelism.** RNNs process tokens sequentially because each step depends on the previous hidden state. Attention computes all token interactions in parallel via matrix multiplications, which is why transformers train on giant data on modern hardware and RNNs effectively don't.

**Q2 — Q/K/V intuition.** In one sentence each, what role do the query, key, and value vectors play in self-attention?

> **Sample answer:** The **query** for a token is what that token is looking for in the rest of the sequence. The **key** for each token is what that token advertises about itself, used to compute how well it matches a query. The **value** is the actual content a token contributes once it is selected — the weighted sum of values across the sequence is the attention output.

### Part 2 — Using an LLM

**Q3 — What an LLM actually does.** A colleague says: "Generative AI is fundamentally different from the classifiers we built in L03–L08." How would you describe what a generative LLM is doing, in one or two sentences, in terms a classifier-builder would recognise?

> **Sample answer:** An autoregressive LLM is a classifier whose classes are "the next token in the vocabulary." Given a sequence of input tokens, it outputs a probability distribution over ~50,000 vocabulary tokens (the classes); you pick one (greedily or by sampling), append it, and run again. The framework is identical to L03's classifier — model produces probabilities, you pick a label — just looped, and with a much larger label space.

**Q4 — Hallucination.** You ask SmolLM2-360M, with no context, "What summer dresses does NorthStar sell under £80?" It returns confident product names and prices that don't exist in the catalogue. Is this a bug? What is the right fix?

> **Sample answer:** Not a bug — the model is doing what it was trained to do, which is produce plausible-sounding text. It has never seen NorthStar's catalogue, so there is no ground truth for it to reach for; it fills the gap with a plausible average of what "a product list" looks like. The fix is not to ask the model to "try harder" or to fine-tune; the fix is to give it the catalogue at inference time. That is the role of retrieval-augmented generation.

### Part 3 — RAG

**Q5 — Why RAG beats raw prompting.** Marcus asks: "If GPT-4 is so smart, why don't we just paste our whole catalogue into the prompt every time?" Give two reasons RAG is the right architecture even with a large hosted model.

> **Sample answer:** (1) **Cost and latency.** Pasting 76 products (let alone a real catalogue of 10,000) into every prompt is wasteful — you pay per token and add latency for content the model doesn't need for this query. RAG sends only the top-5 relevant rows. (2) **Quality.** LLMs degrade on very long contexts — they "lose the needle in the haystack." A focused 5-product prompt with explicit grounding instructions produces more reliable recommendations than 10,000 products dumped in. RAG separates "which products are relevant?" (cheap embedding search) from "phrase a recommendation" (LLM) and lets each component do what it's good at.

**Q6 — Where the catalogue goes in the prompt.** Notebook 04 puts the retrieved products in the *user* turn rather than the system prompt, with the instruction "recommend only from these." Why does this matter for a small model?

> **Sample answer:** A 360M-parameter model can drift on long system prompts that contain structured data — it tends to treat them as text to continue rather than rules to follow. Framing the catalogue as "here is what we have, answer my question using only these" in the user turn keeps the grounding constraint and the data adjacent, and matches the chat-template patterns the model was instruction-tuned on. Larger hosted models are more forgiving, but the principle holds: prompt structure is part of the design, not an afterthought.

**Q7 — Evaluating generative output.** Sarah's first RAG demo answers her own test query perfectly. She wants to ship it. What do you tell her she still needs to do before launch?

> **Sample answer:** A single demo query is not evidence the system works. Sarah needs (a) a held-out evaluation set of 20–50 realistic (query → acceptable products / answer) pairs that she did not see while building, (b) a measurement at both stages — retrieval precision (does the top-K contain the right products?) and generation quality (does the answer recommend products that are in the retrieved set, and only those?), and (c) a safety check that flags responses mentioning product names not in the retrieved set. Only then is she comparing the RAG assistant against the L09 search baseline on equal footing.

**Q8 — When NOT to reach for an LLM.** Marcus also asks whether the customer-review sentiment task from L01 should be "upgraded" to use an LLM now that Sarah knows how. What's your recommendation?

> **Sample answer:** Probably no. L01's pretrained sentiment pipeline is fast, cheap, deterministic, and good enough for the task. An LLM would add latency, cost, non-determinism, and a hallucination surface for zero accuracy gain. Reach for an LLM when the task genuinely needs language generation or open-ended reasoning the smaller model can't do (the shopping assistant qualifies); stick with the smaller specialised model when classification is all you need.

---

## Where L10 fits — and where you go next

L10 closes the module. Across ten lessons Sarah has built the toolkit a working ML practitioner actually uses: honest evaluation and confidence intervals (L02), supervised pipelines and threshold choice (L03–L04), unsupervised methods and time series (L05–L06), neural networks and computer vision (L07–L08), semantic search (L09), and now the transformer architecture that underlies all of modern NLP plus the RAG pattern that ties retrieval to generation. None of these is a final answer — every one is a starting point. Where the toolkit takes you next is your call: a hackathon project where you ship Sarah's assistant end-to-end on your own data; a real problem at work where you finally have the vocabulary to scope it; a deeper dive into MLOps, fine-tuning, evaluation harnesses, or agentic systems. The foundations are now under you. The interesting work starts on the other side of this lesson.

---

> *"Nine months ago, we asked you to read 10,000 reviews. Now we've got a churn model, a forecast, a photo tagger, a search bar that understands 'blue summer dress', and an assistant that recommends from the catalogue without making things up. I don't fully understand how it all works — but you do. That's the difference."* — Marcus, after the shopping-assistant launch.
>
> Sarah's chapter at NorthStar closes here. Yours starts now — go build something.
