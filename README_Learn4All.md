# Learn4All — HACK-AI-THON 2026

**Inclusive Learning Progress & Resource Planner**  
*Learn a little every day.*

Learn4All is the project name; **LearnTrack** is the application name used in parts of the source code. The prototype helps school learners practise syllabus-linked lessons, track progress, revisit weaker topics, and gives teachers a class-level view of learning activity.

> **Examiner note:** This README describes the supplied code, not an unverified promise. The application is a local browser-based prototype with a Python HTTP API and SQLite storage. If the event's final rules require a Tkinter-only interface, confirm compliance before submission: this build's interface is HTML, CSS, and JavaScript, not Tkinter.

## At a glance

- **Learners:** Classes 6–10
- **Catalogue:** 303 lesson-title/metadata entries in the current source catalogue
- **Learning loop:** lesson quiz → score review → mastery status → revision plan/retake
- **Mastery threshold:** 70% to mark a lesson mastered and unlock the next lesson in its sequence
- **Teacher workspace:** class overview, student monitoring, marks, and targeted school notices
- **Storage:** SQLite database created locally on first launch
- **Core runtime:** Python standard library; optional AI Study Buddy requires an internet connection and a Groq API key
- **Demo data:** fictional Class 6 learners, clearly labelled as demo accounts

The catalogue is grouped into Hindi, English, Math, History, Politics, and Science; Physics, Chemistry, and Biology can appear as filtered domains within integrated Science. Catalogue titles and metadata are not a reproduction of NCERT textbook content. The original practice questions still need teacher review for full lesson-by-lesson accuracy.

## Contents

