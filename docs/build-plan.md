# AI Legal Portal — Team Setup & A–Z Claude Code Build Plan

Oct 23, 2026 · @Navtcc

## How to use this document

The three of you will build the AI-Powered Legal Information Portal (Master Document v1.1) in 8 weeks, using Claude Code on each laptop and one shared GitHub repository. Work through this document top to bottom: set up once (Parts A–B), agree how you work (Part C), then run the 26 prompts in Part E in the order and by the owner shown in Part D.

- **Part A** — install the same tools on all three Windows laptops.  
- **Part B** — create the GitHub repo, branches, protection rules and project board (one person does this, the others join).  
- **Part C** — who owns what, and the daily Git routine so you never overwrite each other's work.  
- **Part D** — the 8-week roadmap mapping every prompt to a week and an owner.  
- **Part E** — prompts A to Z, copy-paste ready for Claude Code.  
- **Part F** — testing, deployment, demo and submission checklist.

**Names used here.** Member A \= AI/RAG, Member B \= Backend/Data, Member C \= Frontend, exactly as in Section 20 of the master document. Replace with your real names once.

**Stack fixed by the master document:** Next.js \+ TypeScript, Python \+ FastAPI, PostgreSQL (with pgvector), SQLAlchemy \+ Alembic, pytest, browser speech APIs. This plan adds only what is needed to make it run: Docker for the database, a local multilingual embedding model, and a swappable LLM provider so the choice of model stays open as Section 11 requires.

**Golden rule for every prompt:** the master document is the source of truth. Copy it into the repo as `docs/master-document.md` (Prompt A does this) so Claude Code can read the requirements (FR-01 to FR-32) every time.

## Part A — Laptop environment setup (all 3 members)

Every member installs the same versions so code that runs on one laptop runs on all three. Do this on Day 1, together on a call, and tick each box.

### A1. Minimum laptop

- Windows 10/11, 8 GB RAM (16 GB better, because the embedding model and Docker share memory), 20 GB free disk.  
- Admin rights to install software; a stable internet connection for the first setup.

### A2. Install the tools

Open **PowerShell as Administrator** and run these one by one:

```powershell
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id Python.Python.3.12 -e
winget install --id Microsoft.VisualStudioCode -e
winget install --id Docker.DockerDesktop -e
winget install --id astral-sh.uv -e
```

&nbsp;

Close and reopen PowerShell, then check versions. Everyone should see the same major versions:

```powershell
git --version
gh --version
node -v
npm -v
python --version
docker --version
uv --version
```

&nbsp;

Notes:

- **Python:** during install, tick "Add python.exe to PATH". If `python` opens the Microsoft Store, turn off the alias in Settings → Apps → Advanced app settings → App execution aliases.  
- **Docker Desktop:** start it once, accept WSL 2 when asked, and restart the laptop. The database runs in Docker so nobody has to install PostgreSQL by hand.  
- **No Docker possible?** Install PostgreSQL 16 natively (`winget install PostgreSQL.PostgreSQL.16`) and add the pgvector extension. Only use this fallback if Docker will not run.

### A3. Configure Git (each person, with their own details)

```powershell
git config --global user.name "Your Full Name"
git config --global user.email "your-github-email@example.com"
git config --global init.defaultBranch main
git config --global core.autocrlf true
git config --global pull.rebase false
```

&nbsp;

Then sign in to GitHub from the terminal (choose HTTPS and "Login with a web browser"):

```powershell
gh auth login
```

&nbsp;

### A4. Install Claude Code

Claude Code needs a Claude account with a paid plan (Pro or above) or Anthropic API credits. Decide as a team how many seats to buy; Section 21 of the master document caps spending at PKR 10,000, so check current prices before paying and record the expense.

```powershell
irm https://claude.ai/install.ps1 | iex
claude --version
```

&nbsp;

Run `claude` once inside any folder and sign in. You can also use the Claude desktop app (Code tab) or the VS Code extension; they all read the same `CLAUDE.md` file in the repo.

### A5. VS Code extensions

```powershell
code --install-extension ms-python.python
code --install-extension charliermarsh.ruff
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension bradlc.vscode-tailwindcss
code --install-extension ms-azuretools.vscode-docker
code --install-extension eamodio.gitlens
```

&nbsp;

### A6. Accounts and keys

- A GitHub account for each member.  
- One LLM API key for development, kept by one person and shared privately (never in chat groups, never in Git). Each member puts it in their own `backend/.env` file.  
- Agree a project folder path, for example `D:\Projects\ai-legal-portal`. Avoid paths with spaces or inside OneDrive, which slow down `node_modules` and Git.

### A7. Setup checklist

- [ ] Member A: all tools installed and versions match  
- [ ] Member B: all tools installed and versions match  
- [ ] Member C: all tools installed and versions match  
- [ ] Docker Desktop starts on all three laptops  
- [ ] `gh auth status` shows logged in on all three laptops  
- [ ] Claude Code opens and is signed in on all three laptops

## Part B — GitHub setup (one repo, three people)

Member B creates the repository once; A and C accept the invite and clone it. Everyone works on short-lived feature branches and merges into `develop` through pull requests; `main` only receives tested releases.

### B1. Create the organization and repository (Member B)

1. On github.com create a free **Organization** (for example `ai-legal-portal-fyp`). An organization keeps the repo independent of any one student's account. Invite Members A and C as **Owners**, and your supervisor as a read-only member if they want access.  
2. Create the repository from the terminal:

```powershell
cd D:\Projects
gh repo create ai-legal-portal-fyp/ai-legal-portal --private --clone --add-readme --gitignore Python
cd ai-legal-portal
git checkout -b develop
git push -u origin develop
```

&nbsp;

3. In the repo's **Settings → General**, set the default branch to `develop`, and under Pull Requests enable "Automatically delete head branches" and "Allow squash merging" only.

### B2. Protect the branches (Member B)

In **Settings → Branches → Add branch ruleset** create one rule for `main` and one for `develop`:

- Require a pull request before merging, with **1 approving review**.  
- Require status checks to pass (add the `ci` check after Prompt A creates it).  
- Block force pushes and deletions.  
- For `main` only: restrict who can merge to the person doing the weekly release.

### B3. Labels, milestones and the project board

Create one milestone per week and labels for owner and area:

```powershell
1..8 | ForEach-Object { gh api repos/ai-legal-portal-fyp/ai-legal-portal/milestones -f title="Week $_" }
gh label create "owner:A-ai" --color 7057ff
gh label create "owner:B-backend" --color 0e8a16
gh label create "owner:C-frontend" --color 1d76db
gh label create "area:rag"; gh label create "area:api"; gh label create "area:ui"
gh label create "area:voice"; gh label create "area:lawyers"; gh label create "area:eval"
gh label create "stretch" --color cccccc
```

&nbsp;

Then create a **Project** (Projects → New → Board) with columns Backlog, This week, In progress, In review, Done. Make one issue per prompt in Part E, titled like `[Prompt G] PDF ingestion and chunking`, with its owner label and week milestone.

### B4. Each member clones the repo (A and C)

```powershell
cd D:\Projects
gh repo clone ai-legal-portal-fyp/ai-legal-portal
cd ai-legal-portal
git checkout develop
```

&nbsp;

### B5. Secrets: what never goes to GitHub

- `.env` files, API keys, database passwords, real client documents, lawyer verification uploads.  
- Commit `backend/.env.example` and `frontend/.env.example` with placeholder values instead; each member copies them to `.env` locally.  
- For GitHub Actions, add keys under **Settings → Secrets and variables → Actions**. CI should use the mock LLM provider so it never needs a real key.  
- If a key is ever pushed by mistake, revoke it with the provider immediately; deleting the commit is not enough.

