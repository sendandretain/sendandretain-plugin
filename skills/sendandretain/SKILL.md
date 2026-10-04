---
description: Use Send & Retain from Claude Code — set up sending for a company (provider, domain, senders, API keys), author react.email templates, send and observe transactional + lifecycle email, and manage automations. Use when the user asks about a company's email managed in Send & Retain or wants to set one up.
---

# Send & Retain

This plugin connects Claude Code to your Send & Retain instance via a remote MCP
server. Tools appear with the prefix `email_*` and return a JSON envelope
`{ ok: true, … }` on success or `{ ok: false, error: "…" }` on failure — read
the error, it usually says exactly what to do next.

One **project = one company**: provider keys, domains, senders, templates,
contacts, suppressions, automations, and metrics are all per project.

## Authorization: scope model

You connect via OAuth (see Install below) — nothing to generate or copy. An
OAuth session is always workspace-scoped with full write access: every tool in
this reference is callable, across every company in the workspace.

The endpoint also accepts a first-party `aem_` API key (Workspace → Settings →
API keys), verified by the same code that verifies it for the REST API — one
credential, revoked once. That is the credential for a headless caller: it
carries its own scope and expiry and is pinned to a single company, so a key
session is narrower than the workspace-wide OAuth one. There is no MCP-specific
token to mint either way.

- Project-scoped tools take an optional `projectId` argument — call
  `email_list_projects` first and pass the company's `projectId` on every
  project-scoped call.
- Workspace-level tools (`email_create_project`, settings, cross-company
  metrics) need no `projectId`.

## Setup — from zero to sending

1. `email_create_project` (or pick from `email_list_projects`).
2. `email_get_connection_status` — provider connected? `email_connect_provider`
   returns the settings-page URL where the HUMAN pastes the Resend/SendGrid
   API key. Keys never travel over MCP or chat.
3. `email_create_domain` — registers the sending domain with the provider and
   returns the DNS records (SPF/DKIM). Hand them to the human to add at their
   DNS host, then `email_verify_domain` once propagated.
4. `email_register_webhooks` — delivery/bounce/complaint events flow back to us.
5. `email_create_sender` — the from identity (requires a verified domain).
6. `email_send_test_email` — prove the plumbing end to end.
7. `email_create_api_key` — the company's app sends via
   `POST /api/v1/emails` with this key. The secret is shown ONCE.

## Templates — the authoring loop

react.email TSX, stored per company, versioned; published versions are
immutable (edits create a new draft version).

`email_create_template` → `email_render_template` (fix compile errors from
the result; returns HTML + text + subject) → `email_send_template_test` to a
real inbox → `email_publish_template_version`. **Never publish untested.**
Templates use the COMPANY's brand (kit from `email_get_brand`, falling back
to the brief; persist inferred tokens with `email_update_brand`), not the
app's. Variables ({{name}}, props) are declared in the template's
variables spec; required-without-fallback fails the render.

## Sending & observability

- `email_send_email` — single recipient by design (no blasts). Suppressions
  are enforced platform-side: bounce/complaint/manual block everything;
  unsubscribe blocks lifecycle templates only.
- `email_get_message` / `email_search_messages` — per-message event timeline
  (queued → sent → delivered → opened/clicked, bounces, complaints).
- `email_get_email_metrics` — rates by template/day. `complaintRatePct` is
  complaints ÷ mail to inboxes that report spam (Gmail and iCloud never do);
  at 0.08% or above, stop and investigate.
- `email_list_suppressions` / `email_add_suppression` /
  `email_import_suppressions`. NEVER remove a complaint suppression.

## Recovering from errors

- `401 Unauthorized`: the OAuth session expired or was revoked — run `/mcp`
  (Claude Code) or reconnect the connector (claude.ai) to re-authorize.
- "No project in scope": pass `projectId` (from `email_list_projects`).
- Provider auth errors: `email_get_connection_status`, then re-enter the key
  on the settings page it links.
