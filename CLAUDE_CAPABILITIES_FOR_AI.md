# 🤖 CLAUDE CAPABILITIES & SKILLS MANIFEST

**Documento para compartir con otras IAs**
**Última actualización:** 2026-03-26
**Usuario:** Julio Castillo | **Proyecto:** marcacion-grupo-ct

---

## 📋 DOCUMENTO DE PROPÓSITO

Este archivo describe **EXACTAMENTE** qué puede hacer Claude (yo) en el contexto del proyecto marcacion-grupo-ct. Úsalo para:

- ✅ Saber qué habilidades solicitar
- ✅ Entender qué formato de entrada espero
- ✅ Formular instrucciones más claras y específicas
- ✅ Evitar pedir cosas fuera de mi alcance
- ✅ Aprovechar al máximo mis capacidades

---

## 🎯 INFORMACIÓN GENERAL

| Propiedad | Valor |
|-----------|-------|
| **Name** | Claude Haiku 4.5 (20251001) |
| **Environment** | Cowork Mode (Linux VM Ubuntu 22) |
| **Workspace** | `/sessions/happy-nice-carson/mnt/marcacion-grupo-ct/` |
| **Project** | marcacion-grupo-ct v0.2.0 |
| **Stack** | Next.js 16 + React 19 + TypeScript + Supabase |
| **Active Skills** | 14 (documented below) |
| **Access** | Full filesystem, tools, MCPs |

---

## ⚡ CORE CAPABILITIES (ALWAYS AVAILABLE)

### 1. **Code Execution**
- ✅ Bash commands in Linux VM
- ✅ Can read/write/edit files in workspace
- ✅ Can execute scripts, install packages
- ✅ Can verify output by running commands

### 2. **File Operations**
- ✅ Read any file (text, code, PDFs, images)
- ✅ Create new files in workspace
- ✅ Edit existing files with exact string replacement
- ✅ Work with multiple file formats

### 3. **Web Access**
- ✅ WebSearch (search internet)
- ✅ WebFetch (fetch URLs & parse content)
- ✅ Can browse & interact with pages via Claude in Chrome

### 4. **MCP Integrations**
- ✅ Supabase (execute SQL, create migrations, generate types)
- ✅ Git/GitHub (read commits, PRs, interact with repos)
- ✅ Google Drive (search docs, read files)
- ✅ ClickUp (manage tasks, search, comment)
- ✅ Vercel (check deployments, view logs)

### 5. **Data Analysis**
- ✅ Can parse and analyze code
- ✅ Can trace through logic
- ✅ Can identify patterns, N+1 queries, security issues
- ✅ Can generate metrics and reports

---

## 🛠️ INSTALLED SKILLS (14 TOTAL)

### 📄 DOCUMENT SKILLS (4)

#### 1. **docx** - Word Document Creation & Editing
**Triggers:** Word doc, .docx, report, memo, template, letter, professional document
**Input Formats:**
- Instructions describing desired document
- Existing .docx files to edit
- Content in any format (markdown, plain text, code)

**Output Formats:**
- `.docx` files with formatting (headings, tables, TOC, page numbers, etc)
- Can include: images, headers/footers, styles, tracked changes

**Examples for Your Project:**
```
@Claude /docx

Create a Word document: "RLS Policy Implementation Guide"

Content should include:
- Table of Contents
- Concepts section with SQL examples
- Step-by-step implementation checklist
- Common mistakes
- Debugging guide

Format: Professional, dark theme, code blocks with syntax highlighting
```

---

#### 2. **xlsx** - Excel & Spreadsheet Operations
**Triggers:** Excel, spreadsheet, .xlsx, .csv, data table, data analysis, budget, metrics, chart, graph
**Input Formats:**
- Instructions for new spreadsheet creation
- Existing .xlsx/.csv files to edit/analyze
- Raw data (JSON, text, code output)