- [Features](#features)
- [Technology and architecture](#technology-and-architecture)
- [Requirements](#requirements)
- [Run on Windows](#run-on-windows)
- [Run from a terminal](#run-from-a-terminal)
- [Optional AI Study Buddy](#optional-ai-study-buddy)
- [Demo accounts](#demo-accounts)
- [Run the tests](#run-the-tests)
- [Project structure](#project-structure)
- [Data, privacy, and security notes](#data-privacy-and-security-notes)
- [Known limitations](#known-limitations)
- [Curriculum and other references](#curriculum-and-other-references)
- [Team and credits](#team-and-credits)

## Features

### Student learning

- Student registration and sign-in, including class selection for Classes 6–10
- Lesson catalogue with subject/domain filters
- Lesson-level multiple-choice practice with review of selected and correct answers
- A 70% mastery threshold and sequential lesson unlocking
- My Plans for lessons needing revision, with retake actions
- Score history, progress trends, achievements, and XP
- Class-scoped learner leaderboard
- Study games and Daily Wordle
- Personal calendar, school routine, and printable progress report
- Focus Mode with bundled local soundscapes, session timing, and break reminders
- School updates with notice read status
- Optional AI Study Buddy, disabled when no API key is configured

### Teacher / school administration

- Class overview and topic heatmap
- Student monitoring and marks management
- School notices targeted to everyone, a class, or an individual learner
- Per-student read/pending status for notices
- Class leaderboard and printable learner progress information

### Explainable recommendations

The learning engine uses a set of rule-based recommendations and shows the reason for suggested next steps. It is not presented as a trained machine-learning model.

## Technology and architecture

| Layer | Implementation | Purpose |
|---|---|---|
| User interface | HTML, CSS, JavaScript | Student and teacher/admin screens in the browser |
| Local HTTP/API server | Python `http.server` (`ThreadingHTTPServer`) | Serves the interface and handles JSON API requests |
| Persistence | Python `sqlite3` / SQLite | Stores accounts, attempts, marks, plans, notices, and activity |
| Learning logic | Python modules | Quiz flow, progress analytics, recommendations, planning, and reporting |
| Optional AI | Groq chat completions from the server | AI Study Buddy responses when configured |
| Automated tests | Python `unittest` | Regression, demo-data, AI-client, API, and visual-rule checks |

The server binds to `127.0.0.1` by default, so it is intended for a local demonstration. Do not expose this prototype directly to the public internet.

## Requirements

- Python 3.8 or newer
- A modern web browser
- Windows users can use the supplied batch launcher; other systems can start the app from a terminal
- No third-party Python packages are required for the core local application
- Internet access and a user-supplied Groq API key are required only for the optional AI Study Buddy

## Run on Windows

1. Extract the project folder.
2. Open the `src` folder.
3. Double-click `RUN_LEARNTRACK.bat`.
4. Keep the terminal window open and visit **http://127.0.0.1:8765** in your browser.
5. Sign in using a demo account from [`DEMO_ACCOUNTS.md`](DEMO_ACCOUNTS.md) if the file is in the project root. If you received only the `src` folder, consult the accompanying examiner notes for the demo credentials.

The application creates `learntrack.db` in its working directory on first launch. Keep a copy of the original project if you want to preserve the supplied demo state.

## Run from a terminal

From the project's `src` directory:

```bash
python main.py
```

Then open:

```text
http://127.0.0.1:8765
```

To run the command-line demo/report mode:

```bash
python main.py --demo
```

To use a different local port, set the `PORT` environment variable before starting the app. The server still binds to loopback by default.

## Optional AI Study Buddy

The core application works without AI configuration. To enable the optional Study Buddy:

1. Copy `src/.env.example` to `src/.env` if the launcher has not already created it.
2. Add your own Groq API key as `GROQ_API_KEY`.
3. Optionally set `GROQ_MODEL` to a model available to your Groq account.
4. Restart the application.

The key is read by the Python server and should never be placed in browser code, screenshots, documentation, or a shared ZIP. Keep `src/.env` private and do not commit it. The example file is a template only. If no key is configured, the rest of the app remains usable and the AI Study Buddy stays unavailable.

## Demo accounts

Demo account IDs and passwords are documented in `DEMO_ACCOUNTS.md` in the supplied project package. They are fictional, local-examination credentials only. The default school-admin account is a demonstration account, not a production identity. Change or disable default credentials before any shared deployment. Do not use the prototype with real student information.

All preloaded learner names and learning histories are fictional. Their scores, streaks, and rankings are not evidence of educational impact.

## Run the tests

From the `src` directory:

```bash
python -m unittest discover -s tests -v
```

**Verification performed for this packaged source snapshot:** 30 tests passed with `python -m unittest discover -s tests -v` in the available review environment. This is a repeatable unit/regression-test result, not a claim that every browser workflow, operating system, or live third-party AI request has been tested. Run the command again on the exact package you submit and report the result from that run.

The current suite includes checks for:

- Demo dataset consistency and repeat-safe seeding
- Student/teacher authorization for protected API routes
- Course catalogue IDs and quiz generation
- Mastery progression and XP behaviour
- Registration and malformed/oversized request validation
- Session expiry and static-route/source-file protection
- Focus Mode validation and local audio handling
- Heatmap readability/accessibility-related visual rules
- Groq client configuration using mocked responses (no real provider call)

## Project structure

```text
LearnTrack_Final_Contrast_Heatmap_Fix/
├── README.md
├── DEMO_ACCOUNTS.md
├── QUALITY_AUDIT_REPORT.md
└── src/
    ├── main.py                 # Application entry point
    ├── server.py               # Local HTTP server and JSON API
    ├── storage.py              # SQLite persistence and account operations
    ├── models.py               # Domain model classes
    ├── validation.py           # Input validation
    ├── analytics.py            # Scores, trends, and performance summaries
    ├── engine.py               # Explainable recommendation rules
    ├── planner.py              # Weekly study planning
    ├── curriculum.py           # Lesson catalogue metadata
    ├── course_quizzes.py       # Lesson quiz generation and scoring flow
    ├── lesson_support.py       # Lesson support content
    ├── reporting.py            # Progress reports
    ├── groq_client.py          # Optional server-side AI client
    ├── india_calendar.py       # Calendar dates and holidays
    ├── school_schedule.py      # Routine/schedule logic
    ├── wordle.py               # Daily Wordle logic
    ├── static/
    │   ├── index.html          # Browser UI (HTML/CSS/JavaScript)
    │   └── audio/              # Bundled Focus Mode soundscapes
    └── tests/                  # Automated unit/regression tests
```

## Data, privacy, and security notes

- The application is configured for local use and binds to `127.0.0.1` by default.
- Demo data is fictional; do not import real learner records for the hackathon demonstration.
- Passwords are stored as hashes in the application database; do not describe this as a substitute for a full production security review.
- Server-side role checks are covered by tests for selected protected endpoints. The test suite is not a penetration test and does not establish production readiness.
- Password recovery currently displays a demo OTP on screen; it does **not** send an SMS. This is not suitable for real account recovery.
- Optional AI prompts are sent to the configured external provider when the feature is used. Do not submit private student data to the AI feature.
- `src/.env` may contain a secret. It must be excluded from shared packages and version control. The `.gitignore` file excludes it from Git, but ZIP creation tools can still accidentally include it.

## Known limitations

- The course catalogue stores lesson titles and metadata, not full textbook content.
- The question banks are original practice questions and require teacher review for complete chapter-level accuracy; they are not official NCERT question banks.
- Some class/subject mappings—especially language tracks and annual syllabus details—may vary and require teacher confirmation.
- The calendar's published central-government holidays may not match every state or school calendar.
- Password recovery is a demonstration flow only; no SMS is sent.
- The AI Study Buddy requires internet access and a valid Groq key; it is optional and not required for core use.
- The project has automated regression coverage, but a full classroom pilot, broad browser/OS compatibility check, accessibility audit, and production security review remain outside the evidence claimed here.

## Curriculum and other references

- NCERT textbook portal: https://ncert.nic.in/textbook.php
- CBSE curriculum 2026–27: https://cbseacademic.nic.in/curriculum_2027.html
- India Post holiday list: https://www.indiapost.gov.in/holidays-list
- National Portal of India calendar: https://www.india.gov.in/calendar
- Government of India central holiday information: https://cgca.gov.in/ccadl/list-of-holiday
- United Nations Sustainable Development Goals: https://sdgs.un.org/goals
- Python documentation (`http.server`, `sqlite3`, `unittest`): https://docs.python.org/3/
- Groq Console: https://console.groq.com/

External sources and curriculum details should be rechecked against the current official pages before real classroom use. NCERT/CBSE linkage in this prototype means lesson/book metadata references, not official endorsement or certification.

## Team and credits
## Credits and Acknowledgements

**Project: Learn4All — Inclusive Learning Progress & Resource Planner**
**Event: Myra Global Tech HACK-AI-THON 2026**

### Core Development Team

**Aaditya Sorout — Developer**
Contributed to the development and implementation of the Learn4All application alongside Syed Sarfaraz Ahamed.

**Syed Sarfaraz Ahamed — Developer**
Contributed to the development and implementation of the Learn4All application alongside Aaditya Sorout.

### Research and Foundation

**Pramveer Kumar — Foundation Code and Ideas**
Contributed foundational code and initial ideas that supported the project's development.

**Sarah — Research**
Contributed to the research supporting the project.

### Additional Acknowledgements

We sincerely thank **Mohamed Aathif** for contributing foundational code and ideas, **Mohamed Rayan M** for video and team support, **Sriyansh Sai Mohanty** for sharing an idea, and **M. Vetri Selvan M** for being part of the project team.

We are grateful to everyone who contributed their time, ideas, research, and support to Learn4All. Their contributions helped us develop our concept of a more inclusive, structured, and student-centred learning experience.

### Project Vision

Learn4All was developed with the vision of making learning more accessible, encouraging consistent practice, and helping students and teachers understand learning progress more effectively. This project reflects our commitment to **SDG 4: Quality Education** and **SDG 10: Reduced Inequalities**.


*