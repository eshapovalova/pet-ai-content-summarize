# AI-Powered Content Summarizer &amp; Categorizer (.NET + React)

## 💡 Introduction idea
* **Concept:** Build a web application where users can input large blocks of text (e.g., articles, meeting transcripts, long emails). The backend, powered by .NET 9, would integrate with an AI model (like OpenAI's GPT or a local model via Microsoft.Extensions.AI and Ollama if you want to experiment locally) to summarize the text and categorize it (e.g., "Technology," "Finance," "Health").
	* **.NET 9 Focus:**
		* Leverage Microsoft.Extensions.AI for seamless AI integration.
		* Implement Minimal APIs for a lean and efficient backend.
		* Utilize HybridCache for caching summaries/categories to optimize repeated requests.
		* Potentially use Azure Functions for serverless processing of large texts.
	* **React Frontend:** A simple UI for text input, displaying summaries, and filtering by category.
Directly addresses my AI interest, showcases API integration, text processing, and caching.

## Requirements 

The application should provide a performant, scalable, and intuitive way for users to summarize and categorize large blocks of text using AI.

**Must Have**
- Users must be able to input large blocks of text through a web interface.
- The backend must process the text using an AI model (OpenAI or Ollama via Microsoft.Extensions.AI).
- Return a concise summary and a single high-level category per input.
- Use HybridCache to avoid redundant AI calls for repeated texts.
- Provide Minimal API endpoints in .NET 9 for processing text.
- Display summaries and categories clearly in the frontend.
- Allow filtering of summarized entries by category.

**Should Have**
- Azure Functions for processing large inputs asynchronously.
- Allow selection between OpenAI and Ollama as processing engines.
- Persist processed results in a lightweight database (e.g., SQLite or Cosmos DB).

**Could Have**
- History/log of all processed texts per user session.
- Admin view to monitor and analyze common categories.
- Ability to download summaries.

**Won’t Have (in MVP)**
- User authentication and access control.
- Collaboration features (e.g., sharing summaries).

## High-Level Architecture

```mermaid
sequenceDiagram
    actor User
    participant FE as React Frontend
    participant API as Minimal API (.NET 9)
    participant AI as AI Service (OpenAI / Ollama)
    participant DB as HybridCache + DB
    participant AF as Azure Functions

    User ->> FE: Enter large text
    FE ->> API: POST /process-text
    API ->> DB: Check HybridCache
    DB -->> API: Cache hit? Return result
    API ->> AI: (If cache miss) Send text
    AI -->> API: Summary + Category
    API ->> DB: Store in cache + DB
    API -->> FE: Return summary + category

    alt If text is too large
        API ->> AF: Trigger async job
        AF ->> AI: Summarize & Categorize
        AF ->> DB: Store result
    end


