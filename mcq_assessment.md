# L10 — Transformers, Attention & GenAI: MCQ Assessment

Assess your understanding of the key concepts. Each question has **one** correct answer. Answers and explanations are at the bottom.

---

**Q1. What core problem does attention solve that RNNs and CNNs cannot?**

- A. It compresses an entire sentence into a single fixed-size hidden state
- B. It lets every token look directly at every other token and weight them by relevance
- C. It only examines a small local window of neighbouring tokens
- D. It removes the need for any training data

---

**Q2. In self-attention, what is the role of the *query* vector for a token?**

- A. What the token advertises about itself so others can match against it
- B. The actual content the token contributes to the output
- C. What that token is looking for in the rest of the sequence
- D. The final probability distribution over the vocabulary

---

**Q3. Which sequence correctly describes the self-attention computation?**

- A. Softmax the values, then dot-product against queries, then sum keys
- B. Dot-product queries against keys, softmax to get weights, take a weighted sum of values
- C. Average all token embeddings, then apply a single linear layer
- D. Concatenate keys and values, then run them through an RNN

---

**Q4. According to the lesson, what changes as you scale from `all-MiniLM-L6-v2` (22M params) to GPT-3 (175B params)?**

- A. The fundamental architecture is replaced with a new mechanism
- B. The attention block is abandoned in favour of CNNs
- C. The capability changes, but the underlying block (the recipe) stays the same
- D. Softmax is replaced by a different normalisation at large scale

---

**Q5. An autoregressive LLM is best described, in classifier terms, as:**

- A. A clustering model that groups tokens by similarity
- B. A regression model predicting a continuous score per sentence
- C. A classifier whose classes are "the next token in the vocabulary," run in a loop
- D. A rule-based system that looks up answers in a database

---

**Q6. You ask a small LLM about your product catalogue with no context, and it returns confident product names and prices that don't exist. This is:**

- A. A bug in the model's weights that fine-tuning the loss function will fix
- B. Expected behaviour — the model produces plausible text with no access to ground truth
- C. Proof the model is broken and should be discarded
- D. A tokenisation error caused by the wrong chat template

---

**Q7. What is the correct one-line summary of the RAG pattern?**

- A. Fine-tune the model on every new document before answering
- B. Retrieve relevant documents, augment the prompt with them, then generate the answer
- C. Generate an answer first, then retrieve documents to check it
- D. Replace the LLM entirely with an embedding search

---

**Q8. Marcus asks: "Why not just paste the whole catalogue into the prompt every time?" Which is a valid reason RAG is better even with a large hosted model?**

- A. Large models cannot read structured data at all
- B. Pasting everything costs more tokens/latency, and very long contexts degrade quality
- C. RAG removes the need for any LLM in the pipeline
- D. Embeddings are only useful for images, not text

---

**Q9. Notebook 04 places the retrieved products in the *user* turn with "recommend only from these." Why does this matter for a small (360M) model?**

- A. Small models cannot process system prompts at all
- B. It hides the data from the user for privacy reasons
- C. Small models drift on long system prompts; keeping the data + constraint in the user turn matches their instruction-tuning
- D. The user turn allows unlimited context length

---

**Q10. Sarah's first RAG demo answers her own test query perfectly and she wants to ship. What must she still do?**

- A. Nothing — a successful demo is sufficient evidence to launch
- B. Re-train the LLM from scratch on the catalogue
- C. Build a held-out evaluation set, measure retrieval *and* generation, and add a hallucination safety check
- D. Switch from RAG back to raw prompting to simplify the system

---

## Answer Key

| Q | Answer | Why |
|---|--------|-----|
| 1 | **B** | Attention lets every token attend to every other token directly, in parallel, weighted by relevance — solving RNN long-range forgetting and CNN's local-window limit. |
| 2 | **C** | Query = what the token is looking for. (Key = what it advertises; Value = the content it contributes.) |
| 3 | **B** | Dot-product Q·K → softmax → weighted sum of V. One matmul plus a softmax. |
| 4 | **C** | Scale changes capability, not architecture — the same attention block is stacked; data and compute decide what heads learn. |
| 5 | **C** | An LLM is a next-token classifier over the vocabulary, wrapped in a sampling loop — same framework as earlier classifiers, larger label space. |
| 6 | **B** | Hallucination is the default behaviour: the model produces plausible text with no ground truth. The fix is to supply the data at inference (RAG), not to fine-tune. |
| 7 | **B** | RAG = retrieve, augment, generate. |
| 8 | **B** | Cost/latency per token plus quality degradation on very long contexts ("needle in a haystack"); RAG sends only the top-K relevant rows. |
| 9 | **C** | Small models drift on long structured system prompts; framing data + grounding rule in the user turn matches instruction-tuning patterns. |
| 10 | **C** | A demo isn't evidence — she needs a 20–50 pair held-out eval set, measurement at both retrieval and generation stages, and a check flagging product names not in the retrieved set. |
