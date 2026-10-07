# Job Alert Skill

This skill checks a list of companies' careers pages for new jobs at the levels you choose (from internships to senior roles), judges each one against your CV, and saves a short newsletter-style digest. It is a Claude skill: a `SKILL.md` playbook plus files it reads and updates. There is no code to run.

Out of the box it runs on a **fictional demo persona** (a junior data engineer in the Netherlands), so you can see it work before you enter anything of your own.

## What you need

- A Claude setup that can read files in a folder and open web pages: Cowork, or Claude Code, or Claude on claude.ai with skills enabled.
- A browser tool for some companies. The demo includes Amazon, whose job lists only load in a browser. Cowork has a built-in browser. In plain Claude Code there isn't one, so Amazon will show up as "could not be checked" in the digest. That is expected.

## Option A: Run it in place (no install)

Use this to try the demo, or if you don't want to install anything.

1. Download or clone this repo to a folder.
2. Open a Claude session that can access that folder (connect it in Cowork, or start Claude Code inside it).
3. Send: **"Follow SKILL.md in this folder and run the job alert."**
4. Wait for the final summary. The full digest is saved in `demo/digests/` and the jobs it showed are added to `demo/shown-jobs.example.md`.

## Option B: Install it as a skill to run by name

Use this if you want to trigger the skill by name (`job-alert-skill`) from any Claude Code session.

1. Put the whole repo folder in `~/.claude/skills/job-alert-skill/` (shared across all projects) or `.claude/skills/job-alert-skill/` (one project only).
2. Start a new Claude Code session or close and reopen the one you have.
3. Send: **"Run my job alert."** — Claude will find and run the `job-alert-skill` skill.

If you already have a skill with the same name, change `name:` at the top of `SKILL.md` first, or the two will compete.

