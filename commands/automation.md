---
description: Design, test-drive, and (on explicit OK) enable an event-triggered email automation
---

Design an automation with the user. The bundled `sendandretain` skill is the reference for every tool and the safety model.

1. Resolve the company (`email_list_projects`) and agree the design: trigger event, exit event, audience routing (filters/priority for variants), step cadence (delays, send windows), and which templates each step uses (author missing ones via /sendandretain:new-template first).
2. `email_create_automation` — it is ALWAYS created paused. It returns a humanized timeline; `email_preview_automation` renders every send step so you can show the user the actual emails before anything is enabled.
3. Test-drive: `email_upsert_contact` a test contact (the user's address, realistic attributes incl. timezone) → `email_emit_contact_event` with the trigger → `email_list_automation_runs` to confirm enrollment and timing.
4. `email_pre_launch_check`, then walk the enable checklist: every template published, suppression import done (migrations), volumes sane for the domain's age.
5. Only on the user's explicit decision: `email_set_automation_status` to enable. Then watch the first cohort with `email_get_automation_metrics`.

$ARGUMENTS
