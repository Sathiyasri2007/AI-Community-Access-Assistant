# Design Document: Voice-First AI Government Scheme Assistance System

## 1. System Overview

This document outlines the technical design for a voice-first AI system that provides government scheme assistance through toll-free calls. Citizens can call, speak in their preferred language, and receive personalized scheme information through natural voice conversations.

## 2. System Architecture

### 2.1 High-Level Architecture

```
User Call → Twilio (IVR) → OpenAI Whisper → FastAPI Backend → GPT API + Eligibility Engine → PostgreSQL → Azure TTS → Voice Response
                                ↓
                        AWS Lambda + API Gateway
                                ↓
                          CloudWatch Monitoring
```

### 2.2 Architecture Flow
1. User calls toll-free number
2. Twilio handles IVR and language selection
3. OpenAI Whisper converts speech to text
4. FastAPI processes request and coordinates services
5. GPT API understands intent and generates responses
6. Eligibility Engine matches user with relevant schemes
7. PostgreSQL provides scheme data
8. Azure TTS converts response to speech
9. Voice response delivered to user
10. All deployed on AWS Lambda with CloudWatch monitoring

## 3. Technology Stack

- **Telephony**: Twilio for call handling and IVR
- **Speech-to-Text**: OpenAI Whisper for multilingual recognition
- **AI Intelligence**: OpenAI GPT API for conversation and reasoning
- **Backend**: FastAPI (Python) for API development
- **Database**: PostgreSQL for scheme data storage
- **Text-to-Speech**: Azure Neural TTS for natural voice synthesis
- **Cloud Platform**: AWS Lambda + API Gateway for serverless deployment
- **Monitoring**: AWS CloudWatch for logging and metrics
- **Optional**: Docker for containerization, FAISS for semantic search

## 4. Component Design

### 4.1 Telephony Module
**Purpose**: Handle incoming calls and audio streaming
```python
class TelephonyModule:
    def handle_incoming_call(self, call_sid):
        # Process call and route to IVR
    
    def manage_ivr_flow(self, user_input):
        # Handle language selection
    
    def stream_audio(self, audio_data):
        # Stream audio to/from speech services
```

### 4.2 Speech Processing Module
**Purpose**: Convert speech to text and text to speech
```python
class SpeechProcessingModule:
    def transcribe_speech(self, audio, language):
        # OpenAI Whisper integration
    
    def synthesize_speech(self, text, language):
        # Azure Neural TTS integration
```

### 4.3 AI Intelligence Module
**Purpose**: Process natural language and generate responses
```python
class AIIntelligenceModule:
    def process_query(self, text, context):
        # GPT API for understanding and response
    
    def extract_user_info(self, conversation):
        # Extract age, income, location, etc.
```

### 4.4 Eligibility Engine
**Purpose**: Match users with relevant government schemes
```python
class EligibilityEngine:
    def assess_eligibility(self, user_profile, scheme):
        # Rule-based eligibility checking
    
    def find_matching_schemes(self, user_profile):
        # Return top 3 relevant schemes
```

### 4.5 Database Layer
**Purpose**: Store and retrieve government scheme information
```sql
-- Core Tables
CREATE TABLE government_schemes (
    id UUID PRIMARY KEY,
    name VARCHAR(255),
    description TEXT,
    category VARCHAR(100),
    eligibility_rules JSONB,
    benefits TEXT
);

CREATE TABLE call_logs (
    id UUID PRIMARY KEY,
    call_sid VARCHAR(100),
    duration INTEGER,
    language VARCHAR(10),
    schemes_discussed UUID[]
);
```

## 5. Sequence Flow

### 5.1 Detailed Sequence Diagram
```
User          Twilio       API Gateway    FastAPI      Whisper      GPT API      Eligibility    PostgreSQL    Azure TTS
 │               │              │           │            │            │           Engine           │             │
 │ 1. Dial       │              │           │            │            │             │             │             │
 │ Toll-free     │              │           │            │            │             │             │             │
 ├──────────────>│              │           │            │            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │ 2. IVR Menu   │              │           │            │            │             │             │             │
 │ (Language)    │              │           │            │            │             │             │             │
 │<──────────────┤              │           │            │            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │ 3. User       │              │           │            │            │             │             │             │
 │ Speaks        │              │           │            │            │             │             │             │
 ├──────────────>│              │           │            │            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │ 4. Webhook   │           │            │            │             │             │             │
 │               │ (Audio Data) │           │            │            │             │             │             │
 │               ├─────────────>│           │            │            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │ 5. Route  │            │            │             │             │             │
 │               │              │ Request   │            │            │             │             │             │
 │               │              ├──────────>│            │            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 6. Speech  │            │             │             │             │
 │               │              │           │ to Text    │            │             │             │             │
 │               │              │           ├───────────>│            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 7. Text    │            │             │             │             │
 │               │              │           │ Response   │            │             │             │             │
 │               │              │           │<───────────┤            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 8. Process │            │             │             │             │
 │               │              │           │ with GPT   │            │             │             │             │
 │               │              │           ├────────────────────────>│             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 9. AI      │            │             │             │             │
 │               │              │           │ Response   │            │             │             │             │
 │               │              │           │<────────────────────────┤             │             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 10. Check  │            │             │             │             │
 │               │              │           │ Eligibility│            │             │             │             │
 │               │              │           ├────────────────────────────────────>│             │             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 11. Query  │            │             │             │             │
 │               │              │           │ Schemes    │            │             │             │             │
 │               │              │           ├──────────────────────────────────────────────────>│             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 12. Scheme │            │             │             │             │
 │               │              │           │ Data       │            │             │             │             │
 │               │              │           │<──────────────────────────────────────────────────┤             │
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 13. Text   │            │             │             │             │
 │               │              │           │ to Speech  │            │             │             │             │
 │               │              │           ├────────────────────────────────────────────────────────────────>│
 │               │              │           │            │            │             │             │             │
 │               │              │           │ 14. Audio  │            │             │             │             │
 │               │              │           │ Response   │            │             │             │             │
 │               │              │           │<────────────────────────────────────────────────────────────────┤
 │               │              │           │            │            │             │             │             │
 │               │ 15. Stream   │           │            │            │             │             │             │
 │               │ Audio        │           │            │            │             │             │             │
 │               │<─────────────────────────┤            │            │             │             │             │
 │               │              │           │            │            │             │             │             │
 │ 16. User      │              │           │            │            │             │             │             │
 │ Hears AI      │              │           │            │            │             │             │             │
 │ Response      │              │           │            │            │             │             │             │
 │<──────────────┤              │           │            │            │             │             │             │
```