**Output Formats:**
- `.xlsx` files with formulas, formatting, charts
- Can include: pivot tables, conditional formatting, data validation, graphs

**Examples for Your Project:**
```
@Claude /xlsx

Analyze Supabase query performance data:
- Columns: RPC name, avg query time (ms), P95 latency, calls/day, error rate
- Time period: Last 2 weeks
- Create:
  - Performance trends chart
  - Slowest RPCs ranking
  - Heatmap by hour of day
- File: performance_analysis_20260326.xlsx
```

---

#### 3. **pdf** - PDF Processing
**Triggers:** PDF, .pdf, form, extract, merge, split, OCR
**Input Formats:**
- Existing PDF files
- Instructions for new PDF creation
- Multiple PDFs for merging

**Output Formats:**
- `.pdf` files (merged, split, edited, with annotations)
- Can extract text, tables, images from PDFs

**Examples for Your Project:**
```
@Claude /pdf

Extract text from PDF: "quarterly_report.pdf"

Find and return:
- All sections mentioning "performance"
- All SQL queries
- All metrics tables

Output: Extracted text in markdown format
```

---

#### 4. **pptx** - PowerPoint Presentations
**Triggers:** Presentation, deck, slides, slide deck, pitch, .pptx
**Input Formats:**
- Instructions for new presentation
- Existing .pptx files to edit/extract
- Content (markdown, text, data)

**Output Formats:**
- `.pptx` files with slides, layouts, speaker notes
- Can include: charts, images, animations, themes

**Examples for Your Project:**
```
@Claude /pptx

Create presentation: "Q1 2026 Engineering Review"

Slides:
1. Title: marcacion-grupo-ct Progress
2. Metrics: Users, uptime, performance improvements
3. Technical wins: Indexes, optimizations, migrations
4. Q2 priorities
5. Risks & mitigations

Style: Dark theme, 15 minutes duration
```

---

### 🔧 ENGINEERING SKILLS (10)

#### 5. **engineering:code-review** - Code Security & Performance Review
**Triggers:** "review this", PR, diff, "is this safe?", "N+1", "injection", "memory leak"
**Input Required:**
- Code snippet to review (exact file content)
- What specifically to check
- Context (what changed, why)

**Output Provides:**
- Security vulnerabilities found
- Performance issues (N+1, unbounded queries, etc)
- Code style suggestions
- Risk assessment

**Prompt Format:**
```
@Claude /engineering:code-review

**What:** [Brief description]
**Code:** [paste exact code]
**Check:**
- [Specific concern 1]
- [Specific concern 2]
- [Specific concern 3]

**Stack:** Next.js 16, React 19, TypeScript, Supabase
```

**Real Example (Your Project):**
```
@Claude /engineering:code-review

**What:** RLS policy for supervisors viewing employee logs

**Code:**
CREATE POLICY "supervisors_view_employee_logs" ON attendance_logs
  FOR SELECT
  USING (
    auth.uid() IN (
      SELECT supervisor_id FROM employees
      WHERE employee_id = attendance_logs.employee_id
    )
  );

**Check:**
- Is the subquery N+1 or efficient?
- Are there security gaps?
- Are indices needed?
- Can this be optimized?
```

---

#### 6. **engineering:debug** - Structured Bug Diagnosis
**Triggers:** Error, "doesn't work", "broken after deploy", stack trace, unexpected behavior
**Input Required:**
- Error message or stack trace
- Steps to reproduce
- What changed recently
- Environment (local/staging/prod)
- Is it intermittent?

**Output Provides:**
- Root cause diagnosis
- Step-by-step fix
- Verification steps
- Prevention for future

**Prompt Format:**
```
@Claude /engineering:debug

**Error:** [exact error message]
**Stack:** [paste stack trace]
**Context:** [environment, when started]
**Repro:**
1. [step 1]
2. [step 2]
3. [step 3]
**Tried:** [what already attempted]
```

