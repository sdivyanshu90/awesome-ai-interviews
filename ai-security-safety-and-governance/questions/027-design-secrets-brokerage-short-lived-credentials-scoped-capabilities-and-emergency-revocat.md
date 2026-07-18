### Q: Design secrets brokerage, short-lived credentials, scoped capabilities, and emergency revocation.
* **Difficulty:** Principal
* **Category:** Security Architecture
* **The 10-Second Pitch:** Keep secrets outside model/prompt/logs. A broker authenticates workload/user and issues short-lived, action-scoped credentials or performs the operation on behalf of the agent.
* **The Deep Dive:** Use workload identity and policy inputs: tenant, user, tool, resource, purpose, limits, approval digest, and expiry. Broker audits issuance/use and rotates root material. Capability tokens are nontransferable where possible and audience-bound. Executors receive credentials just before call, zeroize afterward, and do not echo. Central revocation/kill switch blocks issuance and invalidates sessions.
* **Production Reality & Tradeoffs:** Short TTL does not fix overbroad scope. Distributed revocation may lag. Design tool behavior when broker unavailable and rehearse rotation.
A secrets broker authenticates the workload and exchanges an approved capability request for a short-lived scoped credential; the secret never enters prompt or trace. Scope includes resource, operation, tenant, purpose, and maximum value, with audience-bound tokens and non-exportable identities where possible. Central revocation and emergency deny policies beat waiting for expiry. Audit issuance and use separately. Avoid long-lived environment variables inherited by arbitrary child processes.

* **Red Flag:** Putting encrypted secrets in the prompt because the model cannot decrypt them.
