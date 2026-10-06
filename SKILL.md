---
name: "job-alert-agent"
description: "Checks the user's fixed list of companies for new job openings in their chosen locations, judges each one against their CV using their fit rules, saves a warm newsletter-style digest and ends with a short apply-list summary. Use this whenever the user asks to run their job check, job alert, job digest or weekly job search, or asks \"any new jobs at my companies?\", and when their weekly scheduled task runs."
---

# Job Alert Agent

## Purpose

Find new, fitting jobs at the user's chosen companies, save a warm, readable digest, and finish with a short summary the user can read in a notification: which jobs to apply to, and by when.

## When to use this

- Every week via a scheduled task, or whenever the user asks to check their companies for new jobs.
- If more than two weeks have passed since the last digest in `digests/`, run it now.

Not in this version: searching companies outside the list, and applying to jobs automatically. Do not do either.

## Running unattended

The weekly run usually happens with nobody watching. Do not stop to ask questions. Make the most reasonable choice, note it in the digest under "Notes from this run", and carry on. If the playbook folder or the config files cannot be read at all, stop and make the final summary say exactly that, so the user knows the run did nothing.

## First-run setup

The working folder is the folder this SKILL.md sits in, unless the user names another one. All paths below are relative to it.

The user's personal setup lives in four files in `files/`. They are private and are not part of the shared playbook:

- `files/companies.md`
- `files/preferences.md`
- `files/profile.md`
- `files/shown-jobs.md`

For each one that is missing, there is a template next to it with the same name plus `.example` (for example `files/companies.example.md`).

- If the user is present: copy the templates, then ask the user to fill in `companies.md`, `preferences.md` and `profile.md` before continuing. `shown-jobs.md` can start empty.
- If nobody is present: do not guess, do not use the example content. Stop and say in the final summary which files are missing.

## Toolbox

Do not rebuild these files from memory.

- `files/INDEX.md`: what each file is for. Read it first.
- `files/companies.md`: the working search link for each company and country, whether the site needs a simple fetch or the browser, and quirks from earlier runs. Use it in step 2 instead of searching for careers pages. When a link fails, or you find a better one, update the company's block in this file so the next run starts from the fix.
- `files/preferences.md`: the places in order of preference, work modes, levels, fields, and the user's dealbreakers (languages, maximum years of experience). Use it in steps 3 to 7 and in Decisions.
- `files/profile.md`: the user's CV and evidence (experience, skills, education, projects). If the user keeps this as several files in `data/`, read those instead. Use it in step 9.
- `files/fit-rules.md`: the verdict rules for judging fit. Use it in step 9. Judge fit yourself with these rules; do not hand the job to another agent.
- `files/shown-jobs.md`: every job already shown in a digest. Use it in steps 1, 8 and 13. Do not use it to decide fit; it only prevents repeats.
- `digests/`: one file per run, named `YYYY-MM-DD.md`. Earlier digests show the expected tone and format. For the first run, `sample-digest.md` shows it.
- `notes.md`: corrections, one dated line each, newest first.

## Inputs

1. **Company list:** the companies in `files/companies.md`, with the search link for each, per country. Always start with the company's own careers page. Job boards and LinkedIn are allowed only as a backup when the careers page fails (see Decisions).
2. **Places, in order of preference:** from `files/preferences.md`. On-site, hybrid and remote all count unless preferences say otherwise. Remote counts only in the countries where preferences say it does.
3. **Levels:** from `files/preferences.md` (for example entry-level, graduate, trainee, junior, internship, traineeship).
4. **Fields that usually fit:** from `files/preferences.md`, plus similar roles that match the user's CV.
5. **The user's CV and profile:** `files/profile.md`. `files/fit-rules.md` says how to judge a job against it.
6. **List of jobs already shown:** `files/shown-jobs.md`.

## Steps

