# 🧬 A DMIT System – Fingerprint Capture, ML Ridge Analysis & Luminous Integration

A complete end‑to‑end system for capturing fingerprint images, processing ridge features, and returning analysis results to the Main Luminous System.

This repository contains the frontend scanning application built with React + Vite, along with supporting documentation for development setup, environment configuration (safe placeholders only), and project structure.

## Table of Contents

[Overview](#overview)<br />
[Tech Stack](#tech-stack)<br />
[Features](#features)<br />
[Project Structure](#project-structure)<br />
[Setup & Installation](#setup-&-installation)<br />
[System Workflow](#system-workflow)<br />

---

## Overview

The system uses:

- **React + Vite** for the scanning frontend
- **Node.js / Express** for backend services
- **PostgreSQL** for database storage
- **AWS S3** for encrypted image storage
- **Background ML Worker** for asynchronous fingerprint processing

## Tech Stack


| **Layer**   | **Technology**                            | **Notes**                                       |
| ----------- | ----------------------------------------- | ----------------------------------------------- |
| Frontend    | **React + Vite**                          | Fast, lightweight, perfect for camera workflows |
| Storage     | AWS S3                                    | Encrypted at rest, private access               |
| Backend API | Node.js / Fastify / Express (Your choice) | Handles uploads, validation & analysis trigger  |
| Database    | PostgreSQL                                | Stores session + metadata                       |
| ML Pipeline | Python Worker (optional)                  | Ridge counting & pattern extraction             |

## Features
- Secure fingerprint capture (30 images per session)
- Real‑time quality validation (lighting, ridge clarity, alignment)
- Auto-upload to AWS S3
- Session handling (pending → processing → completed)
- Integration with Luminous Main System
- Admin dashboard support (monitoring only)
- Automatic ML processing trigger
- Clean folder structure and maintainable architecture

## Project Structure
> (In Development)

## Setup & Installation
> (In Development)

---

## **System Workflow**

### **1. User Entry Point**

Users begin in the **Main Luminous System**, which generates:

- A short‑lifespan secure token
- A `redirect_url`
- A dynamic URL → `https://tarumt-app.com/scan?token=...`

### **2. Token Validation**

The TARUMT scanning app:

- Validates the token via `POST /api/auth/validate`
- Creates a new `ScanningSession`
- Rejects invalid tokens and shows a retry page

### **3. Fingerprint Capture (30 Images)**

For each of 10 fingers × 3 angles (Left, Center, Right):

- Camera opens
- Quality checks (ridge visibility, alignment, lighting)
- Auto‑capture
- Upload to **S3**
- Metadata saved to PostgreSQL

### **4. Async ML Processing**

Once all 30 images are present:

- Background worker processes ridge counts & patterns
- Aggregates results per finger
- Generates overall left/right brain summaries

### **5. Callback to Luminous**

A final structured response is pushed back via:
`POST /api/fingerprint/results`

### **6. Admin Dashboard**

Internal staff can:

- View sessions
- View images
- Check processing status
- Approve/reject low‑confidence results

---
