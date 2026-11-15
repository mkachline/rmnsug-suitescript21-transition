---
name: rmnsug-suitescript21-transition
description: Converts NetSuite SuiteScript 1.0 and 2.0 files (.js files or a SuiteScripts.zip) to SuiteScript 2.1 and returns the converted files. Use when the user wants to upgrade, migrate, or transition NetSuite scripts to 2.1.
license: MIT
---
# Transition SuiteScripts to SuiteScript 2.1

## About this skill
[Oracle NetSuite has mandated](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_7161922360.html) that their users transition existing scripts which are written in [SuiteScript 1.0](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_7151939532.html) and SuiteScript 2.0, to the [SuiteScript 2.1](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_8161516336.html) API.
In this skill, you will take each of the user-provided scripts, transition those to SuiteScript 2.1, and then return the transitioned script(s) back to the user.


## About your user
Unless you know otherwise, the user you are working with is **not technical!** They do not know how to code, they are probably accountants by trade. Your user is relying on you to make the proper technical decisions to transition their SuiteScript files to version 2.1.
It is safe to assume that your user does have "Administrator" access to their NetSuite system.
If the user is an agent or developer, adjust your explanations and instructions accordingly.

## About your access to NetSuite
The intent of this skill is that you do NOT modify the user's NetSuite system, even if you have MCP access into NetSuite.  If you do have NetSuite access, it is OK (and advisable) for you to "read" the system to gain clarity (e.g., running searches of 'Script' and/or 'ScriptDeployment' record types.) Only make changes to the user's NetSuite system if they explicitly authorize such. Ask for permission for any changes you make into the user's NetSuite system.


## About the model running this skill
This skill requires writing code, so it should be run by a model which is effective at writing code. If you are not such a model, tell the user briefly at the start, and suggest which model from your family would be better suited to writing code. The user decides whether to switch.
This skill also requires that you can read web pages. The Oracle documentation pages referenced throughout this skill are important knowledge for writing this code correctly. If you cannot read web pages, first consider whether that ability might simply be turned off (e.g., a web search or browsing setting in a chat interface); if so, ask the user to enable web access, then continue. If web access is truly unavailable, tell the user, and do not attempt the conversion; suggest that they use a model or tool which can read web pages.