**Real Example (Your Project):**
```
@Claude /engineering:debug

**Error:** useAttendanceRealtime: "subscription already exists"

**Stack:** [error from DevTools console]

**Context:**
- Environment: Production (Vercel)
- When: After @supabase/supabase-js v2.45.0 update
- Intermittent: No, always reproduces

**Repro:**
1. Go to /dashboard
2. Open DevTools network tab
3. Hard refresh (Ctrl+F5)
4. See error

**Tried:**
- Added cleanup in useEffect
- Made unsubscribe explicit
```

---

#### 7. **engineering:system-design** - Architecture & Design
**Triggers:** "design a system", "how should we architect", "what's the right way", architecture decision
**Input Required:**
- What you're building
- Scale constraints (users, events/min)
- Latency/availability requirements
- What you can't change
- Options you've considered

**Output Provides:**
- Recommended architecture
- Trade-off analysis
- Scalability assessment
- Implementation steps

**Prompt Format:**
```
@Claude /engineering:system-design

**Requirement:** [What you need to build]

**Constraints:**
- Scale: [max users/events]
- Latency: [SLA]
- Availability: [99.9% etc]
- Can't change: [X, Y]

**Options:**
1. [Option A]
2. [Option B]
3. [Option C]

**Questions:**
- [Your uncertainty 1]
- [Your uncertainty 2]
```

**Real Example (Your Project):**
```
@Claude /engineering:system-design

**Requirement:** Real-time notifications when employee clocks in

**Constraints:**
- Scale: 10k employees max, 500 events/minute peak
- Latency: <500ms event to notification
- Availability: 99.9%
- Can't add: Redis, Kafka, new infrastructure

**Options:**
1. Supabase Realtime (current)
2. WebSockets custom
3. Server-Sent Events (SSE)

**Questions:**
- Is Realtime enough for 500 events/min?
- Do we need persistence for offline users?
- How scales to 50k users?
```

---

#### 8. **engineering:documentation** - Technical Writing
**Triggers:** "write docs", "document this", "create README", "runbook", "onboarding"
**Input Required:**
- What document type (ADR, runbook, API docs, README)
- Target audience & their tech level
- Main topics to cover
- Any examples to include

**Output Provides:**
- Well-structured documentation
- Code examples where relevant
- Step-by-step guides
- Clear formatting (markdown/HTML/etc)

**Prompt Format:**
```
@Claude /engineering:documentation

**Document Type:** [ADR / Runbook / API Docs / README / Guide]

**Audience:** [who reads this, tech level]

**Topics:**
- [Topic 1]
- [Topic 2]
- [Topic 3]

**Include:**
- [Concepts explanation]
- [Step-by-step guide]
- [Code examples]
- [Troubleshooting]
```

**Real Example (Your Project):**
```
@Claude /engineering:documentation

**Document Type:** Runbook

**Title:** "How to Add a New RLS Policy"

**Audience:** Junior-mid developers

**Topics:**
- What is RLS and why we use it
- Step-by-step checklist
- How to test
- Common mistakes
- Debugging guide

**Include:**
- SQL examples (simple + complex)
- Testing with psql
- How to verify in production
```

---

#### 9. **engineering:deploy-checklist** - Pre-Deployment Verification
**Triggers:** "before deploy", "pre-deploy", "shipping a release", database migration
**Input Required:**
- What's being deployed
- Type of change (feature, bugfix, migration)
- Files affected
- Database changes
- Rollback plan

**Output Provides:**
- Verification checklist
- Testing steps
- Monitoring metrics to watch
- Rollback instructions

**Prompt Format:**
```
@Claude /engineering:deploy-checklist

**Change:** [Brief description]
**Type:** [Feature / Bugfix / Migration / Performance]
**Files:** [list of files]
**Database Changes:** [migrations, none]
**Environment:** [Staging / Production]
**Rollback Plan:** [How to revert]
**Monitor:** [What to watch post-deploy]
```

