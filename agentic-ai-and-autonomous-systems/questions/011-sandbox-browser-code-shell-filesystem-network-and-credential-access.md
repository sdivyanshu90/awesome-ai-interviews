### Q: Sandbox browser, code, shell, filesystem, network, and credential access.
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Run untrusted execution in ephemeral isolated environments with no ambient credentials, restricted network/filesystem/process limits, resource quotas, vetted dependency policy, output limits, and explicit data egress controls. Treat browser pages and command output as hostile input.
* **The Deep Dive:** Use separate identities for orchestration and sandbox, allowlist destinations, mount only task-scoped files read-only unless writes are necessary, and scan artifacts before promotion. Set CPU/memory/time quotas and kill descendants. Browser automation needs domain policy, download handling, form-submission approval, and defenses against DOM-based prompt injection.
Use a fresh unprivileged sandbox per run with read-only base image, bounded writable volume, CPU/RAM/PID/time limits, no host socket, and default-deny egress. Network access goes through an allowlisting proxy; packages come from pinned scanned mirrors; credentials are injected only to the authorized process and expire quickly. Browser downloads and tool output are untrusted. Preserve command, image digest, policy decision, resource metrics, and output hash for review while redacting secrets.

* **Production Reality & Tradeoffs:** Perfect isolation is difficult; layer containers/VMs, network controls, secret brokers, and monitoring. Make sandbox failure a safe, diagnosable result rather than falling back to host execution.
* **Red Flag:** Running generated shell commands directly on the orchestrator host.
