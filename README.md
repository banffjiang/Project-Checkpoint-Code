# Project-Checkpoint-Code

This project documents my Quarter 1 work for the BabyLM Challenge, where I explored low-resource language model training and curriculum design using the CLiMB and GPT-BERT frameworks. The goal was to deeply understand how model behavior changes under small-data constraints, and to build reliable infrastructure for large-scale yet efficient experimentation on Kubernetes (NRP Nautilus).

**Getting Started on NRP Nautilus**

I began by setting up my development and compute environment on NRP Nautilus, a Kubernetes-based research platform. I learned how to launch GPU pods, request persistent volumes for long-term storage, and mount these volumes to train models reproducibly. Understanding how Nautilus manages containers and resources was essential for running multi-hour experiments and saving checkpoints across pods. This process taught me how distributed infrastructure supports reproducible ML research, especially when experimenting with multiple model versions.

**Reading the Literature**

Next, I read several notable papers and challenge descriptions to understand the motivation behind the BabyLM competition. The challenge focuses on building language models trained on extremely small datasets, emphasizing data efficiency and linguistic generalization rather than scale.
I learned about the two official tracks — the Small track (up to around 10 million tokens) and the Strict track (curated and smaller in scope). I also reviewed the BLiMP benchmark and minimal-pair testing, which evaluate whether models capture fine-grained grammatical knowledge rather than just surface-level correlations.

These readings helped me understand why BabyLM is a meaningful scientific task: it’s not just about achieving low perplexity, but about learning language structure with limited exposure — much like how human learners generalize from small samples.

Implementing Curriculum Learning (CLiMB)

My first experiments focused on curriculum learning, inspired by the CLiMB framework. I designed a data curriculum that introduced examples in increasing order of difficulty — starting with short, frequent, and syntactically simple sentences, and gradually progressing to longer or more complex structures.

Alongside this, I experimented with a vocabulary curriculum, controlling when new subwords were introduced during training. This allowed me to observe whether gradually exposing the model to new vocabulary would lead to more stable learning and better sample efficiency.

These curricula aimed to simulate how humans acquire language — moving from common, simple words to more complex expressions — while still respecting the small token budgets of the BabyLM tracks.

**Modeling and Training**

After setting up the data, I worked with a GPT-BERT hybrid model, which combines aspects of bidirectional and autoregressive transformers. I tuned parameters such as depth, hidden dimension, and sequence length to fit within GPU limits on Nautilus.
During training, I used wandb (Weights & Biases) to log runs, visualize learning curves, and manage hyperparameter sweeps. I experimented with learning rates, warm-up schedules, and batch sizes to optimize validation perplexity under a fixed token budget.

This process gave me a strong understanding of how optimization choices affect generalization when data is scarce, and how even subtle changes in training dynamics can shift performance on linguistic evaluations.

**Evaluation: Perplexity and BLiMP**

Once the models were trained, I evaluated them using perplexity on held-out validation data to measure predictive accuracy. Then I turned to BLiMP and minimal-pair evaluations, which test whether models prefer grammatically correct sentences over incorrect ones across multiple linguistic phenomena (e.g., subject-verb agreement, anaphora, binding, morphology).

The CLiMB-based curricula consistently improved perplexity and boosted BLiMP accuracy in certain syntactic categories, though results varied across phenomena. For instance, models learned sentence-level agreement and anaphora patterns better, but gains in morphological reasoning were less consistent.

These analyses taught me that evaluating small models requires more than one metric — perplexity alone doesn’t capture deeper linguistic competence.

**Insights and Takeaways**

Through this process, I learned the value of rigorously explaining and emulating finished work. Writing down each experiment forced me to clarify every assumption — how data was sampled, how token budgets were enforced, and how evaluation metrics were computed.
By comparing my methods against established baselines, I developed a better sense of what “good” empirical reasoning looks like in NLP research. I also identified limitations — for instance, some curricula improved early learning but not long-term generalization, suggesting that curriculum design needs linguistic awareness, not just sentence length heuristics.

Looking Ahead: Quarter 2 Goals

For the next phase, I plan to:

Develop a phenomenon-aware curriculum, where training data is categorized by linguistic feature (e.g., agreement, quantifiers, binding) to target BLiMP weaknesses directly.

Explore alternative tokenization schemes (unigram vs BPE vs WordPiece) to study how subword segmentation affects learning in small-data regimes.

Incorporate hybrid training objectives that blend masked-language modeling with causal prediction to improve contextual reasoning.

Expand evaluation to include probing tasks and confidence calibration, ensuring models not only predict well but also know when they’re uncertain.