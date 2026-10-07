# Index

What each file is for. Paths are relative to the working folder (the folder that contains SKILL.md).

## Shared playbook (ships with the repo)

| File | What it is |
|---|---|
| `SKILL.md` | The playbook: purpose, steps, decision rules, digest format, edge cases. |
| `files/INDEX.md` | This file. |
| `files/fit-rules.md` | How to judge fit: hard filters, verdict rules, what to write down per job. |
| `demo/` | A fictional demo persona with its own `*.example.md` setup files, a sample digest and test-run digests. The agent runs on it while `files/` still contains `[FILL IN`. |

## Your own setup (fill these in to use your own data)

| File | What it is | Created by |
|---|---|---|
| `files/companies.md` | Companies to check, with the search link per country, whether to use fetch or the browser, and quirks. The agent updates it when it finds a better link. | The user. `demo/companies.example.md` shows a filled-in version. |
| `files/preferences.md` | Places in order of preference, work modes, levels, fields, languages with levels, maximum years of experience. | The user. `demo/preferences.example.md` shows a filled-in version. |
| `files/profile.md` | The user's CV and evidence: experience, skills, education, projects. | The user. `demo/profile.example.md` shows a filled-in version. |
| `files/shown-jobs.md` | Every job already shown, so none repeat. The agent adds to it after each run. | The agent |
| `digests/YYYY-MM-DD.md` | One digest per run (in a demo run, `demo/digests/`). | The agent |
| `notes.md` | Corrections, one dated line each, newest first. | The agent or the user |