# SRAIS Action Hub

SRAIS Action Hub turns completed SRAIS assessments into reviewable improvement plans, owned actions, and evidence of progress.

## What it does

- Receives a short-lived token from SRAIS after a respondent opens their completed submission.
- Displays the submitted RAI and SAI scores.
- Uses deterministic score rules and de-identified response gaps to ground recommendations.
- Uses Mistral to draft a structured action plan.
- Creates action-tracker items from AI recommendations.
- Supports owner assignment, target dates, task status, and evidence links.
- Lets teams edit or remove tracker tasks and evidence records when plans change.
- Gives SRAIS administrators an **Action Hub** portfolio tab with actions, progress, overdue work, and evidence counts across submissions.
- Stores plans, tasks, and evidence in the existing SRAIS Google Spreadsheet.

AI output is a draft for human review. SRAIS remains the source of truth for assessment scores.

## Files to deploy

| File | Destination |
| --- | --- |
| `srais-action-hub.html` | A separate GitHub Pages repository, renamed to `index.html`. |
| `SRAIS_Action_Hub_AppsScript.js` | The existing SRAIS Apps Script project. |
| `SRAIS_Tool_working_copy.html` | The existing SRAIS GitHub Pages repository, renamed/replaced as its deployed `index.html`. |

## Initial Apps Script setup

1. Back up the current Google Spreadsheet.
2. Copy `SRAIS_Action_Hub_AppsScript.js` into the existing Apps Script project and save.
3. Select `setupSpreadsheet` from the Apps Script function dropdown and click **Run** once.
4. Approve the permissions request.
5. Confirm these tabs exist in the same spreadsheet:
   - `Action Hub Plans`
   - `Action Hub Tasks`
   - `Action Hub Evidence`
6. Deploy a new version of the existing Web App and keep its `/exec` URL.

## Mistral configuration

In Apps Script **Project Settings** → **Script properties**, add:

| Property | Value |
| --- | --- |
| `MISTRAL_API_KEY` | A Mistral API key. Never add it to GitHub or an HTML file. |
| `MISTRAL_MODEL` | `mistral-small-latest` for faster drafts, or `mistral-large-latest` for higher-quality final drafts. |

The Apps Script project must include the `https://www.googleapis.com/auth/script.external_request` OAuth scope for Mistral requests.

## Connect the two sites

In `srais-action-hub.html`, set:

```javascript
const APPS_SCRIPT_URL = 'YOUR_APPS_SCRIPT_EXEC_URL';
```

In `SRAIS_Tool_working_copy.html`, set:

```javascript
const ACTION_HUB_URL = 'YOUR_ACTION_HUB_GITHUB_PAGES_URL';
```

After every code update, publish the updated HTML file to GitHub Pages and deploy a new Apps Script version when the Apps Script code changes.

## Admin portfolio view

After signing into SRAIS as an administrator, open **Submissions & reports** and select the **Action Hub** tab. It provides a portfolio-level view of each submission's RAI/SAI scores, action totals, completion progress, overdue items, and evidence count. Use **Refresh** after colleagues update work in the Action Hub.

## Responsible use

- Do not send API keys, passwords, or access codes in URLs or GitHub commits.
- Treat AI recommendations as suggestions, not legal, regulatory, or security determinations.
- Require a human reviewer before marking an action complete.
- Store links to evidence rather than sensitive documents wherever possible.
- Reassess the AI solution after material changes to its data, model, users, or deployment context.