### B6. Pull request rules

1. One issue \= one branch \= one PR. Branch names: `feat/G-pdf-ingestion`, `fix/chat-citation-404`, `docs/readme-setup`.  
2. Keep PRs small (ideally under 400 changed lines) and open them as Draft early.  
3. The PR description links the issue (`Closes #12`), says how it was tested, and includes a screenshot for UI changes.  
4. Another member reviews and approves; the author merges with **Squash and merge**.  
5. CI (lint \+ tests) must be green before merging.

## Part C — Team roles and daily workflow

Each member owns one folder of the repo, so most days your Claude Code sessions never edit the same files. Shared files (API schemas, migrations, `CLAUDE.md`) change only through a PR that the owner reviews.

### C1. Ownership

| Member | Owns these folders | Main prompts | Reviews PRs from |
| :---- | :---- | :---- | :---- |
| A — AI/RAG | `backend/app/ingestion/`, `backend/app/retrieval/`, `backend/app/rag/`, `backend/app/classify/`, `evaluation/`, `data/` | G, H, J, K, L, N, O, X, Y | B |
| B — Backend/Data | `backend/app/api/`, `backend/app/models/`, `backend/alembic/`, `backend/app/auth/`, `docker-compose.yml`, `.github/` | A, B, C, D, F, Q, S, W | A and C |
| C — Frontend | `frontend/` (all of it) | E, I, M, P, R, T, U, V | B |
| All | `docs/`, `README.md`, `CLAUDE.md` | Z | anyone |

The API contract is the meeting point: B writes Pydantic schemas, FastAPI publishes them at `/openapi.json`, and C generates TypeScript types from that file. Agree a schema before either side builds on it.

### C2. The daily Git routine (every member, every day)

```powershell
# 1. Start of day: get everyone's merged work
git checkout develop
git pull

# 2. Start a task on its own branch
git checkout -b feat/G-pdf-ingestion

# 3. Work with Claude Code, commit small steps
git add -A
git commit -m "feat(ingestion): extract text per page with pdfplumber"

# 4. Before pushing, bring in the latest develop and re-run tests
git fetch origin
git merge origin/develop
git push -u origin feat/G-pdf-ingestion

# 5. Open the PR
gh pr create --base develop --fill --draft
```

&nbsp;

Commit messages follow `type(area): what changed`, with types `feat`, `fix`, `test`, `docs`, `refactor`, `chore`.

### C3. Rules for using Claude Code as a team

1. **Always start Claude Code from the repo root**, so it reads `CLAUDE.md` and the master document.  
2. **Plan first.** For every prompt in Part E, start in plan mode (Shift+Tab in the terminal), read the plan, correct it, then let it build.  
3. **One prompt per branch.** Do not run two big prompts in one branch; it makes PRs impossible to review.  
4. **Ask it to run the tests** before you commit, and read the diff yourself. You must be able to explain every file at the viva.  
5. **Never paste secrets or real client data** into a prompt.  
6. **When Claude Code learns a project rule** (for example "use `uv` not `pip`"), add it to `CLAUDE.md` in a PR so all three laptops get it.  
7. **Merge conflicts:** ask Claude Code "resolve the merge conflicts with develop, keep both sides' intent, then run the tests" — and review the result.

### C4. Weekly rhythm (from Section 19.1)

- **Monday (30 min call):** move this week's issues to "This week", confirm owners and acceptance criteria.  
- **Wednesday:** integrate one vertical slice on `develop` (for example upload → search working end to end).  
- **Friday:** demo the running build to each other, run all tests, merge `develop` into `main` with a tag (`v0.1-week1` … `v0.8-week8`), update issues.

## Part D — Eight-week roadmap with owners

The plan follows Section 19 of the master document: backend and frontend skeletons by Week 2, a cited chatbot end to end by Week 4, all features by Week 6, evaluation in Week 7, and the release in Week 8\.

\[embedded content: 8-week roadmap · 3 owners, 26 prompts, 3 gates\]

Grey cells are non-prompt work: preparing data (A), reviewing PRs (B), fixing bugs and running the user test (C). If Week 4's gate slips, protect the core path in Section 19.2 and push Y, profile Steps 4/5/6/8 and any backend speech upgrades to stretch.

**Exit criterion for each week** (from Section 19):

1. **Week 1:** environment and GitHub ready on all 3 laptops (Parts A–B), Prompt A merged, sample corpus chosen, supervisor questions from Appendix A sent.  
2. **Week 2:** frontend reaches the API, migrations run, sign-in works.  
3. **Week 3:** the sample corpus is searchable and extraction failures are visible.  
4. **Week 4:** chatbot answers with citations that open the source page; out-of-corpus questions abstain. Forum guide sent for legal review.  
5. **Week 5:** summaries, category and forum card, lawyer wizard (Steps 1, 2, 3, 7\) and the verification queue work on demo data.  
6. **Week 6:** voice in and read-aloud work; security tests pass; admin dashboard live.  
7. **Week 7:** evaluation report with real numbers; usability test done; high-priority bugs fixed.  
8. **Week 8:** scope frozen, `v1.0` tagged, demo rehearsed, report and slides submitted.

## Part E — A–Z Claude Code prompts

Run each prompt in a fresh Claude Code session from the repo root, on its own branch, in plan mode first. Each prompt names its owner, week, the requirements it covers, and how you know it is done. Paste the whole code block.

**How to run one prompt**

1. `git checkout develop` → `git pull` → `git checkout -b feat/<letter>-<short-name>`.  
2. Start `claude`, press Shift+Tab until it shows plan mode, paste the prompt.  
3. Read the plan. Reply with corrections. Approve.  
4. When it finishes, ask: "Run all backend and frontend tests and the linters, fix any failures, and show me a summary of what changed."  
5. Check the "Done when" list yourself, commit, push, open a PR, and ask a teammate to review.

**If a prompt is too big for one session**, tell Claude Code: "Do only steps 1–3 now and stop." Then continue in a second session with "Continue Prompt X from step 4."

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

### Prompt A — Repository bootstrap and CLAUDE.md

Owner: B · Week 1 · Before starting: copy `AI_Legal_Portal_FYP_Master_Document_v1.1.docx` into a new `docs/` folder and install pandoc (`winget install JohnMacFarlane.Pandoc`).

```
You are setting up the monorepo for our Final Year Project "AI-Powered Legal Information Portal". Three students will work on it on Windows laptops through GitHub.

1. Convert docs/AI_Legal_Portal_FYP_Master_Document_v1.1.docx to docs/master-document.md with pandoc. Keep the .docx too. This markdown file is the source of truth for all requirements.
2. Create this structure:
   - backend/  (Python 3.12, FastAPI, managed with uv, pyproject.toml)
   - frontend/ (Next.js App Router + TypeScript + Tailwind, created later in Prompt E; just add a placeholder README now)
   - evaluation/ (scripts and datasets for offline evaluation)
   - data/ (README explaining that real documents are NOT committed; data/samples/ for small public or synthetic PDFs)
   - docs/ (master document, architecture notes, ADRs in docs/adr/)
   - .github/workflows/ci.yml
3. docker-compose.yml with a PostgreSQL 16 service using the pgvector/pgvector:pg16 image, a named volume, port 5432, and a healthcheck. Credentials come from a root .env file; add .env.example.
4. A root .gitignore covering Python, Node, .env files, uploaded files (storage/), model caches, and data/private/.
5. A CLAUDE.md at the root that tells any Claude Code session:
   - project summary in 5 lines and a pointer to docs/master-document.md
   - folder ownership (A: ingestion/retrieval/rag/classify/evaluation; B: api/models/auth/alembic/infra; C: frontend)
   - commands to run backend, frontend, tests, linters, and migrations
   - rules: never commit secrets or real client data; enforce authorization in the backend, never only in the UI; citations must map to real chunk IDs; keep the LLM and embedding provider swappable; write tests with every feature; reference requirement IDs (FR-xx, F-xx) in code comments only where helpful
   - Git rules: feature branches from develop, small PRs, conventional commits
6. A root README.md with prerequisites (Git, Node LTS, Python 3.12, uv, Docker Desktop) and step-by-step Windows setup commands.
7. A GitHub Actions workflow named ci with two jobs: backend (uv sync, ruff, pytest against a pgvector service container) and frontend (npm ci, lint, typecheck, test), each skipped gracefully if its folder has no code yet.
8. A pull request template and issue templates (feature, bug).

Do not build any features yet. Show me the final tree when done.
```

