# יומן עבודה יומי — ע.ל.ת

Daily work journal per project, fed by the company WhatsApp group via the Grok bot webhook.
Spec: `docs/אפיון.md`. Plan and stages: `docs/תוכנית.md`. Talk to the user in Hebrew.

## Stack
Next.js (App Router, TypeScript) · Supabase (Postgres + Storage + Auth) · Vercel (auto-deploy from GitHub) · Claude API for field extraction · server-side PDF.

## Work rules
- Work stage by stage per `docs/תוכנית.md`. Don't start a stage before the previous one is approved.
- Write the complete code for a stage first, then run it.
- After every feature:
  - **E2E**: the full flow from a WhatsApp webhook message to the screen (Playwright, AI mocked via `AI_MOCK=1`).
  - **Edge**: message without text, photo without project, duplicate message, very long message.
  - **Regression**: run the full test suite (`npm test` + `npm run test:e2e`); nothing existing may break.
- **Security** (review at the end of every stage):
  - Per-project permissions enforced in the DB (RLS), not only in the UI.
  - Passwords only through Supabase Auth (hashed); never stored by us.
  - Webhook accepts only requests signed by the bot (HMAC + timestamp).
  - API keys only in environment variables (`.env.local`, Vercel env). Never in code or git. `service_role` key is server-only.
- Only after a stage works and tests pass: update `docs/החלטות.md` and `docs/באגים.md` as needed, then commit and push to GitHub.

## Docs (Obsidian)
- `docs/` is part of the user's Obsidian vault — use `[[Note Name]]` wikilinks between notes.
- `docs/יומן-פיתוח.md` is appended automatically by the Stop hook (`.claude/hooks/dev-log.mjs`). Don't edit past entries.
- `docs/החלטות.md`: one entry per decision — date, decision, why.
- `docs/באגים.md`: one entry per bug — date, symptom, cause, fix, test that now covers it.

## Out of scope (v1)
Quantities (belong to procurement), automatic daily report, native phone app, links to procurement / as-made / attendance.