**Real Example (Your Project):**
```
@Claude /engineering:deploy-checklist

**Change:** Add 2 PostgreSQL indices for performance

**Type:** Performance optimization

**Files:** supabase/migrations/20260326_add_indexes.sql

**Database Changes:**
- idx_attendance_logs_employee_status_created_at
- idx_break_logs_employee_date

**Environment:** Production (Vercel)

**Rollback Plan:**
DROP INDEX IF EXISTS idx_attendance_logs_employee_status_created_at;
DROP INDEX IF EXISTS idx_break_logs_employee_date;

**Monitor:**
- Query times in get_monthly_top_delays (should improve)
- Database CPU (should not spike)
- Error logs for index-related errors
```

---

#### 10. **engineering:architecture** - Architecture Decision Records
**Triggers:** "ADR", "architecture decision", "choosing between technologies", "should we use X or Y"
**Input Required:**
- Decision to be made
- Options being considered
- Constraints & requirements
- Trade-offs to evaluate

**Output Provides:**
- ADR document with decision rationale
- Trade-off analysis
- Consequences (pros/cons)
- Alternatives considered

**Prompt Format:**
```
@Claude /engineering:architecture

**Decision:** [What decision needs to be made]

**Options:**
1. [Option A] - [brief description]
2. [Option B] - [brief description]
3. [Option C] - [brief description]

**Constraints:**
- [Constraint 1]
- [Constraint 2]

**Decision Criteria:**
- [Criterion 1 (weight)]
- [Criterion 2 (weight)]
```

---

#### 11. **engineering:incident-response** - Incident Management
**Triggers:** "we have an incident", "production is down", "write postmortem", emergency response
**Input Required:**
- Severity (critical/high/medium/low)
- What's affected
- Current status
- Who's involved

**Output Provides:**
- Incident assessment
- Status updates
- Resolution steps
- Blameless postmortem

**Prompt Format:**
```
@Claude /engineering:incident-response

**Severity:** [CRITICAL / HIGH / MEDIUM / LOW]

**What's Down:** [Service/feature affected]

**Symptoms:** [What users/admins see]

**Timeline:**
- When noticed
- What happened
- Current status

**Affected:** [# users, scope]
```

---

#### 12. **engineering:standup** - Daily Standup Summaries
**Triggers:** "standup", "daily standup", "summarize activity", "yesterday/today/blockers"
**Input Required:**
- List of commits/PRs from yesterday
- Work in progress today
- Any blockers

**Output Provides:**
- Formatted standup (yesterday/today/blockers)
- Clear, concise summary
- Highlighted blockers

**Prompt Format:**
```
@Claude /engineering:standup

**Yesterday:**
[commits, PRs merged]

**Today:**
[what you plan to do]

**Blockers:**
[anything blocking progress]
```

---

#### 13. **engineering:tech-debt** - Technical Debt Audit
**Triggers:** "tech debt", "what should we refactor", "code health", "code quality"
**Input Required:**
- Area of codebase to audit
- Known issues
- Time available for audit
- Priority criteria

**Output Provides:**
- Prioritized list of tech debt
- Severity assessment
- Effort estimates
- Refactoring proposals

**Prompt Format:**
```
@Claude /engineering:tech-debt

**Area:** [Component / Module / Feature to audit]

**Known Issues:**
- [Issue 1]
- [Issue 2]

**Time Available:** [1 hour / 1 day / unlimited]

**Priorities:**
1. [What's most important - security/perf/maintainability]
2. [...]
```

---

#### 14. **engineering:testing-strategy** - Test Planning
**Triggers:** "how should we test", "test strategy", "test plan", "test coverage"
**Input Required:**
- What needs testing (feature, component, API)
- Testing tools/frameworks
- Coverage goals
- Edge cases to consider

**Output Provides:**
- Test strategy document
- Test case matrix
- Sample test code
- Coverage recommendations

