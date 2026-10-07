# Companies and where to search them

> **How to use this file**
> 1. Look at `demo/companies.example.md` for an example of a filled-in company list.
> 2. Add one block per company you want to check. Copy the block below as often as you need.
> 3. Replace every `[FILL IN: ...]` with your own value.
> 4. Delete these instruction lines and the worked example at the bottom.
>
> Not sure about a field? Write `unknown`. The agent tests the page on its first run and records what works.

Last checked: [FILL IN: leave as "never", the agent updates this after each run]

If a link stops working, fix it here, not in the chat.

## [FILL IN: company name, e.g. Company A]
- **[FILL IN: country, e.g. Country A]:** [FILL IN: link to the company's own careers page, ideally already filtered to your levels in this country]
  - also run: [FILL IN: other search words worth trying, e.g. `query=intern`, or leave out this line]
- **[FILL IN: second country, or delete this line]:** [FILL IN: link]
- **How:** [FILL IN: `fetch` if a plain page fetch shows the jobs, `browser` if the page needs JavaScript to load them, or `unknown`]
- **Quirks:** [FILL IN: anything you already know, e.g. "a search word is required", or leave empty. The agent adds to this.]

---

> **Worked example of a filled-in block (delete this before use)**
>
> ## Company A
> - **Country A:** https://careers.example.com/company-a?query=junior&location=Country+A
>   - also run: `query=intern`, `query=graduate`
> - **Country B:** https://careers.example.com/company-a?query=junior&location=Country+B
> - **How:** browser. A search word is required; an empty search shows nothing useful.

>
> If a link can't be found: `- **Country B:** **TODO.** Find the working search link.`
> If a site won't load: add `- **Backup:** [job board link]` and a note that it wouldn't load.