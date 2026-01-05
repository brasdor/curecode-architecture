# CureCode Technical Architecture

> **Swiss Medical AI Documentation Platform** - Complete technical architecture documentation

[![GitHub Pages](https://img.shields.io/badge/Demo-GitHub%20Pages-blue)](https://brasdor.github.io/curecode-architecture/)
[![License](https://img.shields.io/badge/License-Proprietary-red)](#)

## 🏥 Overview

CureCode is a production medical AI documentation platform designed for Swiss healthcare providers. The platform enables doctors to:

- **Record consultations** via audio
- **Transcribe automatically** using Whisper API
- **Generate medical documents** with AI assistance
- **Edit and approve** using a rich-text editor

## 🏗️ Architecture

The platform follows a **three-tier microservices architecture**:

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend (React SPA)                      │
│         curecode-app-client • React 18 • TanStack           │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│   curecode-app-service  │     │   curecode-ai-service   │
│      NestJS • Prisma    │◄───►│    NestJS • LLM API     │
│   Business Logic API    │     │   AI Processing API     │
└─────────────────────────┘     └─────────────────────────┘
              │                               │
              └───────────────┬───────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              PostgreSQL + Azure Blob Storage                 │
│          pgvector • Prisma Migrations • Encrypted           │
└─────────────────────────────────────────────────────────────┘
```

## 🛠️ Tech Stack

### Frontend
- **React 18** with TypeScript
- **TanStack Router** for routing
- **TanStack Query** for state management
- **TipTap Editor** (ProseMirror) for document editing
- **Tailwind CSS** for styling
- **Vite** for build tooling

### Backend
- **NestJS** (TypeScript)
- **Prisma ORM** with PostgreSQL
- **JWT Authentication**
- **OpenAPI/Swagger** documentation

### AI Services
- **Whisper API** for audio transcription (Infomaniak/OpenAI)
- **LLM API** for document generation (Configurable)
- **Azure Blob Storage** for file storage

### Infrastructure
- **Azure App Service** (Switzerland North)
- **Azure Database for PostgreSQL** Flexible Server
- **Azure Container Registry**
- **GitHub Actions** CI/CD

## 📊 Features

| Feature | Description |
|---------|-------------|
| **20 Document Types** | Medical reports, referrals, therapy notes, and more |
| **6 Medical Specialties** | Neurology, Orthopedics, Psychology, General Medicine, and more |
| **Swiss Compliance** | ICD-10-GM, TARMED, FADP, GDPR compliant |
| **Multi-tenant** | Organisation-based data isolation |
| **Audit Trail** | Complete document history tracking |

## 🔒 Security & Compliance

- ✅ **Swiss Data Residency** - All data stored in Azure Switzerland North
- ✅ **FADP & GDPR Compliant** - Swiss Federal Data Protection Act
- ✅ **JWT Authentication** - Secure token-based auth
- ✅ **HTTPS Enforced** - TLS encryption in transit
- ✅ **Encrypted Storage** - Data encrypted at rest

## 📖 Documentation

View the interactive architecture documentation:

**🔗 [https://brasdor.github.io/curecode-architecture/](https://brasdor.github.io/curecode-architecture/)**

The documentation includes:
- System Overview
- Three-Tier Architecture Details
- Complete Data Flow (Audio → Document)
- Database Schema (Prisma)
- API Endpoints Reference
- Azure Deployment Configuration

## 🚀 Deployment

The platform runs on Azure with two environments:

| Environment | Purpose |
|-------------|---------|
| **Production** | Live system with real customers |
| **Staging** | Testing and QA |

Deployment is automated via GitHub Actions using Git tags:
- `prod` tag → Production deployment
- `staging` tag → Staging deployment

## 📝 License

This is proprietary software. The architecture documentation is provided for demonstration purposes only.

---

**Built with ❤️ in Switzerland** 🇨🇭
