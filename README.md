# Incomplete Progress Notes Checker

A self-contained HTML tool I built as a billing and insurance coordinator at a mental 
health practice to automate a recurring, manual weekly task: reminding clinicians to 
complete outstanding progress notes.

## The problem

- Progress notes have to be completed within a set window after each session
- Every week, someone has to pull an appointment status report, manually find every 
  incomplete or unlocked note, figure out which clinician it belongs to, and send 
  each one a personalized reminder email
- Doing this by hand for a full caseload, one clinician at a time, is slow and easy 
  to get wrong (missed notes, wrong recipient, inconsistent wording)

## How it works

1. Drop in one or more appointment status report exports (`.xlsx`)
2. The tool parses the spreadsheet client-side (nothing is uploaded anywhere — it 
   all runs in your browser) and filters to sessions that:
   - Have a billing code that requires a note (e.g. intake or therapy session codes)
   - Have a status of "Unlocked" or "No Note"
3. Pick a clinician from a dropdown to preview their personalized reminder email — 
   subject, recipient, and a list of every outstanding note with client and date
4. Click "Open in Gmail" to launch a pre-filled compose window, or use "Send to 
   everyone" to open one Gmail tab per clinician with outstanding notes, spaced a 
   few seconds apart so each can be reviewed before sending

## Configuration

All organization-specific details (email domain, manual email overrides, excluded 
staff, CC address, and sender signature) live in a single `CONFIG` object at the top 
of the script — nothing else needs to be touched to adapt this to a different team.

```js
const CONFIG = {
  emailDomain: 'example.com',
  emailOverrides: {},
  excludedClinicians: [],
  ccAddress: 'supervisor@example.com',
  senderName: 'Your Name',
  senderTitle: 'Billing & Insurance Coordinator',
  orgName: 'Your Organization'
};
```

## Why this approach

- **No backend needed.** Everything — spreadsheet parsing, filtering, email 
  generation — happens client-side in the browser using the 
  [SheetJS](https://sheetjs.com/) library, so there's nothing to host or maintain.
- **Emails are opened for review, never sent automatically.** The tool pre-fills 
  Gmail's compose window; a human still reviews and clicks send on every email. 
  Nothing is sent silently or without a final look.
- **Handles edge cases explicitly** — clinicians who need a different email pattern, 
  staff who should be excluded entirely, and a manual override field for any 
  one-off case not caught by the standard rules.

## Note on this version

This is a sanitized portfolio version. The original was built and used with a real 
organization's clinician list and email addresses; those have been replaced with a 
generic `CONFIG` block here so this can be shared publicly without exposing anyone's 
real contact information.

## Stack

Vanilla JavaScript, HTML/CSS, [SheetJS](https://sheetjs.com/) (client-side Excel 
parsing), Gmail compose URL scheme
