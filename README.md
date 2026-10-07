# How to use the Job Alert Agent

This agent checks a list of companies' careers pages for new entry-level jobs, judges each one against your CV, and saves a short newsletter-style digest. It is a Claude skill: a `SKILL.md` playbook plus files it reads and updates. There is no code to run.

Out of the box it runs on a **fictional demo persona** (a junior data engineer in the Netherlands), so you can see it work before you enter anything of your own.

## What you need

- A Claude setup that can read files in a folder and open web pages: Cowork, or Claude Code, or Claude on claude.ai with skills enabled.
- A browser tool for some companies. The demo includes Amazon, whose job lists only load in a browser. Cowork has a built-in browser. In plain Claude Code there isn't one, so Amazon will show up as "could not be checked" in the digest. That is expected.

## Option A: run it in place (no install)

Use this to try the demo, or if you don't want to install anything.

1. Download or clone this repo to a folder.
2. Open a Claude session that can access that folder (connect it in Cowork, or start Claude Code inside it).
3. Send: **"Follow SKILL.md in this folder and run the job alert."**
4. Wait for the final summary. The full digest is saved in `demo/digests/` and the jobs it showed are added to `demo/shown-jobs.example.md`.

## Option B: install as a skill in Claude Code

Use this if you want to trigger it by name from any session.

1. Put the whole repo folder in `~/.claude/skills/job-alert-skill/` (all your projects) or `.claude/skills/job-alert-skill/` (one project).
2. Start a new Claude Code session.
3. Send: **"Run my job alert."**

If you already have a skill with the same name, change `name:` at the top of `SKILL.md` first, or the two will compete.

## Option C: upload to claude.ai

1. Zip the repo folder.
2. In claude.ai, open Settings and look for the Skills area (the menu has been called "Features"; check Anthropic's current docs if you can't find it): [Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).
3. Upload the zip, switch the skill on, and send: **"Run my job alert."**

This option needs a plan that includes skills and code execution.

## Make it yours

The agent decides which setup to use by looking at the files in `files/`:

- While they still contain `[FILL IN`, it runs on the demo persona in `demo/`.
- Once none of them does, it runs on your own data.

To switch:

1. Open `files/profile.md`, `files/preferences.md` and `files/companies.md`.
2. Replace every `[FILL IN: ...]` with your own content. Look at the matching file in `demo/` for an example of a good answer.
3. Leave `files/shown-jobs.md` empty. The agent fills it in.
4. Run again with the same sentence. The first line of the digest tells you whether it used your files or the demo.

Tips for a good profile:

- Be specific: what you did, with which tools, for how long, and with what result.
- Label coursework and self-study as such.
- List what you don't have under "Not covered". The agent uses it to spot gaps.

## Run it every week

When you have set it up with your own data, you can ask Claude to run it on a schedule (for example every Monday). A scheduled run works without you: it never stops to ask questions and notes its choices in the digest under "Notes from this run".

## Run the demo again from scratch

The agent remembers which jobs it has shown, so a second run shows only new ones. To start fresh:

1. Delete the files in `demo/digests/`.
2. Open `demo/shown-jobs.example.md` and delete every row under the table header.

## Where things are

| Path | What it is |
|---|---|
| `SKILL.md` | The playbook the agent follows. |
| `files/` | Your own setup (blank until you fill it in), plus `fit-rules.md` and `INDEX.md`. |
| `demo/` | The fictional persona's files and the digests from demo runs. |
| `digests/` | Your own digests, once you use your own data. |