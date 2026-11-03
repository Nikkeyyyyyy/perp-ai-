# PlacementPilot AI

**One platform. Every company. Every role. One personalized preparation journey.**

An AI-powered placement preparation platform: a different course for every company
and role you target, DSA practice with a real editor, AI mock interviews with scored
evaluations, timed assessments, a résumé and job-description matcher, and an
evidence-based readiness score.

---

## Quick start

```bash
npm install
```

```bash
npm run setup
```

```bash
npm run dev
```

Then open <http://localhost:3000>.

`npm run setup` generates the Prisma client, creates the SQLite database and seeds it
with 15 job roles, 161 companies, 54 coding problems and a demo student.

**Demo account** — `demo@placementpilot.ai` / `demo1234`
It comes with a 56-lesson course in progress, 15 solved problems, a completed mock
interview and a submitted assessment, so every screen has real data on first open.

---

## Demo Mode, and what it means

The app runs with **zero external services**. Every feature works out of the box —
but it never pretends an integration is live when it is not.

| Capability | Without configuration | With configuration |
| --- | --- | --- |
| Course generation | Built from the authored curriculum, labelled **demo content** | AI-generated syllabus for the exact company, role, level and timeline |
| Lesson bodies | Authored material: key ideas, pitfalls, interview questions, worked code, quizzes | Full prose written per lesson |
| AI Tutor | Built-in tutor replies, clearly marked as such | Context-aware conversational tutoring |
| Mock interviews | Question bank + a transparent keyword-and-structure rubric that explains exactly what it measured | Live interviewer with follow-ups and a model-written evaluation |
| Résumé review | Rule-based ATS review, skill extraction and JD matching | The same, plus model findings on the actual text |
| Code execution | **Practice-only mode** — code is saved, nothing is run, and the UI says so | Submissions run against the full hidden test suite |

Nothing is ever fabricated: no invented run results, no made-up company requirements,
no claimed AI output when no model ran.

### Enabling live AI

Add one of these to `.env` and restart:

```bash
ANTHROPIC_API_KEY=sk-ant-...
```

```bash
OPENAI_API_KEY=sk-...
```

Optionally pin the model with `AI_PROVIDER` and `AI_MODEL`. Keys are read server-side
only — they never reach the browser and are never stored in the database.

### Enabling code execution

Point at any Judge0-compatible provider:

```bash
JUDGE0_URL=https://your-judge0-host
```

Add `JUDGE0_KEY` and `JUDGE0_HOST` if your provider requires them. Code is never
executed on the application server.

---

## What's in it

**Company Explorer** — 161 seeded employers across global tech, SaaS, Indian product
companies and startups, IT services, consulting, banking and fintech, gaming and
industrial. Search by name, industry, country, city, type, size or difficulty. Add
*any* company that is missing: give a name, optionally a website or a pasted job
description, and a starting profile is built for you to edit. Custom profiles are
marked **unverified** and are private to you.

**Course generator** — The syllabus is not a fixed template. A service-sector employer
adds an aptitude module; a highly selective one adds harder patterns and a design
module; a beginner gets fundamentals first; a data role gets SQL and statistics
instead of frontend machine coding. Module count, lesson selection and pacing all
follow the role, your diagnostic result and the time you actually have.

**Interactive lessons** — Objectives, key ideas, worked code, the mistakes that cost
marks in interviews, the questions interviewers actually ask, linked practice
problems and a quiz. 60% passes the lesson; 100% marks it mastered.

**DSA Practice Studio** — 54 original problems across 24 topics with staged hints,
editorials, complexity analysis, a Monaco editor with six languages, custom input,
submission history, bookmarks and notes.

**Mock interviews** — Ten round formats (DSA, technical, core CS, frontend, backend,
full-stack, project, HR, behavioural, company-specific) with follow-up questions,
optional voice input, and a scored report covering technical accuracy, problem
solving, communication, clarity and ownership — plus what you missed and what to do
next.

**Assessment Center** — Timed papers with a section mix matched to the employer type,
a question navigator, mark-for-review, auto-save, auto-submit on expiry, and
topic-wise weakness analysis with full answer review.

**Résumé & JD matcher** — PDFs are parsed in your browser (the file never leaves your
device). ATS findings, skill extraction, keyword match against a job description, the
gap as a study list, and the questions your own résumé invites. It will never invent
experience for you.

**Readiness score** — Weighted from course completion (30%), practice (25%),
interviews (20%), assessments (15%) and quiz averages (10%), with the breakdown and a
confidence level shown. It is an estimate derived from measured activity, never a
decorative number.

Plus: application tracker with a status board and deadlines, analytics with topic
accuracy and score trends, ⌘K global search, a notification centre, light/dark/system
themes, and keyboard-navigable, screen-reader-friendly UI throughout.

---

## Stack

Next.js 15 (App Router) · React 19 · TypeScript · Tailwind CSS v4 · Prisma · SQLite
Radix primitives · Recharts · Monaco · Framer Motion · Zod · bcrypt + JWT sessions

### Moving to PostgreSQL

The schema is deliberately portable — every JSON-shaped column is TEXT holding JSON.

1. In `prisma/schema.prisma`, change `provider = "sqlite"` to `"postgresql"`.
2. Point `DATABASE_URL` at your Postgres instance.
3. `npx prisma db push && npm run db:seed`

No model changes are required.

---

## Project layout

```
prisma/
  schema.prisma          23 models covering every feature
  seed.ts                roles, companies, problems, demo student
src/
  app/
    (auth)/              sign in and register
    onboarding/          six-step conversational setup
    (app)/               the signed-in product (shared shell)
    api/                 21 route handlers
  components/
    ui/                  the design system
    app/                 shell, navigation, command palette, notifications
    …                    one folder per feature area
  lib/
    data/                companies, roles, problems, question bank
    content/             the authored curriculum
    generators/          deterministic course/lesson/interview/assessment/résumé logic
    ai/                  provider abstraction and AI-or-fallback feature wrappers
    auth.ts db.ts api.ts progress.ts execution.ts
```

The split between `generators/` and `ai/` is the core design decision: every feature
has a complete, deterministic implementation, and the AI layer is an *upgrade* over
it rather than a dependency.

---

## Scripts

```bash
npm run dev          # development server
npm run build        # production build
npm run setup        # generate client, create database, seed
npm run db:reset     # wipe and re-seed
npm run lint         # eslint
```

---

## Content and licensing

All 54 coding problems, the curriculum, the question bank and the interview questions
were written for PlacementPilot. Company profiles carry a verification status, a
confidence level and source links; interview rounds and topics are presented as
*commonly reported patterns for that kind of employer*, never as confirmed claims
about a specific company's current process. Practice assessments are never presented
as official company tests.
