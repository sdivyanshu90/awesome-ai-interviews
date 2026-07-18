### Q: Sandbox code, browser, file, network, package, and subprocess access.
* **Difficulty:** Principal
* **Category:** Runtime Security
* **The 10-Second Pitch:** Execute untrusted operations in ephemeral isolated workers with no ambient secrets, minimal mounts, network allowlists, package controls, resource limits, syscall/process restrictions, and artifact promotion gates.
* **The Deep Dive:** Use VM/container defense in depth, nonroot identity, read-only base, task-scoped workspace, seccomp/capabilities, CPU/RAM/time/process quotas, and blocked host sockets. Browser handles downloads/forms/redirects and untrusted DOM. Network policy blocks metadata/internal ranges and uncontrolled egress. Packages come from pinned proxy with hashes. Scan outputs before moving to trusted storage.
* **Production Reality & Tradeoffs:** Containers alone are not a perfect boundary. Some tasks need network/build tools; grant per-task capabilities. Monitor escapes and maintain patching/kill switch.
Layer OS/container/VM isolation with read-only images, unprivileged users, seccomp/capability restrictions, CPU/RAM/PID/time quotas, bounded scratch space, and default-deny network. Browser/package access goes through policy proxies and pinned mirrors. Never mount host sockets or broad cloud credentials. Treat artifacts and output as untrusted, scan downloads, and destroy the environment after use. Test escapes, fork bombs, symlink traversal, DNS rebinding, and timeout-after-side-effect behavior.

* **Red Flag:** Running generated code on the orchestrator with production credentials.
