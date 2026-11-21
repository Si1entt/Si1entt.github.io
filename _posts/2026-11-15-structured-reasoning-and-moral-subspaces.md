---
layout: blog_post
published: false
title: "From Judicial Reasoning to Moral Subspaces: Toward Structured and Intrinsic Alignment in LLMs"
date: 2026-11-15 14:00:00 +0800
tags:
  - LLM Alignment
  - Mechanistic Interpretability
  - Legal AI
  - LLM Agents
excerpt: >-
  A research reflection connecting our recent studies on structured civil judicial reasoning (FocalJudge, ACL 2026), mechanistic moral probing and activation steering (AMF, EMNLP 2026), and evolutionary scientific expertise (KnowEvo, NeurIPS 2026) to upcoming Ph.D. directions in cognitive and social LLM agents at Fudan DISC Lab.
---

Over the past two years at the Information Retrieval Lab of Dalian University of Technology (DUTIR), my research has revolved around a recurring question: **when large language models appear fluent on the surface, why do they still falter in settings that require rigorous normative reasoning, calibrated value boundaries, or sustained domain expertise?**

Looking back at three seemingly distinct lines of work—civil judicial reasoning ([**ACL 2026**](https://aclanthology.org/2026.acl-long.1410/)), mechanistic moral alignment ([**EMNLP 2026**](https://aclanthology.org/2026.emnlp-main.1112/)), and evolutionary knowledge agents for protein optimization ([**NeurIPS 2026**](https://arxiv.org/abs/2611.09013))—they turn out to examine three complementary layers of the same underlying problem: **cognitive workflow structure**, **internal latent geometry**, and **externalized evolving memory**. This post summarizes the key lessons from these works and outlines how they motivate my upcoming Direct-Ph.D. research at [**Fudan DISC Lab**](http://www.fudan-disc.com/) starting in Fall 2027.

## 1. The "Clerk Trap" and Structured Cognitive Workflows

In real-world civil litigation, judges do not leap directly from messy pleadings and trial transcripts to a final verdict. Instead, they crystallize the **dispute focus** (*争议焦点*)—the core factual and legal points of contention jointly established by the court and the litigants—as an anchoring pivot for fact-finding and statutory application.

To study whether LLMs can leverage this judicial pivot, we constructed **FocalLaw** ([*To Judge or Not to Judge*, ACL 2026](https://aclanthology.org/2026.acl-long.1410/)), the first full-process Chinese civil judicial dataset centered on dispute focuses, curating 1,000 complex cases across six causes of action from over 730,000 paired judgment documents and court trial videos.

### Why Naive Prompting and Fine-Tuning Backfire

Intuitively, explicitly supplying the ground-truth dispute focus alongside pre-judgment materials (pleadings, evidence records, and trial debates) should make legal judgment prediction and reasoning easier. Surprisingly, across both frontier closed-source models and open-weight/legal-domain LLMs, **directly providing the dispute focus caused a counter-intuitive performance drop** in fact generation, judicial reasoning, and outcome prediction. Standard Chain-of-Thought (CoT) prompting amplified the degradation, and supervised fine-tuning (SFT) struggled to generalize across heterogeneous civil disputes.

We term this failure mode the **"Clerk Trap"**:

> Rather than acting like a *judge* who weighs conflicting evidence to adjudicate disputed claims, an unconstrained LLM acts like a *passive clerk*—conflating **claim extraction** (summarizing what each party asserts around the dispute focus) with **fact adjudication** (determining which assertions are substantiated by admissible evidence).

### FocalJudge: Separating Fact Determination from Statutory Adjudication

Resolving the Clerk Trap required aligning the model's inference trajectory with judicial procedure rather than adding more surface tokens. We designed **FocalJudge**, a two-stage structured cognitive workflow:

1. **Dispute-Guided Fact Determination**: For each dispute focus $f\_k \in \mathcal{F}$, the model first separates undisputed facts from conflicting party claims, weighs evidentiary support point by point, and synthesizes a unified factual finding.
2. **Dispute-Guided Legal Adjudication**: Conditioned on adjudicated facts rather than raw party allegations, the model performs point-by-point statutory matching before synthesizing the final reasoning and verdict.

Across diverse model scales, replacing unstructured CoT with FocalJudge reversed the performance drop and yielded **5%–10% consistent gains** across fact determination, reasoning quality, and legal judgment prediction. The takeaway was clear: *intermediate domain abstractions only help when paired with an explicit cognitive structure that governs how evidence is adjudicated*.

## 2. Probing the Internal Moral Path of LLMs

While structured workflows discipline how a model reasons over external context, safety and normative alignment face an equally fundamental question *inside* the network: **when post-trained LLMs refuse harmful requests—or over-refuse benign ones—do they rely on brittle surface heuristics, or do they possess structured internal moral representations?**

In [*The Straight and Narrow: Do LLMs Possess an Internal Moral Path?* (EMNLP 2026)](https://aclanthology.org/2026.emnlp-main.1112/) ([arXiv](https://arxiv.org/abs/2601.10307), [Code](https://github.com/Si1entt/Moral)), we grounded mechanistic interpretability in social psychology's **Moral Foundations Theory (MFT)**, spanning five foundational dimensions—*Care*, *Fairness*, *Loyalty*, *Authority*, and *Sanctity*—each polarized into *Virtue* and *Vice*.

### Linearly Separable and Cross-Lingually Nuanced Subspaces

By training layer-wise linear probes across English and Chinese corpora, we uncovered two striking mechanistic patterns:

- **Middle-Layer Crystallization**: Linear separability across all moral foundations rises sharply from shallow token embeddings, peaks consistently in the **middle transformer layers**, and gradually tapers toward final decoding layers—indicating that abstract normative concepts are organized most cleanly in intermediate semantic representations.
- **Shared Yet Culturally Differentiated Geometry**: Cross-lingual probe transfer between English and Chinese revealed neither strictly identical universality nor disjoint language silos, but a **shared core moral subspace modulated by culturally specific patterns** (manifesting subset, intersection, and decoupled regimes across different moral foundations).

### Adaptive Moral Fusion (AMF)

Using contrastive sentence pairs, we extracted steerable **Moral Vectors** and verified their causal influence both internally (mapping inter-foundation correlations and decoding latent semantics) and behaviorally. Building on this geometry, we proposed **Adaptive Moral Fusion (AMF)**—a lightweight, training-free activation intervention that dynamically gates moral steering at inference time:

<div class="my-3 text-center">
$$\tilde{\mathbf{h}} = \mathbf{h} + \sum_{m \in \mathcal{M}} \mathbb{I}\!\left[p_m(\mathbf{x}) > \tau\right] \cdot \mathbf{v}_m$$
</div>

where $p\_m(\mathbf{x})$ denotes the probe activation score for moral dimension $m$ and $\mathbf{v}\_m$ is the corresponding steering vector injected into intermediate hidden states $\mathbf{h}$. Because AMF intervenes *only* along dimensions triggered by the input's latent moral profile, it avoids the bluntness of static safety prompts or global activation shifts. Empirically, AMF enabled `Llama-3.1-8B-Instruct` to simultaneously suppress jailbreak attack success rates on **HarmBench** and slash false refusals on **XSTest** to levels comparable with frontier closed-source models (`Claude-3.5-Sonnet`), while preserving general reasoning benchmarks.

## 3. Externalized Knowledge Evolution in Scientific Discovery

Complementing structured workflows and latent steering, our collaborative work on scientific LLM agents—from the [ICLR 2026 RSI Workshop](https://openreview.net/forum?id=NeNWeyfUJj) to [**KnowEvo** (NeurIPS 2026)](https://arxiv.org/abs/2611.09013) ([Code](https://github.com/recynie/KnowEvo))—explored how LLMs adapt in data-scarce experimental domains such as protein stability and solubility optimization.

Instead of updating billions of opaque weights on sparse wet-lab feedback, **KnowEvo** externalizes domain know-how into modular, natural-language **expertise blocks**. Through strategy mutation trees, forward multi-agent task allocation, and backward credit-assignment revision, the agent population evolves interpretable scientific hypotheses over iterative design rounds—achieving faster convergence and higher-fitness protein variants than static prompting or black-box optimization baselines.

## 4. Looking Ahead: Cognitive & Social Agents at Fudan DISC Lab

Stepping back, these three projects point toward a unified research agenda that I am excited to pursue during my Direct-Ph.D. studies at [**Fudan DISC Lab**](http://www.fudan-disc.com/), School of Data Science, Fudan University, under the supervision of [**Prof. Zhongyu Wei**](http://www.fudan-disc.com/people/zywei):

1. **Mechanistic Foundations of Post-Training & Persona Alignment**: Extending representation probing, Sparse Autoencoders (SAEs), and causal steering from static moral dimensions to **dynamic social roles and personas**—understanding how post-training internalizes role cognition rather than merely imitating surface dialogue phrasing, and resolving the tension between *persona stability* and *collaborative adaptability*.
2. **Normative & Epistemic Grounding in Interactive Environments**: Investigating how multi-modal, social, and institutional feedback anchors LLM representations beyond purely self-referential symbol statistics, enabling calibrated knowledge boundaries ("knowing what the model does not know").
3. **Socially Situated Cognitive Agents**: Integrating structured adjudicative/deliberative workflows (as in *FocalJudge* and legal mediation research) with intrinsic value/persona modules to build trustworthy agents for human-AI collaboration and computational social simulation.

If you share interests in LLM post-training, mechanistic interpretability, cognitive/social agents, or AI for law and social science, please feel free to reach out!