## Overall Process this skill should follow
1. Before starting, read each link in [Additional Resources](#additional-resources). If you can't open the links, see [About the model running this skill](#about-the-model-running-this-skill).
1. Your user will give you script(s) to convert
1. You will almost never need to ask the user questions. Use the defaults in [Default Decisions](#default-decisions-do-not-ask-the-user) and keep going.
    - Only ask a question if you truly cannot proceed. Remember the user is an accountant: they do not understand the File Cabinet's folder structure or source code. Ask in plain business language, and never about files, folders, or code.
    - If the user is a developer or an agent, asking questions is fine.
1. You will transition each script to SuiteScript 2.1, except those skipped per the Developer Notes below ("Skip ..." notes)
1. You will return newly-converted script(s) to your user.
1. You will give the user a summary of what files you changed.
    - Make sure to highlight the `-WithBugFixes.js` files. Tell the user which bugs you found and fixed, and then have a conversation with them about whether to use the `-WithBugFixes.js` version or the original-named version of each script.
        - The user would want the original-named version (which retains the bugs) if they want no change in behavior. This is the default recommendation, because the intent of this skill is that the end functionality is exactly what the user is used to today, even if that includes bugs.
        - The user would want the `-WithBugFixes.js` version if they understand how the script should work with the bug fixed. Remember, if the 'old version' had bugs, then, the user is "used to" the script behaving with bugs. Thus, it's important for the user to understand how the script will work once the bug(s) are fixed.
    - Note to the user if any of these scripts appear to contain PII, passwords or API keys. In such a case, use [Oracle Article: Secrets](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_160216486846.html) as a guideline. Flag the secret, but never repeat the actual secret values (passwords, keys, PII) in your reply.
1. You will mention any third-party library refactors, risks or upgrade suggestions. If no third-party libraries were referenced, then, nothing to say here.
1. You will give your user suggestions of what they should test
    - Do NOT ask the user to 'test everything', they don't have time for that. Focus on highest risk changes or decisions you had to make.
    - It is OK to only lightly suggest testing, if you believe the script conversion(s) were straightforward.


## Shape of your final summary to the user
Use this shape for your final wrap-up message, so that every session ends the same way. Write it in plain, non-technical language. Omit any section which has nothing to say. The content below is an **example**; replace it with what actually happened.

```markdown
## Summary
| File type | Received | Converted | Skipped | Had bugs | Notes |
| --------- | -------: | --------: | ------: | -------: | ----- |
| SuiteScript 1.0 scripts | 4 | 4 | 0 | 1 | |
| SuiteScript 2.0 scripts | 2 | 2 | 0 | 0 | |
| SuiteScript 2.x scripts | 1 | 1 | 0 | 0 | |
| Already SuiteScript 2.1 | 1 | 0 | 1 | 0 | Left unchanged |
| Third-party libraries | 3 | 1 | 2 | 0 | jquery upgraded |
| Not scripts (text, images, etc.) | 2 | 0 | 2 | 0 | Skipped |
| **Total** | **13** | **8** | **5** | **1** | |

< Table rules: On every row, Received = Converted + Skipped. The Total row is the column sums. "Had bugs" counts scripts (not individual bugs) and is a subset of Converted. Omit any row whose Received is 0. Keep Notes to a few words, or leave blank. >

## Bugs found
< List the files with bugs, one line each, in exactly this format: - `path/File.js` — severity (e.g., severe, minor). Do NOT describe the bugs, give details, or give advice here. The user can chat with you as a follow-up to learn more. >

## Sensitive information
< List the files with sensitive information, one line each, in exactly this format: - `path/File.js` — kind (e.g., hard-coded password, API key). Do NOT give details or advice here. The user can chat with you as a follow-up to learn more. >


## Which version to use
< Only if bugs were found. One line per file with a `-WithBugFixes.js` copy: - `File.js` — recommend original or fixed, plus a few words on why (in business terms, not code). Default to recommending the original unless the bug makes the script fail outright. Then ask the user which version they want. Do not ask about anything else (e.g., duplicates or deployment) here. >

## Assumptions I made
< One short line per Default Decision you actually used, e.g., "Converted both copies of `X.js` (found in two folders)." Omit this section if you made none. >

## Before uploading
< Only if needed, and concise. For example: for SuiteScript 1.0 scripts, the existing Script record must be updated to use the converted file, and its entry points should be checked. Also say that nothing was run in NetSuite, so the conversions are untested. >

## What to test
< A short numbered list (about 3-5 items) of the highest-risk items only, in business terms. Omit if the conversions were straightforward. >

## Optional clean-up
< Keep this CONCISE. Do not include this at all unless absolutely necessary. Remember the end-user is an accountant, not a developer. >
`Commission_v1.0.js` has "1.0" in its name. You may want to rename it in the File Cabinet.

Would you like to learn more about the bugs I found?
Would you like to learn more about the Sensitive Information I found in scripts, and how to fix that?
Do you need guidance on how to release these changes into your NetSuite system?
```


## Default Decisions (do not ask the user)
Make these decisions yourself. Do NOT ask the user about them. List each one you actually made under "Assumptions I made" in your final summary.
- **Duplicate filenames in different folders:** Convert every copy. Preserve each copy's folder path. Never merge them or pick one.
- **Folders that look unused (e.g., "Deprecated", "Old"):** Convert them anyway. You cannot see whether a script is deployed, and a skipped live script would break after the 1.0 retirement. Mention in the summary that some may be unused.
- **Hard-coded passwords, API keys, secrets:** Keep behavior identical and leave the values in place. Flag each in the summary, without repeating the values.
- **Third-party libraries:** Keep the existing version unless the conversion requires a newer one. If a library is old, just mention it.
- **Whether a script is deployed:** Only check if you have NetSuite access (see "Is a Script File actually being used"). Otherwise, convert everything.
- **Risky scripts (e.g., a SQL query tool):** Convert them and flag the risk in the summary.


## About "packaging" of scripts given by the user
The user will probably give you "scripts to convert" in one of two ways. Either, the user will give you a "SuiteScripts.zip" file, which contains every SuiteScript file in their system, or, the user might give you just a single JavaScript file.
If the user does not know how/where to give you scripts, suggest to them to follow this sequence of steps in their NetSuite system. This produces "SuiteScripts.zip":

1. Log in to NetSuite
1. Go to Documents > File Cabinet
1. Type `SuiteScripts` in the search box at the left and click Search
1. In the results, find the row whose **Type is "Folder"**. It is not the row ending in ".zip" (that is an older archive). Click the "Download" link on that row. A "SuiteScripts.zip" file will be downloaded to your computer.

## About "packaging" of Scripts returned by you, to the user
If the user gave you a zip file, then, you should return to the user, a new zip file. Always name it exactly `SuiteScripts21.zip`, regardless of the name of the zip file you were given. The new zip should contain only the converted files and any `-WithBugFixes.js` files; leave out the files you skipped, so that uploading it does not touch unchanged files.
If you also write a summary file, name it `SuiteScripts21-Conclusions.md`.
If the user gave you individual JavaScript files, then, you should return to the user, individual JavaScript files.
Tell the user to *overwrite* the existing file of the same name in the File Cabinet, rather than adding a new file. See [Oracle Article: Setting Up Your SuiteScript Development Environment](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_0402065535.html).
For converted SuiteScript 1.0 scripts, also tell the user that the existing Script record must be updated to use the converted file; NetSuite then updates the record's API version to 2.1. Entry point function names may have changed, so the user should check the Script record's entry points. See [Oracle Article: Converting a SuiteScript 1.0 Script to a SuiteScript 2.1 Script](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_160098641577.html).


## Developer Note: Is a Script File actually being used in NetSuite?
In many cases, the File Cabinet contains "zombie scripts": source code which exists in the File Cabinet, but is no longer referenced by a "Script" record in NetSuite.
If you have access into the NetSuite system, consider following [this Oracle Article](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_0827101556.html) for identifying which scripts are actually being used.


## Developer Note: Preserve Directory Structure
If the SuiteScripts.zip file (given to you, by the user) has a structure of folders, then, the zip file which you return to the user should preserve that same folder structure.
The intent is that the user will take the zip file which you return, and use the NetSuite File Cabinet "Advanced Add" button to put the new files back into their correct folders.
A `-WithBugFixes.js` file (see [If you find bugs](#if-you-find-bugs-)) goes in the same folder as its original script.

## Developer Note: Skip "Not SuiteScript" file(s)
If you are given file(s) which are NOT SuiteScript, then, skip those.


## Developer Note: Skip "Already SuiteScript 2.1" file(s)
Leave files that are already SuiteScript 2.1 unchanged, unless a change is needed (e.g., updating library references or replacing deprecated API calls). Treat this as a rule of thumb, not a hard-and-fast rule.


## Developer Note: Focus on "Functionally Equivalent"
For each script given to you, your "transitioned" script should function the same as the original script. The goal is that "nobody even noticed" the transition to SuiteScript 2.1.
When an old API call has no exact SuiteScript 2.1 equivalent, use your judgment to choose whatever code changes are needed so that the converted script produces the same end result as the original script.
### If you find bugs ...
If you find 'bugs', you will be faced with the decision of: "Fix the bug?" or "Keep the new script functionally equivalent?" Ultimately, the user needs to make this decision, but, we will default with keeping the bugs as-is. The user is used to these scripts behaving with their bugs, and the intent of this skill is to NOT change behavior. Therefore, the 'same filename' new script is the **functionally equivalent** version: it **is** converted, but **intentionally** retains the original bugs (never put a behavior-changing fix in this file; all fixes go only in the `-WithBugFixes.js` file), and has `FIXME` comments added at the end of the line(s) containing the actual identified bug, describing the bug and suggesting a fix.
In such cases, you should create, and return to the user, **two** new script files: the 'same filename' new script (converted, bugs retained, with `FIXME` comments), and an additional script file whose filename uses a `-WithBugFixes.js` filename suffix. This `-WithBugFixes.js` file is also converted, but has the identified bugs fixed. In this file, put a short `/* FIXED: ... */` comment at the end of each line you fixed, briefly stating what changed, so the user can easily compare the two versions.
At the end of the session, tell the user which bugs were found and fixed, and discuss with them whether to use the `-WithBugFixes.js` version or the original-named version of each script. Default to recommending the original-named version unless the user wants the changed behavior.
**Filename example:**
MyScriptFile.js -> TWO FILES returned...  `MyScriptFile.js` (bugs retained, with FIXME comments) and `MyScriptFile-WithBugFixes.js` (bugs fixed)
**Example "with bugs" code block in new `MyScriptFile.js`**:
```
var myrecord = .... ; /* Originally declared as "myrecord" with correct spelling */
/* ... */
if( myrecrod.type == 'salesorder') { /* FIXME: 'myrecrod' typo will always throw an exception */
    /* ... */
}
```

### If you find "Public" Third-Party Libraries
Do not refactor "publicly available" third-party libraries such as 'jquery', 'lodash', etc.
- If you determine that your new SuiteScripts require either a new version of some third-party library, or a refactor of the third-party library, then ...
    - Search the web: Is there a newer version of the library obtainable? If 'yes', then, use that newer version found.
        - If the user's SuiteScript source code referenced a 'local copy of' the third-party library, then, you should also fetch a 'local copy of' the newer version.
        - If the user's SuiteScript source code referenced a CDN link, then, you should also reference a CDN link.
- If you end up using new libraries, in each SuiteScript which references this new library, add a one-line `@since YYYY-MM-DD (Upgrade of LibName.js from version <old version> to version <new version>)`
- Remember this particular library work, so that you can tell your user about it once you're done.



## Developer Note: Preserve the Old Script's Coding Style
When creating a transitioned script, take effort to retain the old script's "coding style" into the new script. We want this so that the script's original developer can feel comfortable that "this script looks like something I would have written."
Do note that, some scripts are written by different authors, so, consider this philosophy on a 'script-by-script' basis.


## Developer Note: Also-Transition "Commented Out" Code
As you convert a script, if you find blocks of code which are 'commented out', DO convert that 'commented out' code as well, keeping it commented out.


## Developer Note: Client Scripts that use `window` or the DOM
Oracle does not support direct access to the NetSuite UI through the DOM; the NetSuite UI should only be accessed using SuiteScript APIs. See [Oracle Article: Code-Level Best Practices](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_164314083142.html). If a Client Script touches the DOM or `window` against the NetSuite UI, do not guess at a replacement: preserve the behavior where you can, and flag it to the user as a risk to test.


## Developer Note: Converting Libraries
If you find scripts which appear to be used as libraries, consider transitioning those libraries to becoming AMD-style library modules. Think, and use your judgment here. [Preserving Functionality](#developer-note-focus-on-functionally-equivalent) is still your guide here.


## Developer Note: Naming New Scripts
Your newly transitioned script filenames should be identical to the filename being transitioned. The only exception is when you find bugs: in that case you also return a second file with a `-WithBugFixes.js` suffix (see [If you find bugs](#if-you-find-bugs-)). The converted script with the original filename is always returned, and it keeps the original bugs. You will find that, some filenames contain, in the filename, hints that the script is "1.0" or "2.0" or "2.x". In your wrap-up comments to the user, you might lightly suggest that the user considers renaming those script files from within the NetSuite File Cabinet.


## Developer Note: Library Naming Conventions
When converting from SuiteScript 1.0 to 2.1, you will introduce the use of NetSuite libraries such as "N/log" and "N/search". When naming these libraries, try to use your library name consistent with the actual library; versus using abbreviated names; even if the overall codebase tends to also-use abbreviations. If your desired library name conflicts with an existing variable name, then, prefix your library name with 'lib'.
### Examples of desirable library naming conventions:
- `define(["N/search"], function (search) {`
- `define(["N/search"], function (libsearch) {` /* codebase uses 'search' already */
- `define(["N/runtime"], function (runtime) {`
- `define(["N/render"], function (render) {`
- `define(["N/log"], function (log) {`
- `define(["N/log"], function (liblog) {` /* codebase uses 'log' already */

### Examples of undesirable naming conventions:
- `define(["N/search"], function (s) {`
- `define(["N/runtime"], function (run) {`
- `define(["N/render"], function (r) {`
- `define(["N/log"], function (lg) {`
- `define(["N/log"], function (l) {`
- `define(["N/log"], function (Log) {` /* Always all-lowercase library names */


## Developer Note: "File" Comment Block before `define` line
Every file will have a comment block which defines, at least, the NetSuite-required `NApiVersion` JSDoc entry. Please put this entire comment block **before** your `define(___) function (___) {`
### Desirable Comment Block location Example
```
    /**
     * Sends email notifications for any overdue Sponsorship Invoices
     *
     * @exports sponsor/overdue-invoice/sc
     *
     * @copyright 2017 Stoic Software, LLC
     * @author Eric T Grubaugh <eric@stoic.software>
     *
     * @NApiVersion 2.1
     * @NModuleScope SameAccount
     * @NScriptType ScheduledScript
     * @since YYYY-MM-DD (Refactor from SuiteScript 1.0 to SuiteScript 2.1)
     */
define(["N/email", "N/render", "N/runtime", "N/search", "N/log"], function (email, render, runtime, search, log) {
```
### Undesirable Comment Block location Example
```
define(["N/email", "N/render", "N/runtime", "N/search", "N/log"], function (email, render, runtime, search, log) {
    /**
     * Sends email notifications for any overdue Sponsorship Invoices
     *
     * @exports sponsor/overdue-invoice/sc
     *
     * @copyright 2017 Stoic Software, LLC
     * @author Eric T Grubaugh <eric@stoic.software>
     *
     * @NApiVersion 2.1
     * @NModuleScope SameAccount
     * @NScriptType ScheduledScript
     * @since YYYY-MM-DD (Refactor from SuiteScript 1.0 to SuiteScript 2.1)
     */
```

## Developer Note: Add `@since` JSDoc
In every new file-header JSDoc, in the same block where you already define `@NApiVersion`, always also-add: `@since YYYY-MM-DD (Refactor from SuiteScript <old version> to SuiteScript 2.1)`.
- This JSDoc is intended to give a concise note that, this script was once very different from YYYY-MM-DD and prior.
- If there are previous `@since` blocks, preserve those, and just add your new block.
- The `<old version>` should be one of "1.0", "2.0" or "2.x"
- `YYYY-MM-DD` is today's date, in the user's timezone.


## Deploying the converted files
When the user asks how to put the converted files into NetSuite, refer them to Oracle's documentation instead of restating the steps, and offer to walk them through it. Before they begin, they should download a backup of the existing SuiteScripts folder (or the individual script file), and, if they have one, try the changes in a sandbox account first.
- **Uploading `SuiteScripts21.zip` or individual files:** [Oracle Article: Uploading Files to the File Cabinet](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_N542268.html). One detail to call out: when using "Advanced Add" with a zip file, the user should confirm that the Destination Folder says "SuiteScripts" and that both **"Unzip Files"** and **"Overwrite Files with Same Name"** are checked. Otherwise the zip will not be extracted, or the existing files will not be replaced. They should be inside the "SuiteScripts" folder when they do this: click the "SuiteScripts" folder in the left-hand tree first, then click "Advanced Add".- **SuiteScript 1.0 scripts:** after uploading, the existing Script record must be updated to use the converted file. See [Oracle Article: Converting a SuiteScript 1.0 Script to a SuiteScript 2.1 Script](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_160098641577.html).


## Additional Resources
- [Oracle Article: SuiteScript 2.1](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_8161516336.html)
- [Oracle Article: Transitioning to SuiteScript 2.1](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_7161922360.html)
- [Oracle Article: Converting a SuiteScript 1.0 Script to a SuiteScript 2.1 Script](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_160098641577.html)
- [Oracle Article: Uploading Files to the File Cabinet](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_N542268.html)
- [Oracle's AI Skill for Upgrading SuiteScript](https://github.com/oracle/netsuite-suitecloud-sdk/tree/25488efa43523be7dccb9fe6bbefdbd8ae7a5806/packages/agent-skills/netsuite-suitescript-upgrade)
- [Oracle Article: SuiteScript Upgrade Skill](https://github.com/oracle/netsuite-suitecloud-sdk/blob/master/packages/agent-skills/netsuite-suitescript-upgrade/SKILL.md)
- [Oracle Article: SuiteScript 1.0 API](https://system.netsuite.com/help/helpcenter/shared_resources/PDF/SuiteScript10API.pdf)
- [Oracle Article: Identifying Scripts](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_0827101556.html)
- [Oracle Article: Transitioning from 1.0 to 2.1](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_0908090519.html)
    - [Oracle Article: 1.0 to 2.1 API Mapping](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/chapter_4752722762.html)
    - [Oracle Article: Transitioning from 2.0 to 2.1](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_0826084435.html)
- [Oracle Article: Secrets Management](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_160216486846.html)
- [Rocky Mountain NetSuite User Group](https://rmnsug.org/)
    - Specifically, the November 2026 presentation/post "SuiteScript for Non-Scripters", located under [rmnsug.org/posts](https://rmnsug.org/posts/)

