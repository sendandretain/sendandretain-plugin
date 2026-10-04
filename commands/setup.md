---
description: Take a company from nothing to a live email programme: understand the business, design the flows, write the emails, then set up sending
---

Take a company from nothing to a live lifecycle email programme.

The order matters. Nothing in phases 1-3 needs a provider, a domain or a sender —
templates can be created and published with none of them — so do NOT ask for DNS
records or API keys until phase 4. Asking for the hardest, least rewarding work
before the user has seen anything is what this order exists to avoid.

**Phase 1 · Understand.** `email_list_projects`, or `email_create_project` with their
website for a new one (it reads the site and infers the brand). Then learn five
things, two at a time, in conversation — never as a form, and never re-asking what
the site already answered. Use `email_fetch_url` yourself when an answer is thin.
Record each with `email_record_onboarding_answers` the turn you learn it, so an
interrupted session resumes where it stopped:
  - what they sell
  - what happens in their product worth sending on, and which events their app
    already emits — this decides every automation's trigger, and a flow wired to
    an event nobody sends looks alive and is dead
  - who is on the list, and whether it exists elsewhere already
  - the one lifecycle moment they most want fixed
  - the domain they will send from — ask for the NAME here and call
    `email_create_domain` immediately, so verification runs in the background for
    the rest of the conversation instead of blocking the end of it

**Phase 2 · Strategy.** `email_list_sequence_packs`, propose the archetype and the
flows in prose — naming each flow's trigger and asking whether their app emits it
— then `email_bootstrap_company` once they agree. It publishes the starter
templates and creates the sequences PAUSED. If no archetype really fits, force-fit
the nearest and say so; the packs are step skeletons and the specificity lives in
the copy.

**Phase 3 · Content.** Rewrite each template in the company's voice
(`email_update_template`), `email_render_template` to catch compile errors, then
`email_publish_template_version`. Start with the email for the moment they said
hurts most. `email_send_template_test` to their own address — over MCP this needs
a verified domain, so if phase 1's domain has not landed yet, say so plainly
rather than promising an inbox.

**Phase 4 · Live.** Now the setup work. `email_get_connection_status` →
`email_connect_provider` (the user pastes the key on the settings page, never in
chat) → `email_verify_domain` → `email_register_webhooks` → `email_create_sender`
→ `email_create_api_key` (relay the secret immediately, shown once) and hand over
a `POST /api/v1/events` snippet for **exactly the triggers the chosen packs use**.
If they are migrating, offer `email_import_suppressions` first. Then
`email_pre_launch_check` and enable flows ONE at a time on their explicit OK.

Done is a flow live, its trigger seen, and a real send on their own domain —
not templates published.

$ARGUMENTS