### 5.2 Step-by-Step Process
1. **User dials toll-free number** → Call routed to Twilio
2. **IVR presents language menu** → "Press 1 for Hindi, 2 for English"
3. **User speaks query** → "I need help with employment schemes"
4. **Twilio sends audio webhook** → Audio data sent to API Gateway
5. **FastAPI routes request** → Coordinates all processing
6. **Whisper converts speech** → "I need help with employment schemes"
7. **GPT processes intent** → Understands user needs employment assistance
8. **System asks for details** → "What is your age and location?"
9. **User provides info** → "I am 25 years old from Mumbai"
10. **Eligibility Engine evaluates** → Matches user profile with schemes
11. **Database query** → Retrieves relevant scheme details
12. **GPT generates response** → "You are eligible for PM Kaushal Vikas Yojana..."
13. **Azure TTS synthesizes** → Converts text to natural speech
14. **Audio streamed back** → Voice response delivered to user
15. **User receives information** → Personalized scheme guidance

## 6. Database Design

### 6.1 Key Tables
```sql
-- Government Schemes
government_schemes (id, name, description, category, eligibility_rules, benefits)

-- Call Logs  
call_logs (id, call_sid, duration, language, schemes_discussed)

-- User Sessions (temporary)
user_sessions (id, call_sid, collected_info, is_active)
```

### 6.2 Sample Data
```sql
INSERT INTO government_schemes VALUES 
('uuid1', 'PM Kaushal Vikas Yojana', 'Skill development for youth', 'employment', 
 '{"age": {"min": 18, "max": 35}, "income": {"max": 200000}}', 
 'Free training and job placement');
```

## 7. Deployment Architecture

### 7.1 AWS Lambda Functions
```
voice-ai-system/
├── call_handler/          # Handle Twilio webhooks
├── speech_processor/      # Whisper + Azure TTS
├── ai_intelligence/       # GPT API integration
├── eligibility_engine/    # Rule-based matching
└── shared/               # Common utilities
```

### 7.2 Configuration
```yaml
# Lambda Configuration
functions:
  call_handler:
    memory: 1024MB
    timeout: 30s
    
  speech_processor:
    memory: 2048MB
    timeout: 60s
```

### 7.3 API Endpoints
```python
@app.post("/webhook/call-status")
async def handle_call():
    # Process Twilio webhooks

@app.post("/webhook/speech-input") 
async def process_speech():
    # Handle speech processing

@app.post("/eligibility/check")
async def check_eligibility():
    # Assess user eligibility
```

## 8. Implementation Plan

### Phase 1 (Month 1): Infrastructure Setup
- AWS Lambda + API Gateway setup
- Twilio integration for basic call handling
- PostgreSQL database with sample schemes
- FastAPI backend structure

### Phase 2 (Month 2): AI Integration
- OpenAI Whisper for speech recognition
- GPT API for conversation handling
- Azure TTS for speech synthesis
- Basic eligibility engine

### Phase 3 (Month 3): Testing & Optimization
- End-to-end testing with sample calls
- Performance optimization
- Error handling and monitoring
- User feedback collection

### Phase 4 (Month 4): Documentation & Deployment
- Complete system documentation
- Final testing and validation
- Academic presentation preparation

## 9. Success Metrics

- **Technical**: Handle 50 concurrent calls, <10s response time, >85% speech accuracy
- **User Experience**: >80% task completion, >3.5/5 satisfaction rating
- **Academic**: Working prototype demonstration with all core features

## 10. Cost Estimation

- **AWS Lambda**: Free tier (1M requests/month)
- **OpenAI APIs**: ~₹2,000/month for testing
- **Azure TTS**: ~₹1,000/month for testing
- **Twilio**: ~₹1,500/month for toll-free number
- **Total**: ~₹5,000/month for academic project

This design provides a clear, implementable architecture for the voice-first AI government scheme assistance system suitable for academic project submission.