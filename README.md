# StudyMate

A cloud-backed study platform that turns uploaded PDFs into summaries, practice quizzes, and document chat. It groups course material by subject and records study sessions and quiz results for review.

## How the application works

| Area | Implementation |
| --- | --- |
| Interface | React, Vite, and Tailwind for sign-in, study views, PDF reading, and quiz interactions. |
| Accounts and files | AWS Amplify and Cognito for authentication, with S3 for subject PDFs and saved study state. |
| AI features | Express sends summary, quiz, and chat requests to Groq. |
| Study tools | Session tracking, quiz history, weak-topic review, a Pomodoro timer, and a task board. |

## Source guide

This repository contains source snapshots for major features.

| Area | Source |
| --- | --- |
| Frontend setup | [React, Vite, and Tailwind setup](<Project setup + Vite + React + Tailwind>) |
| Accounts | [Amplify and Cognito configuration](<AWS Amplify + Cognito config + Signup page>), [email verification](<OTP verification page>) |
| AI services | [Express backend](backend), [AI integration](AI_Utilization), [assistant interface](Assistant_Interference) |
| UI components | [Component snapshot](Components) |
