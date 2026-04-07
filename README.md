# HARDCORE Code Security Review Skill Documentation

A practical, developer-friendly guide for using and maintaining the `code-security-review` skill.

---

## 1) Skill Snapshot

- **Skill name:** `code-security-review`
- **Goal:** Perform an exhaustive security and code quality review across an entire workspace.
- **Primary use case:** Deep codebase security audits with actionable remediation guidance.
- **Core promise:** "Read everything, miss nothing important, prioritize by exploitability."

### Trigger Intent (Examples)

Use this skill when users ask things like:

- "check my code"
- "is this secure"
- "audit this project"
- "find vulnerabilities"
- "security scan"
- "review this codebase"

---

## 2) Audit Scope

This skill is designed to analyze **all workspace files**, not only source code.

### Included file classes

- Source code (multi-language)
- Config and environment files
- Infrastructure files (Docker, Kubernetes, Terraform, CI/CD)
- Dependency manifests and lock files
- DB schemas/migrations/dumps
- API specs and docs
- Security assets (certs/keys where present)

### Major finding categories

- Injection vulnerabilities
- AuthN/AuthZ flaws
- Cryptography and key management issues
- Data exposure and secret leakage
- Business logic abuse
- Misconfiguration
- Malware/backdoor indicators
- Supply-chain risk
- Performance and code-quality risks

---

## 3) Operating Principles

- **Comprehensive first:** inspect complete file inventory before detailed findings.
- **Security-first prioritization:** critical exploitable issues are surfaced first.
- **Evidence-backed findings:** include file location, vulnerable pattern, impact, and fix.
- **Actionability:** every significant issue should include remediation steps.
- **Developer usability:** findings should map clearly to owner teams and next actions.

---

## 4) Standard Workflow (8 Phases)

## Phase 1: Recon & Inventory

Build complete visibility:

- Total files, folders, size
- File type breakdown by language/config/infra/dependency/data
- High-risk directories (`auth`, `admin`, `payment`, `api`, etc.)

## Phase 2: Secret & Credential Hunt

Search for:

- API keys and tokens
- Cloud credentials
- DB credentials and connection strings
- Private keys and certificates
- JWT/OAuth secrets
- Webhook endpoints

## Phase 3: Malware / Backdoor Detection

Inspect suspicious patterns such as:

- `eval`/`exec` abuse
- Obfuscated payloads
- Webshell signatures
- Cryptominer indicators
- Suspicious outbound network behavior

## Phase 4: File-by-File Review

Apply a deterministic review order:

1. Env/config files
2. Auth/AuthZ flows
3. API endpoints
4. Database operations
5. File handling paths/uploads
6. Crypto usage
7. Payment/financial logic
8. Third-party integrations
9. Remaining source files
10. Dependencies
11. Infrastructure
12. Documentation/info disclosure

## Phase 5: Deep Vulnerability Analysis

Perform category-level checks for:

- SQL/NoSQL/command/template/XML/LDAP/XPath injection
- XSS (reflected/stored/DOM)
- CSRF
- SSRF
- Insecure deserialization
- Path traversal/ZIP slip
- Broken access control / IDOR
- Session/JWT/OAuth weaknesses
- Weak crypto and insecure randomness
- DoS vectors and regex backtracking risks
- Business logic race/TOCTOU/mass assignment issues

## Phase 6: Supply Chain Analysis

Review manifests and lock files for:

- Known CVEs
- Outdated packages
- Typosquatting/suspicious dependencies
- Unmaintained packages
- License/compliance risks

## Phase 7: Report Generation

Produce a structured audit report with:

- Executive summary
- Severity distribution
- Detailed findings with CVSS + CWE
- PoC/attack path for critical findings
- Remediation roadmap (phased)
- Compliance mapping (OWASP/CWE/GDPR/PCI/SOC2 where relevant)

## Phase 8: Delivery & Follow-up

Deliver report and offer:

- Deep dives
- Fix plan assistance
- Issue tracker ticket generation
- Re-audit after remediation

---

