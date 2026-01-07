# CureCode Architecture - Speaking Notes

## 🎯 Overview Tab

### Opening (Key Features)
"CureCode is a medical AI documentation platform built specifically for the Swiss healthcare market. Our core value proposition centers on four pillars: First, all data stays in Switzerland, hosted in Azure's Zürich North to ensure FADP and GDPR compliance. Second, we've built specialized medical expertise with support for different medical specialties which also directly positively impacts the quality of the output. Third, the goal is to improve documentation efficiency - doctors can dictate their notes and our AI generates structured medical documents automatically. And fourth, enterprise-grade security with end-to-end encryption, JWT authentication, and role-based access controls."

### Tech Stack
"We've built CureCode as a modern TypeScript full-stack application. The frontend is a React 18 SPA using Vite for fast builds, TanStack Router for type-safe routing, and TipTap as our rich text editor - which is critical for medical document editing. On the backend, we run two NestJS services: one for core business logic and one dedicated to AI processing. Our data layer uses PostgreSQL with the pgvector extension for semantic search capabilities, and Azure Blob Storage for audio files and documents. Everything runs in Docker containers on Azure App Service."

---

## 🏗️ Architecture Tab

### Three-Tier Overview
"Our architecture follows the classic three-tier pattern, which you can see visualized in this diagram. The presentation layer is our React SPA - this is what doctors interact with daily. The business layer consists of two domain services: the App Service handles all business logic, authentication, and CRUD operations, while the AI Service is dedicated to speech processing and document generation. The data layer combines PostgreSQL for structured data with Azure Blob Storage for binary assets like audio recordings. This separation of concerns allows us to scale each tier independently based on demand."

### Frontend Details
"The frontend is built with React 18 and TypeScript, giving us strong type safety across the entire UI. We use TanStack Router which provides file-based routing with full TypeScript inference - no more runtime routing errors. TipTap is our rich text editor, built on ProseMirror, which handles the complex medical document editing with custom extensions for medical terminology. For styling, we use Tailwind CSS with shadcn/ui components, giving us a consistent, accessible design system. The frontend communicates with our backend exclusively through a generated TypeScript API client, ensuring type safety end-to-end."

### Backend Services
"Our backend is split into two NestJS services for clear separation of concerns. The App Service handles everything business-related: user authentication, organisation management, patient records, and document CRUD operations. It owns the Prisma ORM layer and all database migrations. The AI Service is purpose-built for compute-intensive AI tasks: audio transcription using speech-to-text APIs, text extraction from PDFs and Word documents, and LLM-powered document generation. This separation means we can scale AI processing independently when demand spikes, without affecting core application performance."

---

## 🔄 Data Flow Tab

### Audio Recording Flow
"Let me walk you through how we process a doctor's voice recording. The doctor records audio directly in the browser using the Web Audio API - we capture high-quality audio locally. That recording is uploaded to Azure Blob Storage with a secure SAS token - the audio never sits unencrypted. The AI Service picks up the job, sends the audio to our speech-to-text provider, and receives the transcription. We then run that transcription through our LLM with specialty-specific prompts to generate a structured medical document. Finally, the generated document appears in the editor where the doctor can review and finalize it."

### Document Generation Flow
"For document generation from existing files, the flow is similar but starts differently. The doctor uploads a PDF or Word document containing consultation notes or referral letters. Our AI Service extracts the text content using specialized parsing libraries. That extracted text becomes context for our LLM, which generates the appropriate medical document type - whether that's a consultation report, discharge summary, or specialist referral. The key insight here is that we're not just transcribing - we're transforming unstructured input into properly formatted, specialty-appropriate medical documentation."

---

## 🗄️ Database Tab

### Capabilities Overview
"Our database architecture is designed around the specific needs of medical documentation. We maintain a multi-tenant structure with complete data isolation between healthcare organisations - each practice's data is completely separate. We support multiple document schemas covering 20 different medical document types, from consultation reports to specialist referrals. The pgvector extension enables semantic search, so doctors can find relevant past documents based on meaning, not just keywords. And we've implemented soft delete with configurable retention - nothing is permanently lost by accident, but we can fully erase data for GDPR compliance when required."

### Security Features
"Data security is paramount in healthcare. All data is encrypted at rest using AES-256 and in transit using TLS 1.3 - there's no point in the pipeline where data is unencrypted. We store all data exclusively in Azure's Switzerland North region, which is critical for Swiss healthcare compliance. Audit logging captures every significant action for regulatory compliance. And our deletion strategy combines soft delete for safety with hard delete capability for GDPR's right to erasure."

---

## 🚀 Deployment Tab

### Azure Infrastructure
"We deploy everything to Azure's Switzerland North region - this is non-negotiable for Swiss healthcare compliance. We run two complete environments: production and staging, each with its own set of App Services and database schemas. All three services - frontend, app service, and AI service - run as Docker containers on Azure App Service for Linux. The database is Azure Database for PostgreSQL Flexible Server, which gives us automated backups and point-in-time recovery. Blob storage handles all our file assets with private containers and SAS token access."

### CI/CD Pipeline
"Our deployment process is fully automated through GitHub Actions. We use a tag-based deployment strategy. The pipeline runs tests, builds Docker images with the appropriate tags, pushes them to Azure Container Registry, and deploys to the corresponding App Services. This means we can deploy to staging for QA testing, and when ready, a single tag push promotes to production. Health checks verify each deployment succeeded before it's considered complete."

---

## 💡 Tips for Presenting

1. **Start with the problem**: "Doctors spend 2+ hours daily on documentation. CureCode reduces that to minutes."

2. **Emphasize Swiss compliance**: This is your key differentiator - data never leaves Switzerland.

3. **Demo the architecture diagram**: Walk through a real user flow visually.

4. **Keep technical depth appropriate**: Adjust based on audience - executives need the "what", engineers need the "how".

5. **End with the value**: Circle back to time saved and compliance guaranteed.
