## Case Study: Job Alert Skill

### Problem

Every week, you want to scan a your usual list of "dream"  companies' careers pages across your chosen countries, filter for specific roles, check if they fit your CV (skills, experience, location, languages), and avoid showing you jobs you've already seen.

This means either:
- Running a Python script that needs internet access, API keys, and maintenance (why we replaced it).
- Asking an agent to do it fresh each time (stateless, repeats work, costs more).
- Asking many agents: one per company, one to judge fit, one to deduplicate (fragile, expensive, hard to fix).
- Doing it manually yourself (time-consuming)

This skill is one agent + one playbook (SKILL.md).

### Architecture: One agent + playbook

User: "Check my jobs"
  ↓
One skill (SKILL.md) runs:
  - Reads: profile.md (your CV)
  - Reads: preferences.md (places, levels, languages, dealbreakers)
  - Reads: companies.md (which companies, search links per country)
  - Reads: fit-rules.md (verdict rules: apply / borderline / skip)
  - Reads: shown-jobs.md (what you've seen before)
  
  - Fetches: each company's careers page
  - Filters: by place, level, language, experience rules
  - Judges: each job against fit-rules.md
  - Deduplicates: against shown-jobs.md
  - Saves: digest to digests/YYYY-MM-DD.md
  - Updates: shown-jobs.md with new jobs
  
  ↓
Digest: "Here are 2 to apply to, 1 borderline"

### The Playbook: SKILL.md Excerpt

**Trigger:** Weekly via scheduled task, or whenever the user asks "run my job alert", "check my companies", "any new jobs?"

**Inputs:**
- User's CV and evidence (profile.md)
- Places in order of preference (preferences.md)
- Companies and their search links per country (companies.md)
- Verdict rules (fit-rules.md)
- Jobs shown before (shown-jobs.md)

**Steps (simplified):**
1. Check `files/` for `[FILL IN]` markers. If any exist, run on `demo/` instead (first-time setup).
2. For each company in companies.md:
   - Open its search link for each country.
   - Filter by place, work mode, level (if the page allows it).
   - Open each job posting itself. Verify location from the posting, not the filter.
3. Drop jobs that:
   - Are not in your preferred places
   - Require a language you don't have
   - Ask for years of experience ≥ your dealbreaker (e.g., 2 years max)
   - Are already in shown-jobs.md
4. For each remaining job, judge it with fit-rules.md: **apply**, **borderline**, or **skip**.
5. Rank: apply jobs first (by confidence, then by place preference), then borderline.
6. Write the digest as a warm newsletter (not a table). Include place, deadline, link, why you fit, your gaps.
7. Add every job shown to the top of shown-jobs.md.
8. End with a final summary: "2 to apply, 1 borderline. [links]. Full digest: digests/2026-10-07.md"

**Check before finishing (5 rules):**
1. The digest says how many jobs to apply to and how many are borderline.
2. Every job has a working link to the posting.
3. No job in the digest was in an earlier digest.
4. Every job passed the place, language, and experience rules.
5. The digest is saved to `digests/YYYY-MM-DD.md` (or `demo/digests/` if demo mode).

**The toolbox that improves:** After each run, update `companies.md` with better links if one broke. Add new rules to `fit-rules.md` if you spot patterns. The decision trail (notes in the digest) shows what you learned.

### Fix Log: Three Real Edge Cases

**1. Junior title, but 2+ years required**  
Failing case: A job is titled "Junior Data Engineer" but the posting asks for "2+ years of data engineering experience". Your dealbreaker is 2 years max.  
Fix: Always read the years requirement from the posting. Title is advisory; years requirement wins. Added to SKILL.md Decisions: "If the posting asks for the user's maximum years of experience or more, drop the job, even if the title says junior."

**2. Wrong country despite the filter**  
Failing case: Amazon's Netherlands search returned a job actually in Hyderabad, India. The filter didn't catch it.  
Fix: Always check the place written in the job posting itself, not the search filter result. Added to SKILL.md Decisions: "If the job's place is not on the list, drop it, even if the careers page filter showed it."

**3. Careers page blocks automatic visits**  
Failing case: A careers page (e.g., Workday site) blocks page-load scripts and returns nothing.  
Fix: Try a job board or LinkedIn as a fallback. Only read what's openly shown; do not log in or get around blocks. If that fails too, report it in the digest. Added to SKILL.md: fallback option in the Decisions section.

### Why Playbook, Not Python (or many agents)

An earlier version was Python + GitHub Actions + API keys:
- It had no internet access in the sandbox environment.
- API keys for job-board access had to stay local (security risk).

The playbook version:
- Runs inside Claude. No API keys needed; no internet setup.
- It runs unattended but leaves a decision trail (the digest notes).

### Make It Yours

This repo ships with a **fictional demo persona**: Jamie Verhoeven, a recent data engineering graduate looking in the Netherlands and Germany.

**To use it for yourself:**

1. **Fill in `files/profile.md`:** Your CV, skills, education, projects. Copy the format from `demo/profile.example.md`.
2. **Fill in `files/preferences.md`:** Your places, work modes, levels, fields, dealbreakers (max years, required languages). Copy the format from `demo/preferences.example.md`.
3. **Fill in `files/companies.md`:** Add the companies you want to watch. For each, find the careers search link and note whether it needs fetch (simple HTTP) or browser (JavaScript-heavy). Copy the format from `demo/companies.example.md`.
4. **Fill in `files/fit-rules.md`:** Update the verdict rules to match your profile. (e.g., if you have AWS experience, maybe AWS is a "strong match" instead of "nice to have"). This file is already in `files/`.
5. **Customize `files/shown-jobs.md`:** Leave the table empty for your first run.
6. **Adjust `files/INDEX.md`:** Optional; this is just a reference guide.

The first time you run the skill, it will check `files/` for `[FILL IN]` markers. When it finds them, it falls back to `demo/` and tells you it ran on the demo persona. When all markers are filled, it will run on your data.

### Governance Note

**Data the skill sees:** Your CV, location preferences, company names, and job postings from public careers pages.

**What the skill is not allowed to do:** Apply to jobs, send emails, log in to sites, or get around login walls. It only reads and reports.

**Who reviews:** You review the digest before deciding to apply. The skill flags jobs it's unsure about as "borderline" so you can check them yourself.

This design aligns with responsible AI: transparent rules, human review before action, decision trails for auditability. (See the thesis on creative professionals and informal AI governance for deeper context.)