## 5) Output Contract

### Default output artifact

- `security-review-report.md`

The original skill text uses `/mnt/user-data/outputs/security-review-report.md`; adapt this path to your runtime/workspace conventions if needed.

### Required report sections

- Executive summary and overall risk level
- Severity and category breakdown tables
- Critical findings (full detail)
- High/Medium/Low findings
- Secret exposure summary
- Supply chain analysis
- Infra/config risk summary
- Compliance mapping
- Remediation roadmap with timeline
- Testing recommendations
- Methodology and disclaimer

---

## 6) Severity Model (CVSS v3.1)

| Severity | CVSS | Typical Handling |
|---|---:|---|
| Critical | 9.0 - 10.0 | Emergency response (same day) |
| High | 7.0 - 8.9 | Short-term sprint priority |
| Medium | 4.0 - 6.9 | Planned remediation |
| Low | 0.1 - 3.9 | Backlog / hardening |

### Prioritization rule

Rank by: **exploitability + impact + exposure** (not severity label alone).

---

## 7) Language-Specific Focus Areas

- **Python:** `eval`/`exec`, `pickle.loads`, `yaml.load`, shell injection, raw SQL formatting.
- **JavaScript/TypeScript:** DOM XSS sinks, `eval`, insecure JWT validation, prototype pollution vectors.
- **Java:** concatenated SQL, unsafe deserialization, XXE parser defaults, command execution.
- **PHP:** dangerous execution funcs, include/require abuse, upload validation gaps, unserialize risks.
- **Go:** SQL string formatting, `exec.Command` misuse, insecure path handling, SSRF on user URLs.
- **Ruby/Rails:** interpolation SQL, mass assignment, unsafe eval/send, unsafe redirects/files.
- **Rust:** unsafe blocks review, SQL string building, process execution with user input.

---

## 8) Developer-Friendly Finding Format

Use this finding template for consistency:

```markdown
### [ID] Finding Title
- Severity: Critical | High | Medium | Low
- CVSS: X.X
- CWE: CWE-XXX
- File: path/to/file
- Function/Area: name

**Why it matters**
Short impact statement.

**Evidence**
Code snippet or query/pattern details.

**Attack path**
How an attacker could exploit it.

**Fix**
Actionable remediation with secure code example.

**Verification**
How to test the fix and prevent regressions.
```

---

## 9) Audit Checklist

### Before starting

- Build full inventory of files
- Identify stack/languages/frameworks
- Confirm report location and format

### During analysis

- Track secrets first
- Prioritize externally reachable surfaces
- Capture file location and evidence per finding
- Deduplicate repeated findings by root cause

### Before delivery

- Ensure critical findings have clear remediation steps
- Ensure report is readable by both engineers and leads
- Verify severity consistency and CVSS rationale
- Include residual risk and testing gaps

---

## 10) Usage Examples

### Example prompts

- "Run a full HARDCORE security review on this project."
- "Audit this codebase for OWASP Top 10 and supply-chain issues."
- "Review auth, API, and DB layers first, then scan everything else."

### Example follow-up asks

- "Generate GitHub issues for all critical/high findings."
- "Create a 7-day remediation plan from this report."
- "Re-check only files changed in the fix PR."

---

## 11) Limitations

- No audit can guarantee 100% vulnerability discovery.
- Static review quality depends on code visibility and context.
- Dynamic exploitability may differ across environments.
- Findings should be validated in staging before production rollout.

---

## 12) Maintenance Guide (for Skill Authors)

Update this skill regularly to keep it useful:

- Refresh vulnerability patterns and CWEs
- Update recommended scanners/tooling
- Track new framework-specific insecure defaults
- Improve report templates from real audit feedback
- Keep remediation examples modern and copy-paste ready

---

## Quick Start (TL;DR)

1. Inventory all files.
2. Hunt secrets.
3. Analyze high-risk paths first (auth/api/db/file ops).
4. Review every remaining file category.
5. Generate detailed report with evidence and fixes.
6. Deliver prioritized remediation roadmap.