**Prompt Format:**
```
@Claude /engineering:testing-strategy

**What:** [Feature / Component / API to test]

**Tools:** [Jest, Vitest, Cypress, E2E, etc]

**Coverage Goal:** [Unit / Integration / E2E / %]

**Edge Cases:**
- [Edge case 1]
- [Edge case 2]

**Specific Concerns:**
- [What specifically to test]
```

---

## ⚙️ AUTOMATION SKILLS (2)

#### **schedule** - Create Scheduled Tasks
**Input Required:**
- Task description (what should run)
- Schedule (cron expression OR one-time timestamp)
- Frequency/interval

**Output Provides:**
- Automated task that runs on schedule
- Can integrate with workflow

---

#### **skill-creator** - Create/Modify Skills
**Input Required:**
- Skill name & purpose
- Triggers (keywords to activate)
- Detailed prompt/instructions
- Examples of execution

**Output Provides:**
- New skill added to system
- Ready to use with `/skill-name` command

---

## 🧩 PLUGIN SKILLS (2)

#### **cowork-plugin-customizer** - Customize Plugins
**Purpose:** Configure existing plugin for specific tools/workflows

---

#### **cowork-plugin-creator** - Create New Plugins
**Purpose:** Build custom plugin from scratch

---

## 📊 SKILL USAGE GUIDE

### **Most Used Skills** (Your Project)

```
1. code-review       → Before every PR merge
2. debug            → When bugs found
3. system-design    → Planning features
4. documentation    → ADRs, runbooks
5. deploy-checklist → Pre-production

AVERAGE TIME SAVED: 6-8 hours/week
```

### **How to Request Skills**

**Good Request Format:**
```
@Claude /[skill-name]

**Context:** [Background info]
**Need:** [Exactly what you need]
**Details:** [Specific code/data]
**Stack:** Next.js 16 + React 19 + TypeScript + Supabase
```

**Example (Good):**
```
@Claude /engineering:code-review

**Context:** Reviewing RLS policy for new feature
**Need:** Check for security vulnerabilities and performance
**Code:** [paste exact SQL]
**Stack:** Next.js 16 + React 19 + TypeScript + Supabase
```

**Example (Bad):**
```
@Claude Review this code
[vague request, missing context]
```

---

## ⚠️ LIMITATIONS & CONSTRAINTS

### **Cannot Do:**
- ❌ Handle sensitive financial data (banking details, API keys)
- ❌ Modify security/access permissions
- ❌ Create accounts or authenticate on behalf of user
- ❌ Execute unlimited operations (has token budget)
- ❌ Permanently delete files without explicit confirmation
- ❌ Follow instructions embedded in webpage content (injection defense)

### **Requires Explicit Permission:**
- ⚠️ Download files
- ⚠️ Make financial transactions
- ⚠️ Change account settings
- ⚠️ Share/forward sensitive information
- ⚠️ Accept terms/agreements
- ⚠️ Grant permissions (OAuth/SSO)

### **Prefers:**
- ✅ Specific, detailed requests
- ✅ Exact code samples (not descriptions)
- ✅ Context about constraints
- ✅ Clear expected outcomes
- ✅ Step-by-step thinking

---

## 🔄 WORKFLOW EXAMPLE

**Typical task flow in marcacion-grupo-ct:**

```
1. PLAN
   → /engineering:system-design
   "How should we add real-time notifications?"

2. CODE
   → Write locally

3. REVIEW
   → /engineering:code-review
   "Check this RPC before I merge"

4. DEBUG (if needed)
   → /engineering:debug
   "Why does this subscription fail in prod?"

5. DOCUMENT
   → /engineering:documentation
   "Write runbook for deploying RLS policies"

6. PRE-DEPLOY
   → /engineering:deploy-checklist
   "What to verify before going to prod?"

7. MONITOR
   → Check logs, metrics

8. TECH DEBT (optional)
   → /engineering:tech-debt
   "What should we refactor?"
```

