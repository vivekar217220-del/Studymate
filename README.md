# StudyMate

StudyMate is a web application prototype for studying from course PDFs. It organizes documents by subject, helps students turn reading material into summaries and quizzes, and tracks study activity in the same workspace.

## What a student can do

| Area | Workflow |
| --- | --- |
| Account | Sign up, confirm an email verification code, and sign in through Amazon Cognito. |
| Subject library | Upload PDFs to a subject, browse the file list, open the viewer, or remove a file. |
| Document study | Extract PDF text and request a summary or multiple-choice quiz. Ask a question using text drawn from a document as context. |
| Review | Check quiz history, study streaks, time by subject, and questions that are frequently missed. |
| Organization | Add document notes, run a Pomodoro session, and manage tasks on a Kanban board. |

## Architecture

```text
Student
  -> React and Tailwind interface
       -> Cognito for account flows
       -> Amplify Storage and S3 for PDFs and app-state snapshots
       -> PDF text extraction for document context
       -> Express AI handlers -> Groq
       -> Browser storage for study activity and notes

Express data handlers -> DynamoDB for profiles, app state, and quiz scores
```

The authentication snapshots show the sign-up, verification, and sign-in screens. The upload and file-list components use Amplify Storage to store and retrieve files by subject. For document-assisted tasks, the frontend extracts text from a PDF and supplies the relevant content to the AI request. The Express snapshots show handlers for chat, summarization, quiz generation, and DynamoDB-backed data operations.

Study activity also has a local path. The tracking hooks record sessions and scores in browser storage, and a separate cloud-sync hook shows how selected app state can be backed up to S3. The browser and backend persistence paths are both represented in the repository.

## Source guide

| Snapshot | What to look for |
| --- | --- |
| [Project setup](<Project setup + Vite + React + Tailwind>) and [Tailwind configuration](<setup Tailwind CSS and postcss configuration>) | Frontend tooling and styling setup |
| [Landing page and routing](<Landing page + routing setup>) | Public entry point and route structure |
| [Amplify and signup](<AWS Amplify + Cognito config + Signup page>), [email verification](<OTP verification page>), and [sign-in](<Login page + signIn flow>) | Cognito account workflow |
| [Components](Components) | PDF upload and viewer, document actions, dashboard analytics, and quiz UI |
| [AI utilities](AI_Utilization) | PDF text extraction and request helpers |
| [Assistant and state hooks](Assistant_Interference) | Context-aware chat, document notes, state sync, and study tracking |
| [Backend](backend) | Express handlers, Groq integration, and DynamoDB data handlers |

## Integration and verification

This repository contains source snapshots of the application. To assemble a runnable environment, place the snapshots into a conventional React and Express project structure, add dependency manifests, and configure your own AWS and AI service resources. Keep credentials in the runtime environment.

For an integrated build, verify the workflow in this order:

1. Create a test account, confirm the verification code, and sign in.
2. Upload a small text-based PDF to a test subject and open it in the viewer.
3. Request a summary and quiz, then compare them with the source PDF for relevance.
4. Complete the quiz and check that the result appears in study history.
5. Verify how your deployment restores state after a new session and handles missing document text or service failures.

Document-based answers depend on the quality of the extracted text. A scanned PDF may need text recognition before it provides useful context. Use sample documents when testing AI requests, since document text is sent for processing.
