### Q: Choose among prompting, RAG, continued pretraining, SFT, PEFT, and full tuning from failure evidence.
* **Difficulty:** Principal
* **Category:** Strategy
* **The 10-Second Pitch:** Use prompting for fast behavior specification, RAG for current/auditable external knowledge, continued pretraining for broad domain language adaptation, SFT for repeatable input-output behavior, and full fine-tuning only when adapter capacity or frozen weights are insufficient. Choose from error evidence, not fashion.
* **The Deep Dive:** Prompting cannot reliably internalize a broad specialized style under tight token budgets. RAG does not teach a model a new output protocol. Continued pretraining changes base representations but can forget general skills. SFT learns demonstrations; full tuning provides maximum capacity but costs more compute, governance, and rollback complexity.
Classify the defect before choosing an intervention. Missing fresh facts imply retrieval; unstable formatting or policy behavior implies SFT/PEFT; weak domain representations across many tasks may justify continued pretraining. Full tuning is justified only after an adapter-capacity sweep. Evaluate each rung against one frozen target, retention, safety, latency, and cost suite so the intervention—not a changed benchmark—explains the gain.

* **Production Reality & Tradeoffs:** Start with the least irreversible intervention and establish an evaluation suite before training. Volatile facts normally belong in retrieval, not weights.
* **Red Flag:** Fine-tuning to make a model know today’s policy document.
