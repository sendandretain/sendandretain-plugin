---
description: Author, test, and publish a react.email template for a company
---

Author a new email template with the user. Load the `email-template-authoring` skill (bundled) and follow it.

1. Resolve the company (`email_list_projects` if needed) and read its brand kit + brief (`email_get_brand`) — the template uses the COMPANY's brand, not generic styling.
2. Agree on: purpose (transactional vs lifecycle — this drives unsubscribe behavior), subject, variables (name, type, required, fallback), and the content outline.
3. `email_create_template` with the TSX source (react.email components, <Tailwind> wrapper).
4. `email_render_template` with example variables — fix any compile errors it returns, iterate until the HTML looks right.
5. `email_send_template_test` to the user's inbox; ask them to check rendering (including dark mode / Gmail).
6. On their explicit OK: `email_publish_template_version`. Never publish untested.

$ARGUMENTS
