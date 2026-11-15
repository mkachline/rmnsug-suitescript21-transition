# rmnsug-suitescript21-transition

An AI skill that converts NetSuite **SuiteScript 1.0 and 2.0** files to **SuiteScript 2.1**. Give it a script downloaded from NetSuite (one script at a time is recommended; developers can instead give it a whole `SuiteScripts.zip`), and it returns the converted script, a bug report, and a plain-language summary.

Built for the [Rocky Mountain NetSuite User Group (RMNSUG)](https://rmnsug.org/), because [Oracle is retiring SuiteScript 1.0 and requires the move to 2.1](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_7161922360.html).

The whole skill is one file: [`SKILL.md`](SKILL.md).

## Quick start (NetSuite admins and accountants)

1. **Install the skill.**
   - In your web browser, go to
     `https://raw.githubusercontent.com/mkachline/rmnsug-suitescript21-transition/refs/heads/main/SKILL.md`
   - Right-click the page, choose **Save As**, and save it as **`SKILL.md`**. Check that the name is exactly `SKILL.md`, not `SKILL.md.txt`. If your browser added `.txt`, rename the file.
   - In your AI assistant's settings, find **Skills** and upload that file. In Claude this is under Settings > Capabilities, or Customize > Skills, depending on your version of the app.
2. **Start a conversation.** In a new chat, type `/rmnsug-suitescript21-transition` and tell the assistant you're new to this, for example: "I'm a NetSuite admin, not a programmer. What should I do first?" The assistant will walk you through it **one script at a time**: backing up, downloading a script from NetSuite, converting it, and what to check before putting the converted file back. For a fuller walkthrough with screenshots, see the November 2026 post, "SuiteScript for Non-Scripters", under [rmnsug.org/posts](https://rmnsug.org/posts/). **Nothing is changed in your NetSuite account** until you upload the results yourself.

> **Please don't ask the assistant to "install this skill" from a web address.** It will read the page and write its *own* version of the skill, which is not the tested one. Always add the downloaded file through the Skills settings.

*Can't add skills on your plan?* In a chat that can browse the web, say: "Please read https://raw.githubusercontent.com/mkachline/rmnsug-suitescript21-transition/main/SKILL.md and follow it exactly, without rewriting it. I'm a NetSuite admin, not a programmer. What should I do first?"

## What it does

- Converts SuiteScript 1.0 and 2.0 scripts to 2.1, keeping each script's original coding style and folder structure.
- Aims for **identical behavior**. Where the original has bugs, the converted file keeps them, with a `FIXME` comment on each. Every converted file gets a new name ending in `-2.1.js` (for example, `util_RMNSUG.js` becomes `util_RMNSUG-2.1.js`), so your original is never overwritten. A second file named `Name-2.1-WithBugFixes.js` has the fixes, so you choose which to deploy.
- Leaves files alone that are already 2.1, aren't scripts, or are third-party libraries (it mentions any that look old).
- Flags hard-coded passwords, API keys and similar secrets without repeating their values.
- Asks the user almost no questions. It makes documented default decisions and lists them under "Assumptions I made" in its summary.
- Ends with a short, fixed-shape summary: a count table, bugs found, sensitive information, which version to use, what to test, and how to upload.

## Install as a skill (developers)

The skill follows the open Agent Skills layout: a folder containing a `SKILL.md` with `name` and `description` frontmatter. It has no dependencies, scripts or other files. The folder name should match the skill's `name`, which is `rmnsug-suitescript21-transition`.

**Claude Code**

```bash
mkdir -p ~/.claude/skills/rmnsug-suitescript21-transition
curl -fsSL https://raw.githubusercontent.com/mkachline/rmnsug-suitescript21-transition/main/SKILL.md \
  -o ~/.claude/skills/rmnsug-suitescript21-transition/SKILL.md
```

Use `.claude/skills/` inside a project instead to scope it to that repo. Restart Claude Code, then check with `/skills`. See the [Claude Code skills docs](https://code.claude.com/docs/en/skills).

**Claude (web and desktop)**

Open the [raw `SKILL.md`](https://raw.githubusercontent.com/mkachline/rmnsug-suitescript21-transition/refs/heads/main/SKILL.md) in your browser and right-click > Save As `SKILL.md` (not `SKILL.md.txt`), then upload that single file in the Skills settings. Skills settings live under Settings > Capabilities, or Customize > Skills in newer versions of the app. Custom skills require a paid plan with code execution enabled. See [Getting started with skills on Claude](https://support.claude.com/en/articles/12512180-getting-started-with-skills-on-claude-ai).

If your version of the app rejects a bare `SKILL.md`, Anthropic's docs describe uploading a `.zip` containing the skill *folder* instead:

```bash
mkdir rmnsug-suitescript21-transition
curl -fsSL https://raw.githubusercontent.com/mkachline/rmnsug-suitescript21-transition/main/SKILL.md \
  -o rmnsug-suitescript21-transition/SKILL.md
zip -r rmnsug-suitescript21-transition.zip rmnsug-suitescript21-transition
```

**OpenAI Codex**

Same pattern, under `~/.codex/skills/rmnsug-suitescript21-transition/SKILL.md`. Restart Codex afterward. See the [Codex skills docs](https://developers.openai.com/codex/skills).

**ChatGPT**

Skills > Create > Upload from your computer, using the same zipped folder as above. Availability of custom skills depends on your plan; see [OpenAI's skills overview](https://openai.com/academy/skills).

**Any other agent that supports Agent Skills:** put `SKILL.md` in a folder named `rmnsug-suitescript21-transition` wherever that tool looks for skills.

## Requirements

- A model that is good at writing code. We suggest Claude Opus 5.5, OpenAI 6.1 Sol, or Gemini 4 Argon. Model names change quickly, so treat these as suggestions. Smaller models tend to ask questions an accountant can't answer or skip steps.
- **Web access.** The skill tells the model to read Oracle's SuiteScript documentation before converting. A model that can't read web pages is told to stop and say so, rather than guess.

## Status and limits

- **Conversions have not been run in NetSuite.** Always back up your `SuiteScripts` folder, try the converted files in a sandbox if you have one, and test the highest-risk scripts before relying on them. The skill's summary says which ones those are.
- Every converted script (1.0, 2.0 or 2.x) needs its existing Script record re-pointed at the new `-2.1` file. See [Oracle's guide](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_160098641577.html).
- Don't commit your own NetSuite scripts, exports or secrets to this repository. The [`.gitignore`](.gitignore) is an allowlist to help prevent that.

## Contributing and feedback

Issues and pull requests are welcome. For a conversion problem, please say which model and interface you used, which step went wrong, and remove any company data, passwords or keys from anything you paste.

## Credits and license

- Created for the [Rocky Mountain NetSuite User Group](https://rmnsug.org/). See the November 2026 presentation, "SuiteScript for Non-Scripters", under [rmnsug.org/posts](https://rmnsug.org/posts/).
- Built on Oracle's SuiteScript documentation, including [Oracle's own SuiteScript upgrade skill](https://github.com/oracle/netsuite-suitecloud-sdk/tree/25488efa43523be7dccb9fe6bbefdbd8ae7a5806/packages/agent-skills/netsuite-suitescript-upgrade).
- [MIT License](LICENSE).