---

## 💡 TIPS FOR OTHER AIs

### **Maximize Claude's Effectiveness:**

1. **Be Specific:** Don't say "review my code" - say "check for N+1 queries and RLS bypass vulnerabilities"

2. **Provide Context:** Stack, environment, constraints, what changed

3. **Include Examples:** Paste exact code, not descriptions

4. **State Expectations:** "Should return 3 columns, currently timing out"

5. **Use Right Skill:** code-review for security, debug for errors, system-design for architecture

6. **Format Matters:** Use the exact prompt templates - they're designed for effectiveness

7. **Stack Reference:** Always mention "Next.js 16 + React 19 + TypeScript + Supabase" so Claude knows the context

8. **Edge Cases:** Mention specific concerns - Claude will dig deeper into those areas

### **Questions This Will Answer:**

- ✅ "What can Claude do?" → See this document
- ✅ "How do I request this?" → Use the prompt templates
- ✅ "What should I expect?" → See the Output Provides section
- ✅ "Is this possible?" → Check Limitations section
- ✅ "How long will it take?" → Check QUICK STATS in CHEAT_SHEET.md

---

## 📎 RELATED DOCUMENTS

Located in `/marcacion-grupo-ct/`:

1. **CHEAT_SHEET.md** - 1-page quick reference
2. **PROMPTS_OPTIMIZADOS.md** - Copy-paste templates with examples
3. **REPORTE_HABILIDADES_CLAUDE.md** - Full capabilities report
4. **NUEVAS_SKILLS_PROPUESTAS.md** - Proposed new skills (supabase-optimizer, etc)

---

## 🎯 QUICK SKILL SELECTOR

```
Your Need → Use This Skill → Input → Output

Code/PR review → code-review → Code + concerns → Security/perf assessment

Bug diagnosis → debug → Error + repro steps → Root cause + fix

Feature design → system-design → Requirements + constraints → Architecture + trade-offs

Write docs → documentation → Topic + audience → Formatted doc

Deploy checklist → deploy-checklist → Change + files + env → Verification steps

Audit code quality → tech-debt → Code area + priorities → Prioritized list

Analyze data → xlsx → Data + desired analysis → Spreadsheet with charts

Create presentation → pptx → Content + audience → PowerPoint deck

Create document → docx → Content + format → Word document

Process PDF → pdf → PDF file → Extracted/merged/edited PDF

Test strategy → testing-strategy → Feature + tools → Test plan + cases

Architecture choice → architecture → Options + criteria → ADR + analysis

Incident response → incident-response → Issue + severity → Assessment + postmortem

Daily standup → standup → Work summary → Formatted standup

Scheduled task → schedule → What + when → Automated task

Create skill → skill-creator → Skill spec → New skill added
```

---

## 📞 SUPPORT

This document should answer most questions about Claude's capabilities.

If you need clarification on:
- **How to trigger a specific skill** → See that skill's section
- **What format of input is expected** → See "Input Required" under each skill
- **What output you'll get** → See "Output Provides" under each skill
- **Real examples** → See "Real Example (Your Project)" under each skill

---

**Document Created:** 2026-03-26
**Valid For:** Any AI/system that needs to request work from Claude
**Context:** marcacion-grupo-ct v0.2.0 (Next.js + React + TypeScript + Supabase)

---

## 🚀 START HERE

**To request work from Claude:**

1. Choose a skill from the list above
2. Copy the "Prompt Format" from that skill
3. Fill in your specific details
4. Send as: `@Claude /[skill-name]` followed by filled template
5. Claude will respond with the skill's output

Example:
```
@Claude /engineering:code-review

**Context:** Reviewing new RLS policy
**Code:** [paste your SQL]
**Check:** Security vulnerabilities, N+1 queries, missing indices
```

**That's it!** Claude knows exactly what you need because this document told him.