Done when: `docker compose up -d` starts the database, CI runs green on a PR, and A and C can clone and follow the README.

### Prompt B — Backend skeleton and health check

Owner: B · Week 2 · Covers: GET /health, NFR Maintainability, Portability

```
Read CLAUDE.md and sections 10, 11 and 15 of docs/master-document.md.

Build the FastAPI backend skeleton in backend/:
1. Layout: app/main.py, app/core/config.py (pydantic-settings reading backend/.env), app/core/logging.py, app/db/session.py (SQLAlchemy 2.0, sync engine is fine), app/api/ (routers), app/models/, app/schemas/, app/services/, tests/.
2. Settings: DATABASE_URL, CORS_ORIGINS, JWT_SECRET, LLM_PROVIDER (default "mock"), LLM_MODEL, LLM_API_KEY, EMBEDDING_MODEL, MAX_UPLOAD_MB (default 20), STORAGE_DIR. Create backend/.env.example with safe placeholders.
3. GET /health returning status, app version, and whether the database is reachable.
4. CORS for http://localhost:3000.
5. A consistent error response shape {"error": {"code", "message", "details"}} with exception handlers.
6. Dev tools: ruff, pytest, pytest-cov, httpx. Add uv scripts or a Makefile-like tasks file that works on Windows (use a tasks.py or just document the commands).
7. Tests for /health (database up and down).

Run the tests and show me how to start the server.
```

