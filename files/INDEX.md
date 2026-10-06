# Index

What each file is for. Paths are relative to the working folder (the folder that contains SKILL.md).

## Shared playbook (ships with the repo)

| File | What it is |
|---|---|
| `SKILL.md` | The playbook: purpose, steps, decision rules, digest format, edge cases. |
| `files/INDEX.md` | This file. |
| `files/fit-rules.md` | How to judge fit: hard filters, verdict rules, what to write down per job. |
| `sample-digest.md` | An example digest with fictional companies and jobs, showing the expected tone and format. |
| `files/*.example.md` | Templates for the private files below. |

## Private files (the user's own setup, never shared)

| File | What it is | Created by |
|---|---|---|
| `files/companies.md` | Companies to check, with the search link per country, whether to use fetch or the browser, and quirks. The agent updates it when it finds a better link. | The user, from `companies.example.md` |
| `files/preferences.md` | Places in order of preference, work modes, levels, fields, languages with levels, maximum years of experience. | The user, from `preferences.example.md` |
| `files/profile.md` | The user's CV and evidence: experience, skills, education, projects. | The user, from `profile.example.md` |
| `files/shown-jobs.md` | Every job already shown, so none repeat. The agent adds to it after each run. | The agent, from `shown-jobs.example.md` |
| `digests/YYYY-MM-DD.md` | One digest per run. | The agent |
| `notes.md` | Corrections, one dated line each, newest first. | The agent or the user |