# 🤖 AI Resume Builder — Backend

> A Spring Boot backend that uses **Ollama AI (DeepSeek R1)** running locally to generate professional, structured resumes from plain English descriptions. Just describe yourself in simple sentences — the AI does the rest!

---

## 📌 Table of Contents

- [What is this project?](#what-is-this-project)
- [How does it work? (Simple explanation)](#how-does-it-work-simple-explanation)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Complete Flow Diagram](#complete-flow-diagram)
- [API Endpoints](#api-endpoints)
- [Getting Started](#getting-started)
- [Understanding the Code](#understanding-the-code)
- [Resume Prompt Template](#resume-prompt-template)

---

## 📖 What is this project?

This is the **backend server** for the AI Resume Builder application.

**In simple words:**
- You type a few sentences about yourself in the app
- This backend sends your description to an AI model (DeepSeek R1)
- The AI reads your description and creates a properly structured resume
- The backend sends that resume data back to the frontend
- The frontend displays it as a beautiful resume that you can download as PDF

**No database needed** — this project works in real-time. User data is not stored anywhere!

---

## 🧠 How does it work? (Simple explanation)

Think of it like this:

```
YOU TYPE:
"I am YOUR_NAME , a Java developer from Pune.
 I know Spring Boot and React. I did B.Tech from XYZ college."

                    ⬇️ Sent to AI

AI UNDERSTANDS AND CREATES:
{
  "fullName": "YOUR_NAME",
  "location": "Pune",
  "skills": ["Java", "Spring Boot", "React"],
  "education": [{ "degree": "B.Tech", "university": "XYZ college" }]
}

                    ⬇️ Sent to Frontend

FRONTEND DISPLAYS:
┌─────────────────────────┐
│    YOUR_NAME            │
│    Pune                 │
│    Skills: Java...      │
│    Education: B.Tech... │
└─────────────────────────┘
```

**You write in plain English → AI converts to structured data → Frontend shows beautiful resume!**

---

## 🛠 Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 21 | Programming Language |
| Spring Boot | 3.4.1 | Backend Framework |
| Spring AI | 1.0.0-M5 | AI Integration Framework |
| Ollama | Latest | Local AI Model Runner |
| DeepSeek R1 | 1.5b | AI Language Model |
| Maven | 3.x | Build & Dependency Tool |

### What is Ollama?
Ollama is a tool that lets you run AI models **locally on your laptop** — no internet needed, no API cost, completely free! It downloads the AI model to your PC and runs it there.

### What is DeepSeek R1?
DeepSeek R1 is an AI language model (similar to ChatGPT) that understands text and can generate structured content. The `1.5b` version is a smaller, faster version suitable for local machines.

---

## 📁 Project Structure

```
ai-resume-builder-backend/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/resume/backend/
│   │   │       │
│   │   │       ├── controller/
│   │   │       │   └── ResumeController.java     # Handles API requests
│   │   │       │
│   │   │       ├── service/
│   │   │       │   ├── ResumeService.java         # Service interface
│   │   │       │   └── ResumeServiceImpl.java     # Business logic
│   │   │       │
│   │   │       ├── ResumeRequest.java             # Request data model
│   │   │       └── ResumeAiBackendApplication.java # Main class (entry point)
│   │   │
│   │   └── resources/
│   │       ├── application.properties             # Configuration file
│   │       └── resume_prompt.txt                  # AI prompt template
│   │
│   └── test/
│       └── ResumeAiBackendApplicationTests.java   # Test file
│
├── .gitignore
├── pom.xml                                        # Maven dependencies
└── README.md
```

### What does each file do?

| File | Role | Simple Explanation |
|------|------|--------------------|
| `ResumeController.java` | Controller | Receives the user's description from frontend |
| `ResumeService.java` | Interface | Defines what the service should do |
| `ResumeServiceImpl.java` | Service | Sends description to AI, gets response back |
| `ResumeRequest.java` | Model | Holds the user's description text |
| `resume_prompt.txt` | Template | Instructions given to AI about how to generate resume |
| `application.properties` | Config | Tells Spring Boot which AI model to use |

---

## 🔄 Complete Flow Diagram

### Step by Step — What happens when you click "Generate Resume"

```
┌──────────────────────────────────────────────────────────────────┐
│  STEP 1: User types description in browser                       │
│                                                                  │
│  "I am YOUR_NAME, Java developer, know Spring Boot and React..." │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               │ HTTP POST Request
                               │ /api/v1/resume/generate
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 2: ResumeController receives the request                   │
│                                                                  │
│  @PostMapping("/generate")                                       │
│  public ResponseEntity getResumeData(                            │
│       @RequestBody ResumeRequest resumeRequest)                  │
│                                                                  │
│  → Calls ResumeService                                           │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 3: ResumeServiceImpl processes the request                 │
│                                                                  │
│  1. Loads resume_prompt.txt template                             │
│  2. Fills {{userDescription}} with user's text                   │
│  3. Creates a complete prompt for the AI                         │
│                                                                  │
│  Example prompt sent to AI:                                      │
│  "You are an AI Resume Generator...                              │
│   Generate resume for: I am YOUR_NAME, Java developer..."        │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               │ Sends prompt to Ollama
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 4: Ollama AI (DeepSeek R1) runs locally                    │
│                                                                  │
│  localhost:11434                                                 │
│                                                                  │
│  AI reads the prompt and generates:                              │
│  <think>                                                         │
│    User is a Java developer from Pune...                         │
│  </think>                                                        │
│  ```json                                                         │
│  {                                                               │
│    "personalInformation": {                                      │
│       "fullName": "YOUR_NAME",                                   │
│       "location": "Pune"                                         │
│    },                                                            │
│    "skills": [{"title": "Java", "level": "Expert"}],             │
│    ...                                                           │
│  }                                                               │
│  ```                                                             │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 5: ResumeServiceImpl parses the AI response                │
│                                                                  │
│  parseMultipleResponses() method:                                │
│  → Extracts <think> content                                      │
│  → Extracts JSON from ```json block                              │
│  → Returns clean Map with "think" and "data" keys                │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               │ JSON Response
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 6: Frontend (React) receives JSON                          │
│                                                                  │
│  data.data.personalInformation.fullName → "YOUR_NAME"            │
│  data.data.skills → ["Java", "Spring Boot", "React"]             │
│  data.data.education → [{"degree": "B.Tech"...}]                 │
│                                                                  │
│  Resume.jsx fills empty slots with this data                     │
│  → Beautiful resume displayed on screen!                         │
│  → User can edit and download as PDF                             │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🔗 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/resume/generate` | Generate resume from description |

### Request Body:
```json
{
  "userDescription": "I am YOUR_NAME, a Java developer from Pune. I know Spring Boot, React and MySQL. I completed B.Tech from XYZ University in 2024."
}
```

### Response Body:
```json
{
  "think": "AI reasoning about the resume...",
  "data": {
    "personalInformation": {
      "fullName": "YOUR_NAME",
      "email": "",
      "phoneNumber": "",
      "location": "Pune",
      "linkedin": "",
      "gitHub": "",
      "portfolio": ""
    },
    "summary": "A passionate Java developer...",
    "skills": [
      { "title": "Java", "level": "Expert" },
      { "title": "Spring Boot", "level": "Intermediate" }
    ],
    "experience": [],
    "education": [
      {
        "degree": "B.Tech",
        "university": "XYZ University",
        "graduationYear": "2024"
      }
    ],
    "projects": [],
    "certifications": [],
    "languages": [],
    "interests": []
  }
}
```

---

## 🚀 Getting Started

### ⚠️ Prerequisites

Before running this project you need:
- Java 21
- Maven
- Ollama installed on your PC
- Minimum **8GB RAM** (AI model needs RAM to run)

---

### Step 1 — Install Ollama

Ollama is the tool that runs the AI model locally on your PC.

1. Go to 👉 **https://ollama.com/download**
2. Download for Windows/Mac/Linux
3. Install it (simple next → next → finish)

---

### Step 2 — Download DeepSeek AI Model

Open **Command Prompt** or **Terminal** and run:

```bash
ollama pull deepseek-r1:1.5b
```

This will download the AI model (~1GB). Wait for it to complete.

> 💡 This is a one-time download. After this, the model stays on your PC.

---

### Step 3 — Start Ollama

```bash
ollama serve
```

Ollama will start running on `http://localhost:11434`

> ⚠️ Keep this running in background — don't close this terminal!

---

### Step 4 — Clone & Run Backend

```bash
git clone https://github.com/Sushrut-Kadate/ai-resume-builder-backend.git
cd ai-resume-builder-backend
```

Open in **IntelliJ IDEA** → Open `ResumeAiBackendApplication.java` → Click **Run ▶**

Backend starts on: **http://localhost:8080**

---

### Step 5 — Run Frontend

```bash
git clone https://github.com/Sushrut-Kadate/ai-resume-builder-frontend.git
cd ai-resume-builder-frontend
npm install
npm run dev
```

Frontend starts on: **http://localhost:5173**

---

### Step 6 — Test It!

1. Open **http://localhost:5173**
2. Click **"Get Started"**
3. Type your description:
```
I am [Your Name], a Java developer from [City].
I know Java, Spring Boot, React and MySQL.
I completed B.Tech in Computer Science from [University] in [Year].
I have built projects like an ecommerce website.
My email is yourname@gmail.com
```
4. Click **"Generate Resume"**
5. Wait 10-30 seconds (AI is thinking!)
6. Your resume appears — edit and download as PDF!

---

## 🔧 Configuration

`src/main/resources/application.properties`:

```properties
spring.application.name=resume-ai-backend

# Ollama AI Configuration
spring.ai.ollama.chat.model=deepseek-r1:1.5b
spring.ai.ollama.base-url=http://localhost:11434
```

> No database configuration needed — this project has no database!

---

## 📝 Understanding the Code

### ResumeController.java
```java
@RestController
@CrossOrigin("*")                          // Allows frontend to call this API
@RequestMapping("/api/v1/resume")
public class ResumeController {

    @PostMapping("/generate")
    public ResponseEntity<Map<String, Object>> getResumeData(
            @RequestBody ResumeRequest resumeRequest) {
        // Calls service to generate resume
        // Returns JSON response to frontend
    }
}
```

### ResumeServiceImpl.java — The Brain
```java
@Service
public class ResumeServiceImpl implements ResumeService {

    // Step 1: Load the prompt template from resume_prompt.txt
    String promptString = this.loadPromptFromFile("resume_prompt.txt");

    // Step 2: Fill {{userDescription}} with actual user text
    String promptContent = this.putValuesToTemplate(promptString,
        Map.of("userDescription", userResumeDescription));

    // Step 3: Send to AI and get response
    String response = chatClient.prompt(prompt).call().content();

    // Step 4: Parse and return
    return parseMultipleResponses(response);
}
```

### resume_prompt.txt — Instructions for AI
This file tells the AI exactly how to generate the resume. It says:
- What JSON format to use
- What fields are required
- How to fill skill levels
- What NOT to do

The `{{userDescription}}` placeholder gets replaced with the actual user text before sending to AI.

---

## 🗂 Resume JSON Structure

The AI always generates resume in this exact structure:

```json
{
  "personalInformation": {
    "fullName": "", "email": "", "phoneNumber": "",
    "location": "", "linkedin": "", "gitHub": "", "portfolio": ""
  },
  "summary": "",
  "skills": [{ "title": "", "level": "Basic/Intermediate/Expert" }],
  "experience": [{
    "jobTitle": "", "company": "", "location": "",
    "duration": "", "responsibility": ""
  }],
  "education": [{
    "degree": "", "university": "", "location": "", "graduationYear": ""
  }],
  "certifications": [{ "title": "", "issuingOrganization": "", "year": "" }],
  "projects": [{
    "title": "", "description": "",
    "technologiesUsed": [], "githubLink": ""
  }],
  "languages": [{ "name": "" }],
  "interests": [{ "name": "" }]
}
```

---

## ⚠️ Important Notes

- Ollama must be **running before** starting the Spring Boot backend
- Minimum **8GB RAM** required for smooth performance
- First resume generation may take **20-30 seconds** (AI model loading)
- Subsequent requests are faster
- No internet needed once Ollama and model are installed

---

## 👨‍💻 Author

**Sushrut Kadate**
- GitHub: [@Sushrut-Kadate](https://github.com/Sushrut-Kadate)

---

## 🔗 Related Repository

- **Frontend:** [ai-resume-builder-frontend](https://github.com/Sushrut-Kadate/ai-resume-builder-frontend)
