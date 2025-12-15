# Interview Assistant Angular Requirements Specification

## Overview
An interview assistant web application that helps interviewers run structured technical interviews. The app follows the layout shown in the provided design, with a focus on streamlined navigation between questions, rich candidate state tracking, and guided evaluation.

## Goals
- Keep interviewers focused on the candidate by surfacing only the information they need during the session.
- Make it easy to browse question packs, track candidate progress, and capture structured feedback.
- Provide a repeatable rubric-driven evaluation to reduce bias.
- Support note-taking, skill tagging, and summarizing feedback for post-interview review.

## Personas
- **Interviewer**: Runs a live interview session, navigates questions, records notes and evaluations.
- **Hiring panel reviewer**: Reads the captured answers, scores, and notes after the interview.
- **Content curator**: Adds or updates question packs and evaluation rubrics.

## High-level user flows
1. Start a new session from the dashboard and select a question pack.
2. Navigate through questions in a pack via the side navigation and next/previous controls.
3. Read the prompt, scenario details, and answer guidance; expand/collapse sections as needed.
4. Capture rubric selections and an overall answer in structured fields.
5. Record notes and visible feedback; tag or rate skills demonstrated.
6. Submit or save progress, then review or export the session transcript.

## Functional requirements
### Navigation & session context
- Display current candidate name, role, and profile quick actions (view profile, edit session, view transcript).
- Show breadcrumb/section info and a progress tracker for the current question pack (e.g., "Current section: Fundamentals", next section CTA).
- Side navigation lists all questions in the pack with status indicators (unseen/in-progress/completed). Clicking navigates without losing state.
- Top-level actions for ending session and accessing dashboard/question pack library.

### Question workspace
- Present the question title, category/skills tags, and scenario diagram or assets when available.
- Allow expandable guidance section detailing expected answers, hints, or interviewer notes.
- Provide an evaluation rubric with selectable options; allow multiple selections when specified.
- Provide an answer text area for free-form notes with character counting and autosave.
- Include action buttons for submitting the current question evaluation and moving to the next.

### Notes & feedback
- Dedicated notes panel with plain text entry and autosave.
- Visible feedback section to draft post-interview feedback/snippets.
- Skills tagging widget to rate or tag observed skills per question or overall.

### State management & persistence
- Autosave answers, rubric selections, notes, and skills tags locally and remotely (API) to avoid data loss on navigation.
- Preserve per-question state when switching between questions.
- Track completion status per question and overall progress.

### Accessibility & usability
- Keyboard navigation for moving between questions and focusing primary controls.
- ARIA labels for all form inputs and controls.
- Color contrast compliant with WCAG AA for text and interactive elements.

## Non-functional requirements
- **Framework**: Angular (latest LTS) with TypeScript, using standalone components or an AppModule as project dictates.
- **State**: Use RxJS signals/observables or NgRx for session state; persist via a data service that abstracts API calls.
- **Routing**: Angular Router for session layout and deep-linking to specific questions via URL params.
- **Styling**: Use a design system compatible library (e.g., Angular Material) and maintain CSS variables or SCSS theming for brand colors.
- **Testing**: Provide unit tests for components/services and integration tests for critical flows (navigation, autosave, rubric selection) with Jasmine/Karma or Jest.
- **Performance**: Lazy-load heavy modules/assets, ensure initial load < 3s on standard hardware.
- **Security**: Role-based access guard for interviewer vs. reviewer, with secure API calls (JWT/bearer tokens) and sanitized rich text inputs.

## Data model (conceptual)
- **Candidate**: id, name, role, profile link, session metadata.
- **QuestionPack**: id, name, section order, questions[].
- **Question**: id, text, category, tags, assets (diagram/image), rubric[], answerGuidance.
- **SessionState**: currentQuestionId, answers[], notes[], skills[], progress.
- **EvaluationEntry**: questionId, rubricSelection[], answerText, feedback, timestamps.

## API integration requirements
- Fetch candidate/session details and question packs at session load.
- Endpoints to save question-level state (answers, rubric, notes, skills) and session-level summaries.
- Optimistic updates with retry/backoff on failure; surface non-blocking toasts for network errors.

## UI components (Angular)
- **ShellLayoutComponent**: header with navigation, candidate info, and session actions.
- **QuestionNavComponent**: side navigation for questions with status badges.
- **QuestionWorkspaceComponent**: renders prompt, assets, guidance, and evaluation controls.
- **RubricSelectorComponent**: selectable checklist/radio groups supporting multi-select.
- **AnswerEditorComponent**: text area with character count and autosave indicator.
- **NotesPanelComponent**: notes and visible feedback inputs with save status.
- **SkillsTaggerComponent**: chips/dropdowns for tagging skills with rating.
- **ProgressFooterComponent**: shows current section and next-section CTA.

## Acceptance criteria
- Interviewer can move between questions without losing typed content or rubric choices.
- Rubric selections and notes sync within 2s of change; offline/temporary failures show a retry state.
- Skills tags and notes are visible in the transcript/review view.
- Screen mirrors the provided design layout: left question nav, centered workspace with rubric and answer fields, right notes/skills panel, top header with candidate/session controls, and footer progress/CTA.
- All inputs have keyboard focus styles and accessible labels.

## Open questions
- Should transcripts include audio/video attachments?
- Do rubric options differ by role or pack, and are they editable during a session?
- What is the desired autosave frequency vs. manual save control?
