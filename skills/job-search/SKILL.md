# Job Search

Use this skill when the user wants Codex to run Career-Ops for job search work:

- evaluate a job URL or pasted JD
- scan portals for new roles
- tailor a resume for a specific position
- generate a tailored ATS PDF
- compare multiple roles
- help fill an application form
- review tracker status, follow-ups, interview prep, or company research

## Read First

Before doing any work, read these files in order:

1. `docs/CODEX.md`
2. `CLAUDE.md`
3. `.claude/skills/career-ops/SKILL.md`

Treat this file as the Codex entrypoint and `.claude/skills/career-ops/SKILL.md`
as the canonical router for mode selection.

## Routing

Use the existing Career-Ops modes. Do not create a parallel Codex workflow.

- Raw JD text or job URL: `modes/_shared.md` + `modes/auto-pipeline.md`
- Evaluation only: `modes/_shared.md` + `modes/oferta.md`
- Multi-offer comparison: `modes/_shared.md` + `modes/ofertas.md`
- Portal scan: `modes/_shared.md` + `modes/scan.md`
- PDF generation: `modes/_shared.md` + `modes/pdf.md`
- Application assistance: `modes/_shared.md` + `modes/apply.md`
- Pending URL pipeline: `modes/_shared.md` + `modes/pipeline.md`
- Tracker status: `modes/tracker.md`
- Deep company research: `modes/deep.md`
- Interview prep: `modes/interview-prep.md`
- Training / certification review: `modes/training.md`
- Project evaluation: `modes/project.md`
- Rejection pattern review: `modes/patterns.md`
- Follow-up cadence: `modes/followup.md`

## Onboarding Checks

At the start of a session, check for these files:

- `cv.md`
- `config/profile.yml`
- `modes/_profile.md`
- `portals.yml`

If `modes/_profile.md` is missing, copy it from `modes/_profile.template.md`.
If `config/profile.yml` or `portals.yml` is missing, create them from the
example/template files before deeper work. Keep all user personalization in
those user-layer files, never in `modes/_shared.md`.

## Rules

- Reuse the checked-in scripts, templates, and tracker flow.
- Use Playwright-backed flows for job liveness and application assistance when available.
- Treat `tailor my resume for this role` as permission to generate job-specific resume output for that exact role only.
- Only enter live application-assist mode when the user has explicitly approved that exact job. Store reusable allowlist entries in `config/apply-permissions.yml`.
- If `config/apply-permissions.yml` is used, prefer capability flags such as `permissions.tailor_resume` and `permissions.assist_application` so job-scoped approvals stay explicit.
- Never submit an application on the user's behalf.
- Never treat resume-tailoring permission as permission to submit.
- Never write tracker rows directly into `data/applications.md`; use the existing TSV/merge flow.
- Keep repo changes system-level. Put user-specific content in gitignored user-layer files.

## Verification

Use these commands after system-level changes:

```bash
npm run doctor
node test-all.mjs --quick
```

For dashboard changes:

```bash
cd dashboard && go build ./...
```