1. Open `files/shown-jobs.md`, so you know what the user has seen.
2. Take the first company in `files/companies.md` and open its search link for each country listed, using the method that block says (fetch or browser).
3. Filter by place (the places in `files/preferences.md`, including remote where it counts), if the page allows it.
4. Filter by work mode (on-site, hybrid, remote), if the page allows it.
5. Filter by level (the levels in `files/preferences.md`), if the page allows it.
6. Look through what is left and keep the jobs whose field could fit.
7. For each job you kept, open the job posting itself. Check the place written in the posting, not the page filter. Drop it if the place is not on the list.
8. Drop any job that is already in `files/shown-jobs.md`.
9. Judge each remaining job with `files/fit-rules.md` against the user's profile, and write down its verdict, confidence, strengths, gaps, open questions, reason and application deadline (if the posting gives one).
10. Apply the rules in Decisions below.
11. Mark this company as checked, then go back to step 2 with the next company until all companies are done.
12. Write the digest (see "How the digest should read" below) and save it as `digests/YYYY-MM-DD.md` with today's date.
13. Add every job in the digest to the top of the table in `files/shown-jobs.md`, with today's date, verdict, deadline and link.
14. End with the final summary (see below) as your last message. It is what reaches the user as a notification or email.

## Decisions

The limits below come from the user's dealbreakers in `files/preferences.md`.

- If the posting **requires** a language the user does not have, drop the job.
- If the posting names a language only as **a plus**, keep it.
- If the posting asks for the user's **maximum years of experience or more**, drop the job, even if the title says junior.
- If the posting asks for some experience but **less than the limit** (for example one year when the limit is two), keep it but lower the confidence to low.
- If the posting asks for experience without a number ("some experience", "a few years"), keep it, lower the confidence to low, and say so in the digest so the user checks it themselves.
- If the job's place is not on the list, drop it, even if the careers page filter showed it.
- If the verdict is **skip**, leave the job out.
- If the verdict is **apply**, put the job in the digest near the top.
- If the verdict is **borderline**, put the job in the digest below the apply jobs.
- Ranking: apply ranks above borderline. Within each group, higher confidence comes first, then the place order in `files/preferences.md`.
- If a careers page will not load or blocks you, look for the company's jobs on a job board or LinkedIn instead. Only read what is openly shown; do not log in or get around blocks. If that fails too, say in the digest that this company could not be checked this time. Never leave it out silently.
- If a job found on a job board or LinkedIn also exists on the careers page, link to the careers page version.
- If a posting page will not open, judge the job from the text shown on the careers page and say so in the digest.
- If the digest cannot be saved to `digests/`, put the full digest in the final message instead. Do not lose it.
- If there are no fitting jobs this time, still save a short digest saying so, and list the companies checked.

## How the digest should read

Write it like a short, warm newsletter, not a table or a bare database. Friendly, encouraging, honest. Address the user as "you". Open with the number of jobs found and any deadline in the next 14 days. For every job include:

- job name
- company
- place and work mode
- deadline, if the posting gives one
- a short summary of the job description
- why you fit, with the concrete reason behind the verdict
- the gaps
- confidence score
- fit score
- the link to the posting, so the user can check it themselves

End with a table of the companies checked and what happened at each, then "Notes from this run" for anything unusual.

A bad version: just a database or list of rows, vague praise like "you're a great fit!" with no reason, missing links, jobs the user has seen before, or jobs that break their rules.

## Final summary

The last message of the run, at most about 10 lines, plain text, readable on a phone:

- First line: how many jobs to apply to and how many borderline, e.g. "2 to apply, 1 borderline."
- One line per apply job: title, company, city, deadline if any, link.
- One line listing the borderline jobs by title and company.
- One line naming any company that could not be checked.
- Last line: where the full digest is saved (`digests/YYYY-MM-DD.md` in the working folder).

If nothing fits, say so in one line, plus the companies that could not be checked.

## Definition of done

- All companies were checked, or the digest says which ones could not be checked and why.
- Every job in the digest passed the place, language and experience rules.
- No job in the digest appeared in an earlier digest.
- Every job has a working link.
- The digest is saved in `digests/`, `files/shown-jobs.md` is updated, and the final summary was sent.

## Edge cases

- **Junior title but too many years in the text:** has happened before. Always read the requirements; the years rule wins over the title.
- **Wrong country despite the filter:** has happened before. Always check the place in the posting itself.
- **Careers pages that block automatic visits** (such as Workday sites): try a job board or LinkedIn, or ask the user to paste the text in, and report it if nothing works.
- **LinkedIn and job boards often block automatic visits too** and may not allow it in their rules. Treat them as a backup that may fail, never as the main source.
- **Same job posted twice** under slightly different titles or places: show it once.