# StarRez Contribution Script Builder

A single-page tool for logging Intentional Conversations in UT Austin's StarRez in bulk.

Upload a spreadsheet (.xlsx or .csv) with a UTEID column and a notes column. The page builds a script you paste into the DevTools console of a logged-in StarRez tab. The script creates one Contribution/Interaction for each resident.

**Live page:** https://kingarthurdev.github.io/starrez-contribution-builder/

## Privacy

The sheet is read entirely in your browser. Nothing is uploaded, and the page has no server. The generated script runs in your own StarRez session and only talks to `utaustin.starrezhousing.com`.

## Using it

1. Open the page and upload your sheet. Pick the UTEID and Notes columns.
2. Set **Assigned to** (an EID; leave it blank to assign to yourself), the interaction type, sub type and room location. Optionally, list UTEIDs to skip.
3. Copy the script and paste it into the console of a logged-in StarRez tab (F12, then Console). By default it submits for real: it looks up every UTEID first and stops before creating anything if one isn't found. Then it creates the contributions and downloads a results CSV.
4. If it stops partway, run the same script again. It picks up where it left off.

To preview without creating anything, choose **Dry run** before copying. It prints what it would submit, with each student's name from StarRez.

## What the script does

For each row it:

1. Finds the student's StarRez entry by UTEID (`EntrySearch/GetSearchResults`).
2. Creates the contribution (`CampusLife/Contribution/New`) with the date, type, sub type, notes and student.
3. Sets Room Location, Comments and Assigned To (`CampusLife/ContributionMain/EditData`). If an assignee is set, it then checks the record's page to confirm the assignment saved.

## Safety

- If any UTEID isn't found, the script stops before submitting anything.
- It stops at the first failure and tells you what state that record is in.
- It remembers every student and note it has submitted from your browser (in the StarRez tab's localStorage). Running it again, even a rebuilt script, won't create duplicates.
- When it finishes, it downloads a results CSV with each new contribution ID.
