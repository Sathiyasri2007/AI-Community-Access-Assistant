Voice4Schemes – AI Voice Assistant for Government Schemes
1. Project Overview

Voice4Schemes is a voice-first AI assistant that helps citizens access government welfare schemes via a simple phone call. It uses AI eligibility matching to provide personalized scheme recommendations in local languages and step-by-step guidance, making it accessible even without smartphones or internet.

2. Problem Statement

Millions of citizens miss out on government welfare schemes due to low literacy, language barriers, and limited internet access.
Existing apps/websites are often generic, app-dependent, and not multilingual, creating an accessibility gap.

3.Proposed Solution

Toll-free voice call interface
AI-based personalized eligibility matching
Multilingual and low-bandwidth friendly
Step-by-step guidance and optional SMS follow-ups
Reduces dependence on middlemen and prevents misinformation

4. Key Features

Voice-first access (no app or internet required)
AI eligibility engine (OpenAI GPT + rule-based logic)
Multilingual support and local language responses
Serverless, scalable architecture (AWS Lambda + API Gateway)
Fraud awareness and optional SMS/WhatsApp notifications

5. Technical Stack

Telephony/Voice: Twilio / Exotel
Speech Processing: OpenAI Whisper, Azure Neural TTS
AI Engine: OpenAI GPT API + Rule-Based Eligibility Engine
Database: PostgreSQL / FAISS (optional)
Backend: FastAPI (Python)
Cloud: AWS Lambda + API Gateway, Docker (optional)

Analytics: Firebase Analytics / AWS CloudWatch

Impact:

Bridges the awareness gap between citizens and government welfare schemes.
Empowers underserved communities by providing access without smartphones or internet.
Reduces dependence on middlemen, preventing misinformation and exploitation.
Ensures benefits reach the right people efficiently and accurately.
Inclusive & scalable solution: works for rural areas, multiple languages, and low-bandwidth environments.
Promotes digital inclusion and improves citizen engagement with public programs.
