---
description: Deliverability incident triage: bounces, complaints, blocked sends
---

Triage a deliverability problem with the user. Load the `email-deliverability` skill (bundled) and follow it.

1. Quantify: `email_get_email_metrics` for the affected window — bounce and complaint rates vs the prior period.
2. Localize: `email_search_messages` filtered to bounced/complained/failed — which template, which recipient domains, when did it start?
3. Inspect examples: `email_get_message` on representative failures; read the provider's bounce classification in the event payload.
4. Check infrastructure: `email_get_connection_status` — domain still verified? DNS records intact?
5. Diagnose (list hygiene vs content vs infrastructure vs volume spike) and recommend the fix. If sending should stop while fixing, say so explicitly — pausing sends (`email_set_sends_paused`) is the user's call to make.
6. Confirm suppressions are in place for the bad addresses (`email_list_suppressions`); never remove complaint suppressions.

$ARGUMENTS
