# Fact sheet: Attention Is All You Need (arXiv 1706.03762)

Verified against **arXiv 1706.03762 v7** (last revised 2 Aug 2023), which is the
current version on arXiv at the time of checking. Every quote and figure below
comes from the paper's own text — the abstract, the Model Architecture section,
the Training section, and the Results section (plus Tables 2 and 3).

---

## Claim 1 — Attention only, no recurrence or convolutions

**Claim as drafted:** The Transformer is based solely on attention mechanisms and dispenses with recurrence and convolutions entirely.

**Verdict: CONFIRMED**

**Evidence (Abstract, verbatim):**
> "We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely."

Supporting context from the Model Architecture and Positional Encoding sections: "The Transformer follows this overall architecture using stacked self-attention and point-wise, fully connected layers for both the encoder and decoder", and "Since our model contains no recurrence and no convolution ... we add 'positional encodings' to the input embeddings".

---

## Claim 2 — 28.4 BLEU on WMT 2014 English-to-German

**Claim as drafted:** It achieves 28.4 BLEU on the WMT 2014 English-to-German translation task.

**Verdict: CONFIRMED**

**Evidence (Abstract, verbatim):**
> "Our model achieves 28.4 BLEU on the WMT 2014 English-to-German translation task, improving over the existing best results, including ensembles, by over 2 BLEU."

Also in the Results section: "the big transformer model ... outperforms the best previously reported models (including ensembles) by more than 2.0 BLEU, establishing a new state-of-the-art BLEU score of 28.4. ... Training took 3.5 days on 8 P100 GPUs." Table 2 lists Transformer (big) at **28.4** EN-DE.

**Caveat for the slides:** 28.4 is the **big** model. The base model scores 27.3 EN-DE (Table 2).

---

## Claim 3 — EN-FR single-model state of the art

**Claim as drafted:** It reports 41.0 BLEU on the WMT 2014 English-to-French task as a new single-model state-of-the-art.

**Verdict: CORRECTED — the headline figure is 41.8, not 41.0**

**Evidence (Abstract, verbatim):**
> "On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.8 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature."

Table 2 agrees with the abstract: Transformer (big) **41.8** EN-FR, ahead of ConvS2S Ensemble (41.29) and GNMT + RL Ensemble (41.16).

**Worth knowing for Q&A:** the paper is internally inconsistent on this number. The Results body text says "On the WMT 2014 English-to-French translation task, our big model achieves a BLEU score of 41.0, outperforming all of the previously published single models" — leftover wording from **v1 of the paper, whose abstract said 41.0**. In the current v7, the abstract and Table 2 both say 41.8, and they match each other, so present **41.8** and keep 41.0 as a footnote if anyone asks.

---

## Claim 4 — EN-FR result trained 3.5 days on eight GPUs

**Claim as drafted:** That English-to-French result came from training the big model for 3.5 days on eight GPUs.

**Verdict: CONFIRMED**

**Evidence (Abstract, verbatim):**
> "... a new single-model state-of-the-art BLEU score of 41.8 after training for 3.5 days on eight GPUs ..."

**Evidence (Training → Hardware and Schedule, verbatim):**
> "We trained our models on one machine with 8 NVIDIA P100 GPUs."

Table 2 puts the training cost of Transformer (big) at 2.3·10^19 FLOPs, described in the Results section as "a small fraction of the training costs of the best models from the literature".

---

## Claim 5 — Base model layer count

**Claim as drafted:** The base model stacks 8 layers in each of the encoder and the decoder.

**Verdict: CORRECTED — it is 6 layers, not 8**

**Evidence (Model Architecture → Encoder and Decoder Stacks, verbatim):**
> "The encoder is composed of a stack of $N=6$ identical layers."
>
> "The decoder is also composed of a stack of $N=6$ identical layers."

Table 3 confirms $N=6$ for both the base model **and** the big model.

**Likely source of the confusion:** 8 is the number of **attention heads** in the base model ($h=8$; the big model uses $h=16$), not the number of layers. Also in each decoder layer there are **three** sub-layers (masked self-attention, encoder-decoder attention, feed-forward) versus two in each encoder layer.

---

## Claim 6 — Training schedules

**Claim as drafted:** The base model was trained for 100,000 steps (12 hours) and the big model for 300,000 steps (3.5 days).

**Verdict: CONFIRMED**

**Evidence (Training → Hardware and Schedule, verbatim):**
> "For our base models using the hyperparameters described throughout the paper, each training step took about 0.4 seconds. We trained the base models for a total of 100,000 steps or 12 hours. For our big models ... step time was 1.0 seconds. The big models were trained for 300,000 steps (3.5 days)."

Table 3's "train steps" column agrees: base = 100K, big = 300K.

---

## Summary table

| # | Claim | Verdict | Correct figure |
|---|-------|---------|----------------|
| 1 | Attention only, no recurrence/convolutions | Confirmed | — |
| 2 | 28.4 BLEU on WMT 2014 EN-DE | Confirmed | 28.4 (big model) |
| 3 | 41.0 BLEU on WMT 2014 EN-FR | **Corrected** | **41.8** |
| 4 | EN-FR from 3.5 days on eight GPUs | Confirmed | — |
| 5 | Base model has 8 layers each side | **Corrected** | **6 layers** |
| 6 | Base 100K steps / 12 h; big 300K steps / 3.5 days | Confirmed | — |

## Further reading — papers that build on the Transformer

1. **BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding** — Devlin, Chang, Lee, Toutanova (arXiv [1810.04805](https://arxiv.org/abs/1810.04805)). Encoder-only Transformer pre-training; bidirectional context via masked language modelling.
2. **An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale** — Dosovitskiy et al. (arXiv [2010.11929](https://arxiv.org/abs/2010.11929)). The Vision Transformer (ViT): shows a pure Transformer beats CNNs on image recognition at scale.
3. **Language Models are Few-Shot Learners** — Brown et al. (arXiv [2005.14165](https://arxiv.org/abs/2005.14165)). GPT-3: a 175-billion-parameter decoder-only Transformer trained for in-context few-shot learning.
