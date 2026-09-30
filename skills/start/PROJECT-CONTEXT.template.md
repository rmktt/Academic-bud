<!--
The single source of project facts. Every advisor reads this before doing
anything. The lead fills it in by interviewing you, or you can copy this
file to PROJECT-CONTEXT.md in your project and fill it in yourself.

Anything left as a {{PLACEHOLDER}} or "undecided" is treated as unknown.
Advisors will ask about it (or report it as missing) rather than guess.

Never put secrets here (passwords, API keys, tokens). If a secret exists,
write only WHERE it lives.

You can use roles instead of names, and leave the institution out.
-->

# Project context

**Project name:** {{PROJECT_NAME}}
**One-line description:** {{ONE_LINE_DESCRIPTION}}
**Type of project:** {{e.g. capstone, senior design, thesis, course project}}
**Institution / program:** {{optional — leave out if you prefer}}
**Team:** {{roles, not names — e.g. "2 software, 1 hardware, 1 design"}}
**Key deadline(s):** {{DATES — the real ones}}

## Outside the project

### Problem and who it's for

{{The problem, in one or two sentences, and who actually has it.}}

### What already exists

{{Existing products, systems, or published work that solve this or
something close — and why they aren't enough here.}}

### How it's evaluated

{{Who grades or reviews it (a role is enough), what they care about most,
and whether a required report template or rubric exists. If it does, say
where the files are.}}

## Inside the project

### Core idea

{{The specific mechanism that makes the project work — not the pitch.
e.g. not "a smart parking app," but "a spot only counts as occupied after
5 continuous seconds of detection, which filters out passing pedestrians."}}

### Tech stack

{{Languages, frameworks, database, hardware, protocols. Be specific —
"Node.js 20 + Express 4 + PostgreSQL 16" is more useful than "JavaScript".}}

### Architecture / data model

{{The main components and how they connect — modules, services, tables and
their relationships. Enough to reason about tradeoffs without reading the
whole codebase.}}

### Decisions already made

{{Each decision worth defending, and WHY it was made. A decision without
its reason isn't useful to an advisor.}}

### Open problems

{{Real unsolved issues — a hardware limit, a failure mode, a scaling or
security gap. Advisors need real gaps to be useful; don't leave this empty
if problems exist.}}

### Deliberately out of scope

{{What was chosen not to be built, and why — so no advisor reports a
considered decision as "missing".}}

## Sensitive items — never restate in output

{{Where sensitive things live, e.g. "an API key is in the team's local
.env, never committed". Never the value itself.}}
