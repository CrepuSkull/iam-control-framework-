# Changelog — IAM Control Framework

All notable changes to this framework are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [2.0.0] — 2026-05-28

### Added
- **§0 Schema Validation** — mandatory Step 0 before any connection attempt.
  Checks config_version compatibility and all required fields in IAM-Params.
- **§5bis Safety Breaker** — 4 cumulative circuit breakers for Type 2 and 3 scripts:
  CB1 business hours enforcement · CB2 absolute batch size · CB3 relative batch % · CB4 canary DryRun on 3 objects
- **§8bis Digital Notary** — OpenTimestamps (.ots) for all markets; RFC 3161 (.tsr) for CH/LU markets
- **Greylist system** — third account list between whitelist and general population.
  Accounts enter greylist automatically; owner notified within 24h; auto-escalation on expiry.
- **lib/Auth-Helper.psm1** — authentication abstraction module (Dev / Prod-Cert / Prod-Vault).
  Zero credentials in any script. Only AUTH_SUCCESS / AUTH_FAIL traced.
- **lib/IAM-Core.psm1** — shared trace module: Write-Step · Write-StepOK · Write-StepError ·
  Write-StepWarn · Write-StepInfo · Write-StepSkip · Write-StepGrey · Write-Log.
  Includes whitelist/greylist loaders, resume/rollback helpers.
- **config_version field** in IAM-Params — enables §0 compatibility check.
- **greylist section** in IAM-Params — grace_until · owner_notified · escalate_to · close_reason.
- **safety_breaker section** in IAM-Params — all 4 CB thresholds configurable.
- **notariat section** in IAM-Params — OTS endpoint + RFC 3161 TSA URL + enabled_markets.
- **Annual-GreylistReview.ps1** — annual greylist review and cleanup.
- Unit tests for all new lib modules.

### Changed
- Script methodology updated from 9 to **15 sections**.
- Delivery checklist updated from 16 to **19 points**.
- IAM-Params-TEMPLATE.json fully restructured with all new sections.
- §6 Logique métier — greylist check added alongside whitelist at every loop iteration.
- §4 Connexion — now calls `Connect-IAMEnvironment` from Auth-Helper.psm1 exclusively.

### Security
- No credentials, tokens, or secrets may appear in any script or log file.
- `Assert-ReadOnlyScope` added in Auth-Helper to enforce read-only permissions on Type 1 scripts.

---

## [1.0.0] — 2026-03-30

### Added
- Initial release of IAM-Lab Framework (5 repos → unified as iam-control-framework).
- **6 GitHub repositories** covering the full IAM functional scope:
  `iam-foundation-lab` · `iam-lab-identity-lifecycle` · `iam-governance-lab` ·
  `iam-evidence-sealer` · `iam-federation-lab` · `iam-ma-integration-lab`
- **Type 1 scripts**: Audit-MFA · Audit-InactiveAccounts · Audit-OrphanAccounts ·
  Audit-PrivilegedRoles · Audit-RBAC · Audit-SoD · Audit-LegacyAuth ·
  Audit-ConditionalAccess · Audit-OAuthApps · Audit-ExternalAccounts ·
  Audit-HybridSync · Audit-NTFS · Audit-Licenses · Audit-HRIT-Reconciliation
- **Type 2 scripts**: Remediate-* counterparts for all audit domains
- **Type 3 orchestrators**: Run-DiagnosticFlash · Run-PeriodicControls · Run-AccessReview
- **JML scripts**: Process-Leaver · Process-Mover · Process-ForensicSnapshot
- **Periodic scripts**: Daily · Weekly · Monthly · Quarterly · Annual cycles
- SHA-256 sealing on all outputs (.seal files)
- RFC 3161 integration in iam-evidence-sealer
- Whitelist system with hardening verification
- IAM-Params-TEMPLATE.json — client configuration file
- Canonical naming convention: `[CLIENT_ID]_[TYPE]_[DOMAINE]_[AAAAMMJJ].[ext]`
- Score de maturité IAM (5 niveaux, 0–100)
- Compliance mapping: ISO 27001 · NIST CSF 2.0 · CIS Controls · FINMA · CSSF/DORA
- 9-section script methodology (predecessor of v2.0 15-section standard)
- M&A integration scenario: CorpA (Entra ID) absorbing CorpB (AD) — 300+ accounts · 625 SaaS apps
