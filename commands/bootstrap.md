---
description: One command: brand, starter templates, and paused lifecycle sequences for a company — from just a domain
---

Stand up a company's whole email setup fast. Load the `email-business-bootstrap` skill (bundled) and follow it.

1. Resolve the company (`email_list_projects`, or `email_create_project`).
2. Brand first: `email_get_brand`; if empty, `email_fetch_url` the company's website, infer the palette/logo/voice, and save it with `email_update_brand` — starters bake the brand in, so this is what makes the emails on-brand.
3. `email_list_sequence_packs` — present the archetypes (saas / ecommerce / newsletter-media / marketplace) and confirm the choice with the user.
4. `email_bootstrap_company` with the archetype. It materializes + publishes the starter templates and creates the sequence automations PAUSED. Read back the per-automation timelines and pre-launch results.
5. Polish: every step ships its own starter, written for that job but not for this company — rewrite each in the brand voice (`email_update_template`) and re-publish; `email_preview_automation` shows the real emails.
6. `email_send_template_test` each template, then enable sequences ONE at a time on the user's explicit OK (`email_set_automation_status`). Bootstrap never sends and never enables.

$ARGUMENTS
