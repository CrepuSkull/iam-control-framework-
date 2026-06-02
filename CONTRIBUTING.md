# Contributing to IAM Control Framework

Thank you for your interest in contributing. This framework is built on an
**engineering-first methodology** — every script is a legal evidence artifact,
not a utility script. Please read this guide before submitting any change.

---

## Before You Start

1. **Read `SCRIPT-METHODOLOGY.md`** entirely. The 15-section structure is mandatory
   for every script. Pull requests that don't follow it will not be merged.

2. **Understand the 3-list system** (Whitelist / Greylist / Population).
   Any script touching account remediation must handle all three.

3. **Understand the evidence chain**: SHA-256 → .seal → OpenTimestamps (.ots) → RFC 3161 (.tsr).
   Never skip the sealing step.

---

## Development Workflow

### 1. Fork & Branch

```bash
git fork https://github.com/CrepuSkull/iam-control-framework.git
git checkout -b feature/your-feature-name
```

Branch naming convention:
- `feature/` — new script or module
- `fix/` — bug correction
- `docs/` — documentation only
- `test/` — test fixtures or unit tests

### 2. Script Structure

Every new script must include all **mandatory sections** (§0 through §9)
and any relevant optional sections (§10–§13).

Use the lib modules — never reimplement:
- `IAM-Core.psm1` — all trace functions (`Write-Step*`)
- `Auth-Helper.psm1` — all authentication
- `IAM-Sealing.psm1` — all sealing
- `IAM-OTS.psm1` — all OpenTimestamps
- `Greylist-Manager.psm1` — all greylist operations

### 3. Parameters

If your script reads a new field from `IAM-Params-[CLIENT].json`:
- Add the field to `params/IAM-Params-TEMPLATE.json` with a `[placeholder]` value
- Add the field to `params/IAM-Params-EXAMPLE.json` with a realistic fictional value
- Document the field in `docs/params/params-structure.md`

### 4. Tests

Every new script must have:
- A fixture dataset in `tests/fixtures/[ScriptName]-test-dataset.csv`
- Expected outputs in `tests/expected/[ScriptName]-expected.csv`
- A unit test in `tests/unit/Test-[ScriptName].ps1`

Run tests locally before pushing:

```powershell
.\scripts\type1-audit\Your-NewScript.ps1 `
    -ParamsFile ".\params\IAM-Params-EXAMPLE.json" `
    -Test
```

### 5. Checklist Before PR

```
[ ] §0 Schema Validation present and working
[ ] All 9 mandatory sections implemented
[ ] No hardcoded values — all from IAM-Params
[ ] Auth via Auth-Helper.psm1 only — no credentials in script
[ ] Whitelist checked at every loop iteration
[ ] Greylist checked at every loop iteration
[ ] DryRun is default — -Execute required for real action
[ ] StateBefore logged before any modification (Type 2)
[ ] SHA-256 + .seal generated
[ ] OpenTimestamps .ots submitted
[ ] Test mode works against fixture data
[ ] PSScriptAnalyzer passes with no errors
[ ] params/IAM-Params-TEMPLATE.json updated if new fields
[ ] docs/ updated if new behavior
```

### 6. Commit Style

```
feat: add Audit-ShadowGroups.ps1 — detects deep nested group membership
fix: Remediate-MFA — greylist expiry check was off by 1 day
docs: update sealing-protocol.md with RFC 3161 verification procedure
test: add fixture dataset for Audit-ShadowGroups
```

### 7. Pull Request

- Target branch: `main`
- Title: `[TYPE] Short description`
- Description: what it does, which IAM-Params sections it reads, which referentials it covers
- Attach a sample `.log` output from a `-Test` run

---

## Code Style

- **PowerShell**: follow existing patterns in `type1-audit/Audit-MFA.ps1` as reference
- **Variables**: follow the naming conventions in `SCRIPT-METHODOLOGY.md`
- **Comments**: in English — the framework targets international environments
- **No aliases**: use full cmdlet names (`ForEach-Object` not `%`, `Where-Object` not `?`)

---

## Questions

Open a GitHub Discussion before opening a PR for major changes.
For security issues, contact directly via LinkedIn.
