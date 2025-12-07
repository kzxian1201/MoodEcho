# MoodEcho AI - Your Mindful Habit Companion

MoodEcho is a full-stack, AI-powered habit tracking application designed to correlate daily habits with emotional well-being. Unlike traditional trackers that simply log "what you did," MoodEcho analyzes "how you felt" using Generative AI to provide actionable mental health insights.

> **Note for Reviewers:**
> Due to the robust enterprise-grade architecture (Java Spring Boot + PostgreSQL + AI Integration), this application is **containerized using Docker** for consistency and scalability.
> While a live URL is not provided due to cloud resource constraints for Java backends, **the application is fully Docker-ready**.

---

## 🚀 Key Features

* **AI-Powered Journaling**: Uses **Google Gemini 2.5 Flash** to analyze journal entries, generating a mood score (1-10) and a personalized summary.
* **Smart Consolidation**: Automatically consolidates unstructured text data (Journal) with structured data (Habits) into a unified database record.
* **Visual Analytics**: Interactive Area Charts to visualize mood trends over time.
* **Gamified Streaks**: Tracks daily streaks for habits to boost motivation.
* **Mobile-First Design**: Responsive UI optimized for mobile experience using Tailwind CSS.
* **Resilient Architecture**: Implements graceful degradation—if AI services fail, user data is safely preserved.

---

## 🛠️ Tech Stack

* **Frontend**: React (TypeScript), Tailwind CSS, Vite, Recharts.
* **Backend**: Java (JDK 17), Spring Boot 3.5, Spring Data JPA.
* **Database**: PostgreSQL 15.
* **AI Engine**: Google Gemini API (Model: `gemini-2.5-flash`).
* **DevOps**: Docker, Docker Compose (Multi-container orchestration).

---

## 🏗️ Technical Architecture & Thought Process

### 1. Why Spring Boot & PostgreSQL?
Instead of using lightweight BaaS (Backend-as-a-Service), I chose a structured **Controller-Service-Repository** architecture. This ensures strict data validation, transactional integrity for the database, and scalability suitable for enterprise environments.

### 2. The "Consolidation" Logic
The core requirement was to consolidate input into the backend. I implemented this in the `JournalController`:
1.  **Receive**: JSON payload (Journal text + Habits list).
2.  **Process**: Asynchronously call `AiService` to extract sentiment.
3.  **Merge**: Combine user input + AI result + Timestamp.
4.  **Persist**: Save as a single transactional entity into `daily_entries` and `habit_logs` tables.

### 3. AI Resilience (Graceful Degradation)
I implemented a `try-catch` fallback mechanism. If the AI API experiences latency or downtime, the system catches the exception, logs it, and saves the user's entry with a default "AI is resting" status, ensuring **zero data loss** for the user.

---

## 📦 How to Run (Docker - Recommended)

Pre-requisites: Docker Desktop installed.

1.  **Clone the repository**
    ```bash
    git clone [https://github.com/kzxian1201/MoodEcho.git](https://github.com/kzxian1201/MoodEcho.git)
    cd MoodEcho
    ```

2.  **Start the Application**
    Run the following command in the root directory. This will build the Backend (Java), Frontend (Node/Nginx), and Database (Postgres) containers.
    ```bash
    docker-compose up --build
    ```

3.  **Access the App**
    * Frontend: `http://localhost:5173`
    * Backend API: `http://localhost:8081/api/entries`

---

## 🔧 How to Run (Manual Setup)

If you prefer running without Docker:

**1. Database**
* Ensure PostgreSQL is running on port `5432`.
* Create a database named `moodecho_db`.
* Update `backend/src/main/resources/application.properties` with your DB credentials.

**2. Backend**
```bash
cd backend
./mvnw spring-boot:run
```

**3. Frontend**
```bash
cd frontend
npm install
npm run dev
```
