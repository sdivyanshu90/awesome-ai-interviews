### Q: Compare direct, indirect, stored, cross-domain, multimodal, and tool-result prompt injection.
* **Difficulty:** Senior
* **Category:** Security
* **The 10-Second Pitch:** All injections place attacker instructions in model context, but delivery differs: user prompt, retrieved content, persistent memory, another trust domain, image/audio, or tool output. The defense must cover every ingestion path.
* **The Deep Dive:** Direct input arrives from the user. Indirect injection hides in webpages/documents/email. Stored injection persists in databases or memory and triggers later. Cross-domain injection exploits a privileged agent reading an untrusted domain. Multimodal payloads use OCR, pixels, audio, or metadata. Tool-result injection enters structured/free-form API output. Provenance labels help; least privilege, content minimization, output/action validation, egress control, and approval enforce security.
* **Production Reality & Tradeoffs:** Encoding/obfuscation bypasses text filters. Sanitization can remove useful content and never proves safety. Test multi-turn and delayed activation.
Direct injection is authored in the user message; indirect injection arrives through retrieved/web/tool content; stored injection persists in memory or corpora; cross-domain injection crosses a connector boundary; multimodal injection hides commands in images/audio; tool-result injection impersonates trusted status. The delivery channel differs, but the control failure is identical: untrusted data influences control flow. Record source/trust metadata and test each channel through the full executor path.

* **Red Flag:** Treating only `ignore previous instructions` text as prompt injection.
