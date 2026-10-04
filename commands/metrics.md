---
description: Read-only delivery report: volumes, rates, per-template performance
---

Write a delivery/engagement report for the company (argument, or email_list_projects to resolve which one). Read-only — do not change anything.

Start with `email_get_optimization_plan` — the ranked action feed frames what matters. Then pull `email_get_email_metrics` for the requested period (default 7 days) grouped by template and by day, plus `email_search_messages` for notable failures. Structure: the plan's ranked actions (severity + evidence) → headline numbers (sent, delivered %, open %, click %, bounce %, complaints, unsubs) → per-template standouts (best/worst) → the 3 highest-impact next moves. If the plan surfaces an A/B winner ready to promote, present it and only act on the human's explicit OK (`email_promote_ab_winner`).

$ARGUMENTS
