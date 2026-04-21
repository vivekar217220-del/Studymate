📚 StudyMate — AI Student OS

An intelligent, cloud-native study platform that transforms the way students learn — upload PDFs, get AI summaries, auto-generate quizzes, track progress, and chat with a subject-aware AI tutor.


🎯 Purpose
StudyMate is built for students who want more than just file storage. The goal is a complete study operating system — one place to upload your notes, understand them faster with AI, test your knowledge, and track how you're actually improving over time.

🏗️ What We Built
Authentication & Security

AWS Cognito — full sign-up, OTP email verification, and sign-in flow
Protected Routes — unauthenticated users are redirected to login
Identity Pool — scoped, per-user private storage in S3

File Management

AWS S3 (via AWS Amplify Storage) — PDFs stored under private/{userId}/{subjectName}/
Upload with live progress, file listing, secure pre-signed URL sharing, and deletion
In-app PDF Viewer with iframe rendering

AI Features (Groq API — llama-3.1-8b-instant)

AI Summary — extracts key concepts and bullet-point summaries from any uploaded PDF
Quiz Generator — auto-generates 10 MCQ questions directly from document content
PDF Chat — ask anything about a specific document; AI cites which file it's drawing from
Global Chat Panel — subject-aware AI tutor that indexes all your uploaded PDFs across subjects
All AI calls go through the Express backend (/api/ai/*) which proxies to Groq securely

Study Analytics

Study Streak tracker with longest streak record
Session Timer — auto-records time spent per subject
Quiz Score History — saved locally, with per-subject performance breakdowns
Weak Topics Engine — surfaces frequently missed questions for spaced repetition
7-Day Activity Chart — bar chart of daily study minutes
Revision Plan — suggests when to revisit weak topics (1 / 3 / 7 day intervals)
AI Recommendations — surfaces low-performing subjects automatically

Productivity Tools

Pomodoro Timer — 25-minute focus / 5-minute break cycles with progress ring
Kanban Board — To Do / In Progress / Done task management, persisted in localStorage

Cloud Sync

useCloudSync hook syncs all studymate_* localStorage keys to S3 (app_state.json) every 10 seconds and on tab close — restores state on login

Notes

Per-PDF sticky notes, saved to localStorage, surfaced in the PDF Viewer sidebar


☁️ AWS Services Used
ServiceRoleCognito User PoolUser sign-up, OTP verification, sign-inCognito Identity PoolScoped IAM credentials for S3 accessS3PDF storage, custom subjects JSON, app state syncDynamoDBUser profiles, quiz score history, app state (backend)

🖥️ Backend (/backend)
Node.js + Express server that acts as a secure proxy:

POST /api/ai/chat — proxies chat messages to Groq
POST /api/ai/quiz — generates quiz JSON from text via Groq
POST /api/ai/summarize — summarizes text via Groq
GET /api/users/:userId/profile — fetches user profile from DynamoDB
POST /api/users/:userId/sync — syncs app state to DynamoDB
POST /api/users/:userId/scores — saves a quiz score to DynamoDB
GET /api/users/:userId/scores — fetches all quiz scores from DynamoDB

Stack: Express 5, AWS SDK v3, Axios, dotenv, CORS

⚛️ Frontend (/studymate)
Stack: Vite + React + Tailwind CSS + AWS Amplify v6
src/

├── aws/config.js   
# Amplify + Cognito + S3 config

├── pages/

│   ├── Landing.jsx

│   ├── Signup.jsx

│   ├── VerifyOTP.jsx

│   ├── Login.jsx

│   └── Dashboard.jsx      # Main app shell with tab navigation

├── components/

│   ├── UploadButton.jsx

│   ├── FileList.jsx        # PDF list + AI actions per file

│   ├── PDFViewer.jsx       # In-app viewer with Chat, Quiz, Notes sidebar

│   ├── GlobalChatPanel.jsx # Cross-subject AI chat

│   ├── AnalyticsDashboard.jsx

│   ├── ProductivityDashboard.jsx

│   ├── QuizModal.jsx

│   ├── ProtectedRoute.jsx

│   └── ErrorBoundary.jsx

├── hooks/

│   ├── useStudyTracker.js  # Streak, sessions, quiz scores

│   ├── useCloudSync.js     # S3-backed localStorage sync

│   ├── useNotes.js

│   ├── useChatManager.jsx

│   ├── usePersonalization.js

│   └── useRecommendations.js

└── utils/

    ├── groqClient.js       # Groq API wrapper
    
    ├── pdfTextExtractor.js # pdf.js text extraction
    
    ├── Quizgenerator.js
    
    └── summarize.js

🔑 Environment Variables
Backend .env

GROQ_API_KEY=...

PORT=5000

AWS_REGION=ap-south-1

AWS_ACCESS_KEY_ID=...

AWS_SECRET_ACCESS_KEY=...

Frontend .env

VITE_USER_POOL_ID=...

VITE_USER_POOL_CLIENT_ID=...

VITE_IDENTITY_POOL_ID=...

VITE_S3_BUCKET=...

VITE_S3_REGION=...

VITE_GROQ_API_KEY=...

🚀 Running Locally
bash# Backend
cd backend
npm install
npm run dev

# Frontend
cd studymate
npm install
npm run dev

✅ Core User Flow

Sign Up → OTP email verification via Cognito
Login → Dashboard loads with cloud-synced state
Upload PDFs → stored privately in S3 per subject
Summarize / Quiz / Chat → AI processes your actual documents
Track progress → streaks, session time, quiz scores, weak topics
Stay productive → Pomodoro timer + Kanban board


Built with React, Vite, Tailwind CSS, AWS Amplify, Cognito, S3, DynamoDB, Express, and Groq AI.