Done when: `uv run uvicorn app.main:app --reload` serves [http\://localhost:8000/docs](http://localhost:8000/docs) and `/health` shows the database as reachable.

### Prompt C — Database models and migrations

Owner: B · Week 2 · Covers: Section 14 (all entities), FR-01, FR-10

```
Read section 14 (Database Design) and 5.4, 5.5 of docs/master-document.md.

1. Enable the pgvector extension in the first Alembic migration.
2. Create SQLAlchemy models and an Alembic migration for every entity in section 14: users, documents, document_versions, chunks, permissions, query_logs, evaluation_cases, lawyer_profiles, lawyer_specializations, lawyer_education, lawyer_experience, lawyer_verifications, lawyer_documents, legal_categories, forums, category_forum_map.
3. Decisions to apply:
   - UUID primary keys, created_at/updated_at on every table.
   - users.role is an enum: visitor, registered, lawyer, admin.
   - documents.access_level enum: public, restricted. documents.collection enum: public_corpus, case_file, forum_guide, synthetic.
   - document status enum: uploaded, extracting, extracted, failed, indexed.
   - chunks: page_start, page_end, chunk_index, chunk_text, chunk_hash, language, embedding vector(384) nullable, and a generated tsvector column with a GIN index for full-text search (use the 'simple' text search config because the corpus mixes Urdu and English).
   - lawyer_profiles.field_visibility as JSONB; verification_status enum: self_reported, pending, verified, rejected.
   - category_forum_map.conditions as JSONB (for province, amount, land type, employer type).
4. Add a seed script (backend/app/db/seed.py) that creates one admin user from environment variables and the 13 legal categories from section 5.5 with keywords (for example khula, mehar, FIR, eviction, intiqal).
5. Write tests that run the migrations on a fresh database and check the relationships in 14.1.
6. Draw the ERD as a Mermaid diagram in docs/erd.md.

Never store passwords in plain text. Show me the migration file before applying it.
```

Done when: `uv run alembic upgrade head` builds every table on a fresh database and the seed creates the admin and 13 categories.

### Prompt D — Authentication and role-based access

Owner: B · Week 2 · Covers: F-08, FR-09, Section 6 roles, Section 17

```
Read sections 6 and 17 of docs/master-document.md.

Implement authentication and authorization in the backend:
1. POST /api/auth/register (registered user or lawyer), POST /api/auth/login, POST /api/auth/logout, GET /api/auth/me.
2. Hash passwords with argon2 (pwdlib). Issue a short-lived JWT in an httpOnly, SameSite=Lax cookie. Visitors need no account.
3. FastAPI dependencies: get_current_user_optional, require_user, require_role("lawyer"), require_role("admin").
4. A single access helper can_access_document(user, document) used by every endpoint and by retrieval later. Restricted documents are visible only to admins or users with a row in permissions that has not expired.
5. Basic rate limiting on login and register (slowapi).
6. Tests: wrong password, expired token, a registered user cannot reach admin routes, a visitor cannot read a restricted document, a user with a valid permission can.

Authorization must live in the backend; the frontend only hides buttons.
```

Done when: all access-control tests pass and `/docs` shows the auth endpoints with lock icons on protected routes.

### Prompt E — Frontend skeleton and API client

Owner: C · Week 2 · Covers: Section 16 landing page, NFR Usability, Accessibility

```
Read sections 16 and 8 of docs/master-document.md and CLAUDE.md.

Create the Next.js app in frontend/:
1. Next.js (App Router) + TypeScript + Tailwind CSS + shadcn/ui. ESLint, Prettier, Vitest + React Testing Library, and Playwright for end-to-end smoke tests.
2. Generate TypeScript types from http://localhost:8000/openapi.json with openapi-typescript into src/lib/api/types.ts, and a typed fetch client (src/lib/api/client.ts) that sends cookies and maps the backend error shape to a user-friendly message. Add an npm script "gen:api".
3. Layout: header with navigation (Search, Ask, Lawyers, Sign in), an English/Urdu language toggle that switches labels and sets dir="rtl" for Urdu, and a footer with the disclaimer from section 17: informational support only, not legal advice, sources may be incomplete or outdated.
4. Landing page: project explanation, a search box, links to Search and Ask, limitations notice.
5. Sign in and register pages wired to /api/auth.
6. A status badge that calls /health so we can see the frontend reaches the API.
7. Accessible defaults: keyboard focus styles, labels on every input, sufficient contrast. Use a font that supports Urdu (for example Noto Nastaliq Urdu for Urdu text).
8. frontend/.env.example with NEXT_PUBLIC_API_URL.

Add a Vitest test for the API client and a Playwright test that loads the landing page.
```

Done when: `npm run dev` shows the landing page, the health badge is green, and you can register and sign in.

### Prompt F — Document catalogue and upload

Owner: B · Week 3 · Covers: F-01, F-02 (upload part), FR-01, FR-13, UC-04, Section 17 file rules

```
Read F-01, F-02, UC-04, section 12.2 and section 17 of docs/master-document.md.

Implement the document catalogue API:
1. POST /api/documents/upload (admin only): multipart PDF + metadata (title, document_type, jurisdiction, language, date, source_name, source_url, access_level, collection, permission_note). Validate extension, MIME type (check magic bytes, not the client header), and size against MAX_UPLOAD_MB. Compute a SHA-256 checksum and reject duplicates.
2. Store files under STORAGE_DIR with a generated name; never expose raw storage paths. Create a document and a document_version row with status "uploaded".
3. GET /api/documents with filters (type, jurisdiction, language, collection) and pagination; it must apply can_access_document. GET /api/documents/{id}. PATCH /api/documents/{id} for metadata (admin). DELETE as a soft delete that also removes the document from search (admin), to support correction and permission withdrawal.
4. GET /api/documents/{id}/file streams the PDF only if the user may access it.
5. After upload, call an ingestion service interface (ingest_document_version(version_id)) that Member A will implement in Prompt G; for now a stub that just sets status.
6. Clear errors for unsupported files, oversize files, and duplicates (FR-13).
7. Tests for every validation rule and access rule.
```

Done when: an admin can upload a PDF through `/docs`, a visitor cannot see restricted documents, and invalid files return clear errors.

### Prompt G — PDF ingestion, extraction and chunking

Owner: A · Week 3 · Covers: F-02, FR-02, FR-03, FR-13, Section 12.2, RQ3 · Before starting: put 5–10 approved public PDFs (English and Urdu, at least one scanned) in `data/samples/`.

```
Read F-02, FR-02, FR-03, section 12.2 and 13.1 of docs/master-document.md. Implement backend/app/ingestion/.

1. Write a small comparison script (evaluation/extraction_compare.py) that runs pypdf, pdfplumber and PyMuPDF on every PDF in data/samples/ and reports per file: pages, characters per page, empty pages, and whether Urdu text comes out readable (count Arabic-script characters). Save the result as evaluation/results/extraction_compare.md. Recommend one library and record the decision in docs/adr/001-pdf-extraction.md. Do not implement OCR (it is a stretch feature); detect scanned pages instead.
2. Implement ingest_document_version(version_id) with the chosen library:
   - extract text page by page, keep page numbers
   - quality checks: empty pages, very low characters per page (likely scanned), broken encoding; store extracted_text_status and warnings
   - normalize whitespace only, never change legal wording; keep the raw extracted text as well
   - detect language per page (Urdu vs English) with a simple script-ratio heuristic
3. Chunking: split by headings/sections where detectable, otherwise ~800 characters with ~150 overlap, never across documents; store page_start, page_end, chunk_index, chunk_hash; skip duplicate chunks by hash.
4. Status flow: uploaded -> extracting -> extracted -> indexed, or failed with a readable reason. Failed or low-quality extraction must never be silently indexed.
5. Run ingestion in a FastAPI BackgroundTask so upload returns quickly; GET /api/documents/{id} shows status and warnings.
6. A CLI: uv run python -m app.ingestion.cli ingest data/samples/ --collection public_corpus for bulk loading.
7. Unit tests: page mapping, chunk overlap, duplicate chunk skipping, a scanned PDF flagged as low quality, an empty PDF failing clearly.
```

Done when: the sample PDFs ingest, each chunk shows its page range, and the scanned PDF is flagged instead of indexed.

### Prompt H — Keyword search API (baseline)

Owner: A (with B reviewing the endpoint) · Week 3 · Covers: F-03, FR-04, UC-01, Section 13.2 baseline

```
Read F-03, FR-04, UC-01 and 13.2 of docs/master-document.md.

1. In backend/app/retrieval/keyword.py implement keyword search over chunks using the tsvector column (websearch_to_tsquery with the 'simple' config) and ts_rank, returning chunk id, document id, title, document_type, date, page range, and a highlighted snippet (ts_headline).
2. Filter by the user's permitted collections BEFORE ranking, using can_access_document logic in SQL (never filter after the fact). Exclude soft-deleted and non-indexed documents.
3. POST /api/search with {query, filters, page, page_size}. Validate query length. Return a clear empty result with suggestions when nothing matches.
4. Design retrieval behind an interface Retriever.search(query, user, k, filters) -> list[RetrievedChunk] so semantic and hybrid retrievers can plug in later (Prompt J).
5. Tests: relevant chunk ranks first for an exact phrase; restricted chunks never appear for visitors; empty query is rejected.
```

Done when: searching a phrase from a sample PDF returns that page as the top snippet, and restricted documents never appear for visitors.

### Prompt I — Search UI and document detail page

Owner: C · Week 3 · Covers: F-03, UC-01, Section 16 search and document detail screens

```
Read UC-01 and the Legal document search and Document detail rows in section 16 of docs/master-document.md. Run npm run gen:api first.

1. /search page: search box, filters (document type, jurisdiction, language, date range), result cards with title, type, date, source, page range and the highlighted snippet, pagination, loading and empty states with refinement tips.
2. /documents/[id] page: metadata, source link, extraction status, extracted text grouped by page with page anchors (#page-12), an "Open original PDF" button when allowed, and a "Summarize" button (wired later in Prompt N).
3. Clicking a result opens the document detail at the matching page anchor and highlights the snippet.
4. Show the corpus coverage note: results come only from documents in this portal, not all Pakistani law.
5. Urdu results render right-to-left with the Urdu font.
6. Component tests for result cards and a Playwright test: search -> open result -> page anchor visible.
```

Done when: you can search, click a result, and land on the right page of the document.

### Prompt J — Embeddings and hybrid retrieval

Owner: A · Week 4 · Covers: Section 13.2, RQ1, NFR Maintainability (swappable model)

```
Read sections 11 (Embeddings row), 13.2 and 10.1 of docs/master-document.md.

1. backend/app/retrieval/embeddings.py: an EmbeddingProvider interface with a local implementation using sentence-transformers and the model in EMBEDDING_MODEL (default intfloat/multilingual-e5-small, 384 dims; use the "query: " and "passage: " prefixes this model expects). Load the model once and cache it. Make the dimension configurable so the model can be swapped.
2. Compute embeddings for chunks during ingestion (batched) and add a backfill CLI command for existing chunks. Mark the document indexed only after embeddings exist.
3. Add an HNSW index on chunks.embedding (cosine).
4. SemanticRetriever and HybridRetriever (keyword + semantic combined with Reciprocal Rank Fusion, k=60). Same permission filter as Prompt H, applied inside the SQL query. Choose the retriever with RETRIEVER=keyword|semantic|hybrid.
5. Return a relevance score per chunk so the RAG layer can apply an evidence threshold.
6. Tests with a tiny fixture corpus: a paraphrased query (no shared keywords) is found by semantic but not keyword; permissions still apply.

Do not send document text to any external service for embeddings.
```

Done when: a paraphrased Urdu or English question finds the right passage with `RETRIEVER=hybrid`.

### Prompt K — LLM provider layer

Owner: A · Week 4 · Covers: Section 10.1 (replace the LLM without rewriting), Section 11 LLM row, Section 21 budget, Risk "Cost or rate limits"

```
Read sections 10.1, 11, 21 and 22 of docs/master-document.md.

Create backend/app/rag/llm.py:
1. An LLMProvider interface: generate(system, messages, max_tokens, temperature, json_schema=None) -> LLMResult(text, parsed_json, input_tokens, output_tokens, latency_ms, model).
2. Implementations selected by LLM_PROVIDER:
   - mock: deterministic answers for tests and CI (no network)
   - anthropic: uses the official anthropic Python SDK with LLM_MODEL
   - openai_compatible: uses the openai SDK with a configurable base_url, so we can try any provider that offers an OpenAI-compatible endpoint, or a local Ollama server
3. Timeouts, one retry with backoff, and a clear ServiceUnavailable error when the provider fails (FR-13, NFR Reliability).
4. A simple daily token budget (LLM_DAILY_TOKEN_LIMIT) tracked in the database; when exceeded, return a friendly "service limit reached" message instead of calling the API.
5. Log model, tokens and latency to query_logs, never the full question or answer text.
6. A script evaluation/llm_pilot.py that sends 10 Urdu and 10 English test prompts to the configured provider and saves latency and token counts, so we can compare providers before choosing (section 11 says the choice stays open until a pilot).
7. Tests use the mock provider only.
```

Done when: switching `LLM_PROVIDER` between `mock` and a real provider needs no code change, and CI passes without any API key.

### Prompt L — RAG chat endpoint with citations and abstention

Owner: A · Week 4 · Covers: F-04, F-05, F-06, FR-05, FR-06, FR-07, UC-02, Sections 13.1 and 13.6, RQ2

```
Read F-04, F-05, F-06, FR-05..07, UC-02, 13.1 and 13.6 of docs/master-document.md.

Implement POST /api/chat in backend/app/rag/:
1. Request: {question, conversation_id?, language?: "ur"|"en"|"auto"}. Detect language when auto.
2. Pipeline exactly as section 13.1: scope check (permitted collections) -> retrieve top k (hybrid) -> deduplicate and keep the best 5-8 chunks -> evidence check -> generate -> validate citations -> respond.
3. Evidence check: if the best relevance score is below RAG_MIN_SCORE (configurable; we will tune it in Prompt X) or no chunks are found, do NOT call the LLM. Return an abstention: the corpus does not contain enough support, suggest rephrasing, and recommend a qualified lawyer for case-specific decisions.
4. Prompt the model with numbered source passages [S1]..[Sn] including title, date and pages. Instructions: answer only from the sources, cite every legal claim with [S#], say what is uncertain, never invent statute names or section numbers, answer in the user's language. Ask for JSON: {answer, citations:[{source, quote}], insufficient_evidence: bool}.
5. Citation validation: every [S#] must map to a chunk that was actually provided, and each quoted text must appear in that chunk (normalized, fuzzy match allowed). Drop invalid citations; if no valid citation remains, return the abstention instead of the answer.
6. Response: {answer, citations:[{chunk_id, document_id, title, page_start, page_end, date, snippet}], abstained, disclaimer, language}.
7. Keep the last few turns of the conversation for follow-up questions, but retrieve fresh evidence every turn.
8. Tests with the mock LLM: valid citations pass; a hallucinated [S9] is removed; an out-of-scope question abstains without calling the LLM; restricted chunks never reach the prompt for a visitor.
```

Done when: a question from your sample corpus returns an answer whose every citation opens the right page, and "What is the penalty for X" outside the corpus abstains.

### Prompt M — Chat UI with citation cards

Owner: C · Week 4 · Covers: UC-02, F-05, F-06, Section 16 chat interface, POST /api/feedback

```
Read UC-02 and the Chat interface row of section 16 in docs/master-document.md. Run npm run gen:api.

1. /ask page: a conversation view, a multi-line question box (Enter sends, Shift+Enter new line), a send button, and space reserved next to the input for a microphone button (Prompt T) and under each answer for a speaker button (Prompt U) and a category card (Prompt P).
2. Answer bubble: answer text with inline [S1] markers that are clickable; source cards below with title, date, page range and snippet; clicking opens /documents/[id]#page-N.
3. Abstention state: a clearly different, neutral message (not an error) explaining the corpus does not answer the question.
4. Every answer shows the disclaimer: informational only, not legal advice; verify with a qualified lawyer.
5. Feedback: thumbs up/down plus "citation was wrong" option, sent to POST /api/feedback (ask Member B to add this endpoint and the feedback field in query_logs if it is not there; rate-limited).
6. Loading state, error state when the service is unavailable, and keyboard accessibility.
7. Tests: citation markers link to the right source card; abstention renders differently; Playwright flow ask -> answer -> open citation.

Visually separate "source found" from "legal conclusion" as section 16 requires.
```

Done when: the full ask → cited answer → open source page flow works in the browser (this is the Week 4 milestone demo).

### Prompt N — Document summarization

Owner: A (backend) \+ C (UI) · Week 5 · Covers: F-07, FR-08, UC-03, Section 13.3

```
Read F-07, FR-08, UC-03 and 13.3 of docs/master-document.md.

Backend (backend/app/rag/summarize.py):
1. POST /api/summarize {document_id} or {document_id, page_from, page_to} or {text}. Check access first.
2. Refuse to summarize when extraction failed or quality is low (UC-03 alternate flow); return a clear reason.
3. For long documents use map-reduce: summarize groups of chunks, then combine. Output a structured JSON: court/issuing body, date, parties or roles, legal issues, provisions cited, reasoning, outcome; each field may be "not stated" and must never be invented. Include page references for each field.
4. Cache summaries per document version so we do not pay twice.
5. Tests with the mock LLM, including the "not stated" behaviour.

Frontend: wire the Summarize button on /documents/[id] to show the structured summary with page links and the notice "Summaries may omit details; read the original before relying on it."
```

Done when: a judgment PDF produces a structured summary whose page links are correct, and a failed extraction shows a refusal instead of a summary.

### Prompt O — Legal category and forum guidance (backend)

Owner: A · Week 5 · Covers: F-12, FR-23 to FR-28, UC-07, Sections 5.5, 12.5, 13.7, RQ5 · Before starting: draft `data/forum-guide/forum_guide_v1.md` from official sources (one entry per category in 5.5, with province notes, source URL and access date) and send it to your legal reviewer. Use synthetic problem descriptions only.

```
Read F-12, FR-23..28, UC-07, sections 5.5, 12.5 and 13.7 of docs/master-document.md.

1. Forum guide: ingest data/forum-guide/forum_guide_v1.md into the forum_guide collection (one chunk per category entry), and seed forums and category_forum_map from a YAML file data/forum-guide/forums.yaml (name, forum_type, province, conditions, first_steps, source chunk). Show the guide's review date and reviewer status; entries not yet reviewed must be labelled "pending legal review" in responses.
2. backend/app/classify/: classify_problem(text, language) that
   - asks the LLM to choose 1-2 categories ONLY from the 13 fixed categories (return JSON with category ids, sub_issue, confidence: high|medium|low, missing_facts[]); reject any category not in the list
   - runs a keyword fallback (legal_categories.keywords, including Urdu and Roman Urdu words such as khula, talaq, mehar, FIR, kiraya, intiqal) when the LLM is unavailable, and as a sanity check
   - retrieves the matching forum-guide chunk(s) through the RAG retriever, so forum names and first steps come from the guide with citations, never from model memory
   - when the forum depends on missing facts (province, urban vs agricultural land, government vs private employer, claim amount), returns clarifying_questions instead of a forum
   - when confidence is low, uses tentative wording or returns "13. Other / unclear"
3. Safety check first: if the text describes immediate danger or violence, return an emergency block (Police 15, Rescue 1122, and other numbers confirmed by the team) before anything else (FR-27).
4. POST /api/classify and GET /api/categories. Also call classify inside /api/chat when the message describes a personal problem, and add a category_guidance field to the chat response.
5. Response includes lawyer_directory_link filtered by the matching practice area (FR-28).
6. evaluation/datasets/category_cases.csv: 50 synthetic problem descriptions to start (Urdu, Roman Urdu, English) with expected category and forum; tests that check the fixed-list rule, the emergency rule and clarifying questions.
```

Done when: "My husband stopped paying maintenance" returns Family category, Family Court with a cited guide passage, and a Find a lawyer link; a danger message shows emergency help first.

### Prompt P — Category and forum card in the chat UI

Owner: C · Week 5 · Covers: F-12 UI, FR-25 to FR-28, UC-07, Section 16 chat interface

```
Read UC-07 and FR-25..28 of docs/master-document.md. Run npm run gen:api.

1. In /ask, render category_guidance as a card under the answer: category and sub-issue, confidence shown in words ("likely", "possibly"), usual court or department, first steps, guide citations (opening the forum guide page), review date, and a "Find a lawyer in this area" button linking to /lawyers?practice_area=...
2. Clarifying questions render as quick-reply chips plus free text; the answer is sent back to /api/chat.
3. Emergency block renders at the very top in a prominent but calm style, before any other content.
4. Low confidence uses tentative wording; "Other / unclear" suggests consulting a lawyer.
5. Always show "General information, not legal advice."
6. Tests for each state (normal, clarify, emergency, unclear).
```

Done when: all four states display correctly from mock responses and from the live backend.

### Prompt Q — Lawyer profile backend (step-by-step)

Owner: B · Week 5 · Covers: F-09, FR-18 to FR-20, UC-08, Section 5.4, API rows for /api/lawyers

```
Read F-09, section 5.4 (all steps, weights and excluded fields), FR-18..22, UC-05, UC-08 and the /api/lawyers rows of section 15 in docs/master-document.md.

1. Registration as a lawyer requires only Step 1 (basic information). All other steps are optional, can be saved part-way, in any order.
2. PUT /api/lawyers/me/profile/{step} for steps 1-8 with a Pydantic schema per step. MVP must fully support steps 1, 2, 3 and 7; implement 4, 5, 6 and 8 too if simple, otherwise leave the schemas ready and mark them stretch.
3. GET /api/lawyers/me/completion: completion percent using the weights in 5.4 (20/20/15/10/10/10/10/5) and the next suggested step with a message like "Add your education to reach 75%".
4. Field visibility: lawyers choose public/private for sensitive fields (store in field_visibility). Account email/phone are always private. Office address is the lawyer's choice.
5. Excluded fields must not exist in any schema: win rate, cases won, client names, star ratings, CNIC on the public profile, "best lawyer" or guarantee claims. Add a simple validator that rejects such claims in headline/bio text.
6. GET /api/lawyers (public) with filters: city, practice_area, court_level, language, consultation_mode, pro_bono, verified_only, plus pagination. GET /api/lawyers/{id} returns only public fields and only completed sections; never return private fields or verification documents.
7. Tests: Step-1-only profile is valid and listed; completion math; private fields never appear in public responses (test every public endpoint); excluded claims rejected.
```

Done when: a lawyer with only Step 1 appears in the directory, and the completion percentage matches the 5.4 weights.

### Prompt R — Lawyer wizard, directory and public profile UI

Owner: C · Week 5 · Covers: F-09 UI, UC-05, UC-08, Section 16 (Lawyer directory, Lawyer profile wizard, Public lawyer profile)

```
Read UC-05, UC-08, section 5.4 and the lawyer rows of section 16 in docs/master-document.md. Run npm run gen:api.

1. /lawyers/onboarding: an Upwork-style wizard with a left step list (8 steps, required/optional markers, ticks for completed), a progress bar with the percent from /api/lawyers/me/completion and the next-step suggestion, Save, Skip and "Save and finish later" on each step, public/private toggles next to sensitive fields.
2. Step 2 includes credential fields and a licence/Bar card upload (sent in Prompt S). Show "Self-reported" next to credential fields until verified.
3. /lawyers: directory with all filters from section 5.4, profile cards (name, photo, city, practice areas, court level, verification label).
4. /lawyers/[id]: public profile showing completed sections only, "Verified on <date>" or "Self-reported" labels, contact options the lawyer chose, and the notice that the portal does not endorse any lawyer.
5. Read the ?practice_area= query so the chat "Find a lawyer" link pre-filters the directory.
6. Tests: wizard skip/save flow, empty sections hidden, labels correct.
```

Done when: a lawyer can register with basic info, skip to Step 7, see the progress bar update, and view the public profile with empty sections hidden.

### Prompt S — Lawyer verification workflow (admin)

Owner: B (backend) \+ C (queue UI) · Week 5 · Covers: FR-11, FR-21, FR-22, Section 5.4 credentials, Admin verification queue screen

```
Read FR-11, FR-21, FR-22, section 5.4 Step 2, and the Administrator verification queue row in section 16 of docs/master-document.md.

Backend:
1. POST /api/lawyers/me/verification: upload licence/Bar card (PDF/JPG/PNG, size limit, magic-byte check) stored in a private location; creates lawyer_documents rows and sets status pending.
2. GET /api/admin/verifications (admin): pending queue. GET /api/admin/lawyer-documents/{id} streams a document to admins only.
3. PATCH /api/admin/lawyers/{id}/verify {result: verified|rejected, fields_checked[], notes}: writes lawyer_verifications (what was checked, by whom, when). Verified appears only after this; rejected keeps Self-reported and shows the reason to the lawyer.
4. Tests: non-admins get 403 on every admin route and on document download; public endpoints never include verification documents or notes.

Frontend (/admin/verifications): queue table, detail view with the uploaded document, checklist of fields checked, approve/reject with notes, and verification history per lawyer.
```

Done when: an admin approves a lawyer and the public profile changes from "Self-reported" to "Verified on \<date\>", while the uploaded licence stays invisible to everyone else.

### Prompt T — Voice input (speech-to-text)

Owner: C · Week 6 · Covers: F-11, FR-14 to FR-17, UC-06, Sections 12.6 and 13.8

```
Read F-11, FR-14..17, UC-06, sections 12.6 and 13.8 of docs/master-document.md.

1. A useSpeechToText hook using the browser Web Speech API (SpeechRecognition / webkitSpeechRecognition) with lang "ur-PK" or "en-US" from a language selector next to the mic.
2. A microphone button in the /ask input: tap to start, tap to stop, live interim text, recording indicator, and the final transcript placed into the question box for the user to EDIT before sending. Never auto-send (misheard section numbers would retrieve wrong sources).
3. Clear messages for: browser not supported, microphone permission denied, no speech detected, unclear speech. Text input always stays available.
4. Before the first use, show a one-time notice that the browser vendor may process the audio, with a link to the privacy note; store consent in localStorage. Do not record or upload audio in the MVP.
5. Keep the hook behind an interface so a backend /api/transcribe (Whisper) can replace it later without UI changes.
6. Tests with a mocked SpeechRecognition: transcript lands in the box, errors show the right message, unsupported browser hides the mic and shows a tip.
7. Write docs/voice-testing.md: a checklist to test Chrome, Edge and one mobile browser in Urdu and English.
```

Done when: speaking an Urdu question in Chrome fills the box with editable Urdu text, and in an unsupported browser the mic hides with a clear tip.

### Prompt U — Read-aloud output (text-to-speech)

Owner: C · Week 6 · Covers: F-11, FR-29 to FR-32, UC-06, Section 13.8, Voice settings screen

```
Read FR-29..32, UC-06, 13.8 and the Voice settings row of section 16 in docs/master-document.md.

1. A useTextToSpeech hook using window.speechSynthesis: pick a voice matching the answer language (ur / en) from getVoices(), handle voices loading late (voiceschanged event).
2. A speaker button on every answer with play, pause, resume, stop and a speed control (0.75x, 1x, 1.25x, 1.5x). Only one answer plays at a time.
3. What is read: the answer text without [S#] markers, URLs or markdown, then short source names ("Source: <title>"), then the disclaimer.
4. If no voice exists for the language, show "Read-aloud is not available for Urdu in this browser" and keep the text fully usable (FR-32).
5. /settings/voice: auto-read answers toggle (off by default), preferred voice language, default speed; saved in localStorage.
6. Interface ready for a backend /api/tts later.
7. Tests with mocked speechSynthesis for each control and the no-voice case.
```

Done when: every answer has a working speaker button with pause/stop/speed, and the no-Urdu-voice message appears where needed.

### Prompt V — Admin curation dashboard and evaluation logging

Owner: C (UI) \+ B (endpoints) · Week 6 · Covers: F-10, FR-12, UC-04, Section 16 Upload/curation dashboard and Admin/evaluation view

```
Read F-10, FR-12, UC-04 and the Upload/curation and Admin/evaluation rows of section 16 in docs/master-document.md.

Backend (B): GET /api/admin/metrics returning aggregates only: documents by status, ingestion failures with reasons, queries per day, abstention rate, average and p95 latency, feedback counts, citation-wrong reports. No question or answer text.

Frontend (C), under /admin (admin role only, and the backend enforces it):
1. Upload page: drag-and-drop PDF, the metadata form from Prompt F, access level and collection with a required "permission confirmed" checkbox and permission note, then a live status view (uploaded -> extracting -> indexed / failed with reason).
2. Documents table: status, warnings, page count, chunk count, edit metadata, re-ingest, soft delete.
3. Extraction preview: page-by-page extracted text so the curator can spot-check Urdu pages.
4. Metrics page with simple charts for the aggregates above.
5. Tests: a non-admin is redirected; upload form validation.
```

Done when: an admin can upload, watch a document become indexed, spot-check its text, and see metrics that contain no user question text.

### Prompt W — Security, privacy and access-control hardening

Owner: B · Week 6 · Covers: Section 17 (every bullet), NFR Security and Privacy, Section 18 access-control tests, Risk "Confidentiality breach"

```
Read section 17, 12.3, 12.6 and the Access-control tests row of 18.1 in docs/master-document.md. Do a security review of the whole repository.

1. Go through every bullet in section 17 and produce docs/security-checklist.md with, for each bullet, where it is implemented (file and function) or what is missing. Fix what is missing.
2. Rate limiting (slowapi) on /api/chat, /api/search, /api/classify, /api/feedback, /api/summarize and auth routes.
3. Request size limits, input validation on every endpoint, security headers, secure cookie flags in production.
4. Check that logs never contain passwords, tokens, full questions, answers, transcripts or document text.
5. A dedicated tests/security/ suite: for every endpoint in section 15, test visitor, registered, lawyer and admin access; prove restricted chunks never appear in /api/search, /api/chat context, /api/summarize or citations for unauthorized users; prove private lawyer fields and verification documents never appear in public responses.
6. Run pip-audit and npm audit, fix high-severity issues, and add both to CI.
7. Write docs/data-governance.md covering data categories, permission records, retention, deletion and correction process, and voice data handling, for supervisor approval.
```

Done when: the security test suite passes and `docs/security-checklist.md` has no open items for the MVP.

### Prompt X — Evaluation harness and report data

Owner: A · Week 7 · Covers: Section 18 (all test types), RQ1 to RQ6, Section 4.4, Section 18.3 reporting

```
Read sections 4.3, 4.4, 13.2, 18 and 18.3 of docs/master-document.md. Build evaluation/ as a reproducible harness. Never invent numbers; every figure must come from a run.

1. Datasets (CSV in evaluation/datasets/, reviewed by a team member, synthetic or public data only):
   - retrieval_queries.csv: question, language, expected chunk/document ids, answerable yes/no (start with 40-60 questions)
   - unanswerable.csv: adversarial and out-of-corpus questions
   - category_cases.csv from Prompt O, expanded
   - speech_samples/ index for Urdu and English recordings with reference transcripts
2. Scripts:
   - eval_retrieval.py: Precision@k, Recall@k and nDCG@k for keyword vs semantic vs hybrid (RQ1)
   - eval_rag.py: runs /api/chat on all questions, records abstention correctness and produces a citation-audit sheet (claim, cited passage, supports? yes/partly/no) for manual marking (RQ2)
   - eval_extraction.py: extraction success per PDF from ingestion results (RQ3)
   - eval_latency_cost.py: p50/p95 latency and token cost per provider (RQ4)
   - eval_category.py: accuracy, macro-F1, per-class precision/recall, confusion matrix, forum correctness (RQ5)
   - eval_speech.py: word error rate per language from transcripts collected with the Prompt T checklist (RQ6)
3. Each script writes a markdown table and PNG charts to evaluation/results/<date>/ plus the exact config used (model, retriever, thresholds, dataset sizes).
4. Tune RAG_MIN_SCORE on a development split and report results on a held-out split.
5. One command, uv run python -m evaluation.run_all, runs everything that does not need manual input.
```

Done when: one command produces dated result tables and charts for RQ1–RQ6 that you can paste into the final report.

### Prompt Y — Optional trained baseline classifier (stretch)

Owner: A · Week 6–7, only if the supervisor approves (Appendix A) and 300+ reviewed labelled examples exist · Covers: Sections 13.4 and 13.7 optional baseline, RQ5

```
Read 13.4 and the optional trained baseline in 13.7 of docs/master-document.md.

1. In evaluation/classifier/, train a majority-class baseline and a TF-IDF (word + character n-grams, to handle Urdu and Roman Urdu) + Logistic Regression model with scikit-learn on evaluation/datasets/category_cases.csv.
2. Split by case (no near-duplicates across splits); stratified; fixed random seed.
3. Report class distribution, macro-F1, per-class precision/recall and a confusion matrix, and compare with the LLM classifier from Prompt O on the same test split.
4. Save the model with joblib and, if it helps, use it as a second keyword-style fallback in classify_problem behind a CLASSIFIER_BASELINE=on flag.
5. Write the comparison and its limitations to evaluation/results/<date>/classifier_comparison.md.
```

Done when: a comparison table LLM vs TF-IDF baseline exists with macro-F1 and a confusion matrix, or the team has recorded why it was skipped.

### Prompt Z — Deployment, demo data, README and final polish

Owner: All (B leads deployment, C leads demo UI polish, A leads report data) · Week 8 · Covers: Section 23 Definition of Done, Section 21 hosting, NFR Portability

```
Read sections 21, 23 and 19 (Week 8) of docs/master-document.md.

1. Demo data: a seed command that loads the approved public sample corpus, the forum guide, 5 synthetic lawyer profiles (clearly marked synthetic), one verified and one self-reported, and one admin. No real client data.
2. Production config: Dockerfiles for backend and frontend, a docker-compose.prod.yml, and documented environment variables. Keep a fully working local demo (docker compose up) as the fallback.
3. Prepare deployment on free or student tiers (frontend on a static/Next.js host, backend and Postgres with pgvector on a free tier that supports it); write docs/deployment.md with step-by-step instructions and the limits of each free tier. Do not buy anything; list costs for the team to approve.
4. README: one-page quick start that a new member can follow on a clean Windows laptop (Definition of Done item 1); test it by following it in a fresh clone.
5. Walk through every item of section 23.2 Definition of Done and write docs/definition-of-done.md marking each item with evidence (test name, screenshot path, or demo step).
6. docs/demo-script.md: a 10-minute viva demo covering search, cited chat answer, abstention, voice question and read-aloud, category and forum card, lawyer wizard and verification, admin upload, and the evaluation results. Each member presents a part.
7. Generate architecture, ERD and sequence diagrams (Mermaid) in docs/ for the final report.
8. Final cleanup: remove dead code, fix lint warnings, make sure CI is green, tag v1.0.
```

Done when: a fresh clone runs from the README alone, every Definition of Done item has evidence, and `v1.0` is tagged on `main`.

### Bonus prompts you will use every week

```
Review my branch against develop as a strict senior reviewer: bugs, missing tests, security problems (especially authorization and data leaks), and anything that breaks the rules in CLAUDE.md. List problems by severity, then fix the high-severity ones.
```

&nbsp;

```
Explain the code in <folder> to me as if I have to defend it at my FYP viva: what each file does, how data flows through it, which requirements (FR-xx) it implements, and 5 questions an examiner might ask with good answers.
```

&nbsp;

```
Update docs/progress.md with what was merged this week (from git log on develop), which requirements are now covered, open risks, and next week's plan. Keep it short.
```

## Part F — Testing, deployment, demo and submission checklist

The project is finished when every box below is ticked with evidence in the repo; this mirrors Section 23.2 of the master document.

### F1. Testing at every level

| Level | Tool | Who | When |
| :---- | :---- | :---- | :---- |
| Backend unit and API tests | pytest | A, B | Every PR (CI) |
| Access-control and privacy tests | pytest `tests/security/` | B | Every PR (CI) |
| Frontend component tests | Vitest \+ Testing Library | C | Every PR (CI) |
| End-to-end smoke tests | Playwright | C | Every Friday on `develop` |
| Retrieval, citation, category, speech evaluation | `evaluation/` scripts | A | Week 7, re-run Week 8 |
| Usability test with 5–8 users | Task sheet \+ short survey | All | Week 7 |

### F2. Definition of Done (Section 23.2)

- [ ] A new member can follow the README and run the project  
- [ ] An approved PDF ingests, or a clear failure is shown  
- [ ] Search results show source metadata and snippets  
- [ ] Chat answers include citations that resolve to real passages  
- [ ] Unanswerable questions trigger a safe fallback  
- [ ] Restricted documents cannot be reached by unauthorized users through UI or API  
- [ ] A user can ask by voice, edit the transcript, and listen to the answer, or fall back to text  
- [ ] A described problem returns a category, usual forum and first steps with citations, or a clarifying question  
- [ ] A lawyer can register with basic info only, add steps later, and sees Verified only after admin approval  
- [ ] Evaluation methods and actual results, including failures, are documented  
- [ ] Known limitations and future features are documented

### F3. Before the viva (Week 8\)

- [ ] Supervisor decisions from Appendix A recorded in `docs/supervisor-decisions.md`  
- [ ] Forum guide reviewed by a legal reviewer, with the review date shown  
- [ ] Local demo works offline on at least two laptops (fallback if hosting or the LLM fails)  
- [ ] Mock LLM mode ready as a last-resort demo fallback, clearly labelled  
- [ ] Demo uses synthetic or public data only  
- [ ] Final report chapters (Appendix C) drafted from `docs/` and `evaluation/results/`  
- [ ] Slides, architecture/ERD diagrams, and the 10-minute demo script rehearsed by all three  
- [ ] Every member can run the system and explain every part of the architecture  
- [ ] `v1.0` tagged on `main` and the repository shared with the supervisor

## Part G — Changing features after v1.0

Claude Code does not remember earlier chats: each session starts fresh, on each laptop. Your project's memory is the repo itself, so every change starts by updating the documents Claude Code reads, then the code.

### G1. The project memory files

| File in the repo | What it remembers | Who updates it |
| :---- | :---- | :---- |
| `CLAUDE.md` | Project rules, folder owners, commands; loaded automatically when Claude Code starts at the repo root | Anyone, through a PR |
| `docs/master-document.md` | Requirements (FR-01 to FR-32, F-01 to F-12); the source of truth | Whoever proposes the change |
| `docs/adr/` | Decisions and why they were made (one short file each, numbered) | Owner of the area |
| `docs/progress.md` | What was built each week and what is next | Weekly, with the bonus prompt |
| `CHANGELOG.md` | What changed in each release (`v1.0`, `v1.1` …) | Author of each PR |
| GitHub issues and PRs, Git history | Every change and the reason for it; Claude Code reads them with `git log` and `gh` | Automatic |
| Code and tests | The current behaviour of the system | Every PR |

Claude Code also keeps a personal auto memory per project on each laptop. Teammates do not share it, so anything the team needs goes into `CLAUDE.md` or `docs/`.

### G2. The change-a-feature workflow

1. **Write the change down first.** Edit the requirement in `docs/master-document.md` (for example FR-29: read-aloud on by default). If the reason matters, add `docs/adr/00N-<topic>.md` with the context, the decision and the consequences.  
2. **Open an issue** describing the change and the requirement IDs affected, then branch from `develop`: `git checkout -b change/<short-name>`.  
3. **Run the impact prompt** (G3) in plan mode and read the impact list before approving any edits.  
4. **Make the change** with the same prompt, including tests and docs.  
5. **Test and review:** run all tests, read the diff yourself, open a PR that links the issue and the ADR; a teammate approves.  
6. **Release:** merge to `develop`, then to `main` with a new tag (`v1.1`, `v1.2` …) and a `CHANGELOG.md` entry.  
7. **Teach future sessions:** if the change creates a new project rule, add it to `CLAUDE.md`.

### G3. Example prompts

**Change an existing feature**

```
Read CLAUDE.md, docs/master-document.md (FR-29..32 changed today) and docs/adr/.
We are changing an existing feature: read-aloud should now be ON by default, with a one-time notice.
1. Find every place in backend and frontend that implements read-aloud (search the code, git log, and the tests).
2. Show me the impact: files, tests, docs and other features affected.
3. Then make the change, update or add tests, update docs/progress.md and CHANGELOG.md.
Do not change unrelated behaviour.
```

**Add a new feature**

```
Read CLAUDE.md, docs/master-document.md and docs/adr/.
We are adding a new feature F-13: <describe it>. I have added its requirements as FR-33..FR-3x in the master document.
1. Propose a design that fits the existing architecture (section 10) and folder ownership in CLAUDE.md: endpoints, database changes (as a new Alembic migration, never editing old ones), UI screens and tests.
2. List risks to access control, privacy and existing features.
3. Wait for my approval, then build it in small steps, running the tests after each step.
```

**Remove or replace a feature or component**

```
Read CLAUDE.md and docs/adr/. We are replacing <old component, e.g. browser speech-to-text> with <new one, e.g. backend Whisper via /api/transcribe>.
1. Find everything that depends on the old component and show me the list.
2. Write docs/adr/00N-<topic>.md recording the decision.
3. Replace it behind the existing interface, keep the old one available behind a setting until the new one passes its tests, then remove dead code.
4. Update tests, README, CHANGELOG.md and CLAUDE.md if any command or rule changed.
```

**Catch up after a long break**

```
I am returning to this project after a break. Read CLAUDE.md, docs/progress.md, CHANGELOG.md, docs/adr/ and the last 30 commits on develop. Summarize the current state of the system, open issues, known limitations and anything that looks half-finished. Then run all tests and tell me what fails.
```

### G4. Rules for changes

- Never edit an old Alembic migration; add a new one.  
- Never change an API response shape without regenerating the frontend types (`npm run gen:api`) in the same PR.  
- Re-run the matching evaluation script from Prompt X when a change touches retrieval, the RAG prompt, the category classifier or speech, and keep the old results for comparison.  
- A change that touches access control, privacy or lawyer verification needs review from Member B and a passing `tests/security/` suite.