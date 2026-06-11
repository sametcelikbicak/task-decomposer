# Task Decomposer — Usage Scenario

## Scenario: Developer says "build a blog platform"

**User:** `Alex` (full-stack developer, 5 years experience, first time giving a large task to AI)

---

### Act 1 — Vague Request (The Problem)

**Alex:** "Build me a blog platform."

**Typical AI reaction (without task-decomposer):**
Starts writing code immediately. 200 lines in, things get tangled. User says "no, not like that." Context balloons. Results in frustration.

---

### Act 2 — Goal Articulation (Clarification)

**AI (task-decomposer active):** "Let's break this down. First, let's clarify the goal:"

> **Goal:** A blog with an admin panel, markdown support, and user authentication. MVP scope, must ship in 1 week. Using Next.js + PostgreSQL. Must be deployable.

**Alex's response:** "Yes, Next.js 14 + Prisma + PostgreSQL. NextAuth for authentication. react-markdown for rendering. No comments system in MVP — only admin publishes."

---

### Act 3 — Dependency Map

**AI:**

```
Level 1 — Foundation
  1. Next.js project setup + Prisma schema design
  2. PostgreSQL connection + migration
Level 2 — Authentication
  3. NextAuth integration + admin role
Level 3 — Core Features
  4. Blog CRUD API (create, edit, publish, delete)
  5. Admin panel UI (list, write, edit posts)
  6. Public blog view page
Level 4 — Polish
  7. Markdown rendering, SEO meta, error handling
Level 5 — Verify
  8. Tests + deploy prep
```

> "Is this the right order? Want to adjust any dependencies?"

**Alex:** "Looks good. Levels 1-3 can be one session. Level 4 separately."

---

### Act 4 — Atomic Prompt Cards

**AI generates cards that each fit comfortably within a context window:**

```markdown
## Prompt 1: Project setup + Prisma schema
### Context
Create an empty Next.js 14 project with Prisma + PostgreSQL.
### Requirements
- `create-next-app` with TypeScript
- Prisma schema: User (id, name, email, role), Post (id, title, slug, content, published, createdAt, updatedAt, authorId)
- PostgreSQL connection string in `.env`
- Initial migration
### Acceptance Criteria
- `npm run dev` works
- `npx prisma db push` succeeds
- Tables visible in `prisma studio`

## Prompt 2: NextAuth + admin role
### Context
Add NextAuth to the project from Prompt 1.
### Requirements
- [... continues]
```

> **8 prompt cards total.** Each is self-contained and executable independently.

---

### Act 5 — Execution Plan

**Final plan presented to Alex:**

```markdown
## Execution Plan

Session 1 (today — ~2 hours)
  ☐ Prompt 1: Project setup + Prisma
  ☐ Prompt 2: NextAuth + admin
  ☐ Prompt 3: CRUD API
  ☐ Prompt 4: Admin panel UI
  └─ ✔ Verify: log into admin, write post, save

Session 2 (tomorrow — ~1 hour)
  ☐ Generate CONTEXT.md → carry to new session
  ☐ Prompt 5: Public blog pages
  ☐ Prompt 6: Markdown + SEO + error handling
  ☐ Prompt 7: Tests
  ☐ Prompt 8: Deploy configuration
  └─ ✔ Verify: open live site, see the post
```

**Alex:** "Perfect, let's start."

---

### Act 6 — Session Handoff (Context Transfer)

**End of Session 1** — AI auto-generates `CONTEXT.md`:

```markdown
# Context Handoff — Session 1

## Completed
- [x] Next.js 14 + Prisma setup — `/prisma/schema.prisma`
- [x] PostgreSQL migration working
- [x] NextAuth with email/password login + admin role
- [x] Blog CRUD API — `/api/posts`
- [x] Admin panel UI — `/admin/*`

## Decisions
- Authentication: Email/password (Google OAuth to be added later)
- Slug: auto-generated from `title`, unique constraint
- Role system: `role: enum(ADMIN, AUTHOR, READER)` on User model

## Next session
- Prompt 5: Public blog — `app/blog/[slug]/page.tsx`
- Prompt 6: Markdown rendering, SEO metadata, error boundaries
- Prompt 7: Integration tests
- Prompt 8: Vercel deploy config + env vars

## State
- Branch: `feature/blog-platform`
- Migrations: 1 (initial schema)
- Env vars: `DATABASE_URL`, `NEXTAUTH_SECRET`, `NEXTAUTH_URL`
```

---

### Act 7 — Session 2 (Resume Where You Left Off)

**Alex:** "Continuing — I've attached the CONTEXT.md."

**AI:** Reads CONTEXT.md, starts directly from Prompt 5. No need to remember past context — everything is in CONTEXT.md.

---

## Summary

| Stage | Without task-decomposer | With task-decomposer |
|---|---|---|
| Start | "Build blog" → AI runs in all directions | Goal articulation clarifies intent |
| Prompt | One massive prompt → bloated, loses focus | 8 atomic prompts → each is crisp |
| Context | Forgets by hour 2 | CONTEXT.md → zero context loss |
| Result | Half-finished project, frustration | Step-by-step MVP delivered |

---

## Alex's take (simulated)

> "The first time I said 'build a blog,' the AI jumped straight into code. 5 minutes later I was correcting it. With task-decomposer, it questioned me first, then broke everything into steps with verification checkpoints. Finished in 2 sessions with zero context loss."
