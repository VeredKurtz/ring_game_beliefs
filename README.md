# Ring Game belief pilot - Prolific deployment

Production package for the online pilot.

## Prolific redirects already configured

- Successful completion: CELC7P0O
- Screened out after two failed Ring Game comprehension attempts: CD7I6NVT

The app captures these URL parameters when Prolific sends them:
`PROLIFIC_PID`, `STUDY_ID`, and `SESSION_ID`.

## Data

Participant state is saved server-side in SQLite after major checkpoints and every prediction trial. The final Prolific redirect occurs only after a successful server save.

On Render, mount a persistent disk at `/var/data` and set `DATA_DIR=/var/data`.

## Researcher/export access

Set a long random `ADMIN_TOKEN` environment variable. Then use:

- Researcher preview: `https://YOUR-SITE/researcher?token=YOUR_ADMIN_TOKEN`
- Summary CSV: `https://YOUR-SITE/admin/export/summary.csv?token=YOUR_ADMIN_TOKEN`
- Trial-level CSV: `https://YOUR-SITE/admin/export/trials.csv?token=YOUR_ADMIN_TOKEN`
- Raw JSON backup: `https://YOUR-SITE/admin/export/raw.json?token=YOUR_ADMIN_TOKEN`

Never share the admin-token URLs with participants.

## Local production test

```bash
ADMIN_TOKEN=test-secret DATA_DIR=./data python3 server.py
```

Open participant view:
`http://127.0.0.1:8000/?PROLIFIC_PID=TEST123&STUDY_ID=TESTSTUDY&SESSION_ID=TESTSESSION`

Open researcher view:
`http://127.0.0.1:8000/researcher?token=test-secret`

## v3 consent update
- The ethics-approved informed-consent form is now the first participant-facing screen.
- No server-side research record is created before affirmative consent.
- Participants who decline consent are shown an exit message and no data are sent to the server.
- Affirmative consent and its timestamp are included in server exports.
