# ClassBridge — AI-Powered Course Recovery & Learning Workspace

ClassBridge helps learners turn course notes into a focused learning path. Upload a text-based PDF, explore the topics detected in it, take a baseline diagnostic, ask course-grounded questions, practice at different difficulty levels, and track topic mastery to decide what to revisit next.

> **Current prototype scope:** The included app is an individual learner workspace. It does not yet implement the originally envisioned teacher workflow for grouping an entire classroom by misconception, and scanned PDFs requiring OCR are not supported by the current text-extraction path.

## Problem
Learners often finish reading a chapter without knowing which concepts they understand, which need revision, or what to practice next. Generic quizzes do not always connect feedback to the learner's own course material.

## What works in this prototype
- **Sample course mode:** Explore a preloaded DBMS course without configuring an AI provider.
- **Course upload:** Upload a text-based PDF (maximum 10 MB); the backend extracts its text and page count.
- **Topic mapping:** Build a topic list from course material.
- **Baseline diagnostic:** Answer a quiz to estimate starting understanding by topic.
- **Course-grounded teacher:** Ask questions scoped to a topic and retrieve relevant passages from the uploaded material. When configured, an OpenAI-compatible chat-completions endpoint can generate an answer; otherwise a local retrieval-based fallback is used.
- **Focused practice:** Generate topic-specific questions at Easy, Medium, or Hard difficulty.
- **Mastery and recommendations:** Track topic scores and receive a suggested next action.
- **Learning history:** Store course, quiz, mastery, and activity data in SQLite.
- **Dark and light themes.**

## Tech stack
- **Frontend:** React, TypeScript, Vite, Tailwind CSS, TanStack Query, Wouter
- **Backend:** Node.js, TypeScript, Express
- **Data:** SQLite (Node's built-in `node:sqlite` API); shared API schemas generated with Zod
- **Document processing:** Poppler utilities (`pdftotext`, `pdfinfo`) for text-based PDFs
- **Optional AI:** OpenAI-compatible Chat Completions API; not required for demo mode

## Architecture
1. The React/Vite frontend calls the `/api` endpoints.
2. The Express server validates requests and routes learning actions.
3. PDF text is extracted server-side, then topics and relevant text chunks are identified.
4. Quiz attempts and topic mastery are stored in SQLite.
5. Question answering uses retrieved course text and an optional configured model, with a local fallback when no model is configured or the call fails.

## Run locally

Requirements:
- Node.js 22 or a compatible version supporting `node:sqlite`
- `pnpm` (the repository uses a pnpm workspace)
- Poppler utilities installed and available on `PATH` (`pdftotext` and `pdfinfo`) for PDF uploads

Install dependencies:

```bash
pnpm install
```

Start the API server in one terminal (port 3001):

```bash
PORT=3001 pnpm --filter @workspace/api-server dev
```

Start the frontend in another terminal (set the required Vite environment values):

```bash
PORT=5173 BASE_PATH=/ pnpm --filter @workspace/classbridge dev
```

Open `http://localhost:5173`. If your environment requires different port or base-path settings, use the matching values in `artifacts/classbridge/vite.config.ts` and configure `API_SERVER_URL` if the API is not on `http://127.0.0.1:3001`.

Optional live AI configuration (not required for demo mode):

```bash
OPENAI_API_KEY=your_key
OPENAI_MODEL=gpt-4.1-mini
OPENAI_BASE_URL=https://api.openai.com/v1
```

Set these environment variables for the API server process. Never commit real API keys to GitHub.

## Testing
The backend learning-engine tests can be run with:

```bash
pnpm --filter @workspace/api-server test
```

The source test suite was run during submission preparation with Node's test runner: **14 tests passed, 0 failed**. These are engine/unit tests, not a complete browser-to-database end-to-end test.

## Limitations
- PDF uploads currently support text-based PDFs only; scanned/image-only PDFs need OCR and are not supported by the current extraction flow.
- The optional AI provider needs a valid key and network access. Demo mode remains available without one.
- This prototype currently tracks one demo learner's course and progress; multi-user classroom management and teacher-side misconception grouping are not implemented.
- Run a fresh-install smoke test in the target deployment environment before relying on a public demo link.

## Hackathon track
**AI + Education** — a learning-support tool that connects course material, assessment, targeted practice, and mastery feedback.

## Demo flow (2–4 minutes)
1. Open the welcome screen and start the sample DBMS course.
2. Show the overview and topic mastery map.
3. Start the baseline diagnostic and answer a few questions.
4. Open AI Teacher, ask a question directly related to the selected course topic, and point out the source-grounded answer.
5. Generate a focused practice set and submit answers.
6. Return to the mastery map to show updated progress and the next recommended action.
7. If upload works in the demo environment, upload a text-based PDF and show the extracted topic map. Keep the sample-course flow as a fallback.

## Links to add before submitting
- Live demo: _add verified public URL_
- Source code: _add public GitHub repository URL_
- Demo video (public, 2–4 minutes): _add YouTube or equivalent URL_

## Credits and transparency
Built with React, TypeScript, Vite, Express, SQLite, and open-source libraries. Optional AI generation uses an OpenAI-compatible API when configured. The app can run its sample learning flow without a model API key.
