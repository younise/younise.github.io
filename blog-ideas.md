# Blog Content — VMware Cloud Foundation 9.0 Series

> **Series angle:** Lead every post with what most practitioners get wrong or skip, before covering the mechanics. Practitioner perspective, not a documentation mirror.
> **Start with:** Post #3 or #4 — opinionated, immediately useful, builds audience faster than an intro post.

---

## Part I — Context & Strategy

- [ ] **1. VCF 9.0: What Actually Changed and Why It Matters** — architectural shifts (not a feature list), what Broadcom simplified and what they removed
- [ ] **2. The Broadcom Licensing Model Decoded** — VCF SKUs, what's bundled, what's add-on, and how to avoid buying the wrong thing
- [ ] **3. Design Decisions You Can't Easily Undo** — the 5 choices made at deployment that define your environment for years
- [ ] **4. The Real Bill of Materials** — hidden requirements the quickstart guide skips: IP blocks, DNS zones, NTP hierarchy, certificates, service accounts

---

## Part II — Pre-Deployment Planning

- [ ] **5. Network Design for VCF: The Choices That Define Your Experience** — management, vMotion, vSAN, TEP VLANs — why the defaults aren't always right
- [ ] **6. DNS, NTP, and Certificates: Getting the Boring Stuff Right** — the #1 cause of failed deployments is not the hardware
- [ ] **7. HCL Reality Check** — VCF HCL vs. what you have vs. what you need for supportability
- [ ] **8. Sizing for Production vs. the Minimum Requirements Trap** — what 4-host starter clusters actually cost you operationally

---

## Part III — Deployment

- [ ] **9. Cloud Builder 9.0 Walkthrough: What the Wizard Doesn't Tell You** — pre-validation failures, what each check actually means
- [ ] **10. SDDC Manager 9.0: The Control Plane You Need to Understand** — not a UI tour, but what it owns and what happens when it's wrong
- [ ] **11. vSAN in VCF: Storage Policy Decisions That Stick** — FTT, dedup/compression, ESA vs. OSA trade-offs
- [ ] **12. NSX in VCF 9.0: The Simplified Model and Its Trade-offs** — what got easier, what got less flexible

---

## Part IV — Day 2 Operations

- [ ] **13. Lifecycle Management the Right Way** — why you should never patch outside SDDC Manager, sequencing, async patches
- [ ] **14. Certificate Management at Scale** — Microsoft CA integration, wildcard certs, what breaks when certs expire
- [ ] **15. Identity and Access Across VCF** — vCenter, NSX, SDDC Manager IAM: one source of truth or three?
- [ ] **16. Capacity Management** — when to add hosts, when to add clusters, what the metrics actually mean

---

## Part V — Workload Consumption

- [ ] **17. Workload Domain Strategy: To Domain or Not to Domain?** — operational and licensing cost of every new domain
- [ ] **18. Migrating Existing Workloads to VCF** — what the migration guides skip about networking, storage policy, and downtime
- [ ] **19. VCF as Private AI Infrastructure** — GPU passthrough, DRS constraints, storage throughput considerations
- [ ] **20. What's Left of Tanzu in VCF 9.0** — Kubernetes reality check: Supervisor, TKGs, and what's actually supported

---

## Part VI — Upgrades

- [ ] **21. The Pre-Upgrade Checklist That Actually Matters** — what to validate before you click upgrade (not the official checklist)
- [ ] **22. When Lifecycle Operations Fail** — troubleshooting SDDC Manager bundle downloads, precheck failures, remediation
- [ ] **23. Post-Upgrade Validation** — how to confirm the environment actually works, not just that the UI says green

---

## Part VII — Advanced & Operational Maturity

- [ ] **24. Multi-Site VCF: Stretched Clusters and Site Recovery** — what's supported, what's not, where the edges are
- [ ] **25. Security Hardening Beyond Defaults** — STIG applicability, what VCF exposes that you need to lock down
- [ ] **26. Monitoring and Observability Gaps** — what VCF tells you, what it doesn't, and how to fill those gaps with Aria Operations
