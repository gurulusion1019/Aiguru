# Salon Owner Lead Capture Wizard

## Overview

A mobile-first 4-step wizard form application designed to capture qualified salon owner leads from Instagram/Facebook advertising campaigns. The application provides a streamlined registration process with OTP verification, image uploads with client-side compression, and webhook integration for lead data submission.

The wizard guides users through: About You (contact verification) → Salon Details (business information) → Media (photo uploads) → Review & Submit.

**Status**: Fully functional MVP with all core features implemented and tested.

## User Preferences

Preferred communication style: Simple, everyday language.

## Recent Changes (Oct 16, 2025)

### Backend Implementation
- Created complete REST API with Express.js
- Mock OTP service (sends/verifies 6-digit codes, logs to console)
- Image upload endpoint with multer (saves to `/uploads` directory)
- Lead submission endpoint with webhook forwarding
- All validation schemas using Zod

### API Endpoints
- `POST /api/otp/send` - Send OTP to mobile (mock implementation)
- `POST /api/otp/verify` - Verify OTP code
- `POST /api/upload` - Upload and compress images
- `POST /api/leads` - Submit complete lead form

### Features Implemented
- Form auto-save to localStorage per step
- Duplicate submission prevention (10-minute cooldown)
- Client-side image compression (max 1MB, 1600px)
- UTM parameter tracking from URL query strings
- Webhook submission to external endpoint (configurable via WEBHOOK_URL env var)
- Mobile-optimized UI with sticky header/footer navigation
- GPS location detection with reverse geocoding (auto-fills city/pincode)

### Testing
- End-to-end test completed successfully
- Verified all 4 wizard steps function correctly
- ⚠️ OTP verification temporarily disabled (removed for deployment without SMS service)
- Form persistence across steps validated
- Navigation and validation confirmed working
- Location detection tested with GPS and reverse geocoding (auto-fills Mumbai/400070)
- Simplified flow tested: name + mobile only (no OTP required)

## System Architecture

### Frontend Architecture

**Framework & Build System**
- React with TypeScript for type-safe component development
- Vite as the build tool and dev server
- Wouter for lightweight client-side routing
- React Query (@tanstack/react-query) for server state management

**UI Component Strategy**
- shadcn/ui components (Radix UI primitives with custom styling)
- Tailwind CSS for utility-first styling with custom design tokens
- Mobile-first responsive design approach
- Custom theme system supporting light/dark modes via CSS variables

**Form State Management**
- Local state management with React hooks
- localStorage for form data persistence across steps
- Client-side validation before submission
- Auto-save functionality to prevent data loss

**Image Handling**
- browser-image-compression library for client-side image optimization
- Maximum 1MB file size, 1600px max dimension
- Support for JPEG, PNG, and WebP formats
- Multi-file upload capability for gallery images

### Backend Architecture

**Server Framework**
- Express.js with TypeScript
- RESTful API design pattern
- Multer middleware for handling multipart/form-data uploads
- In-memory storage (MemStorage class) - designed to be replaceable with database layer

**API Endpoints**
- `/api/otp/send` - Mock OTP generation and logging
- `/api/otp/verify` - OTP validation with expiry checking
- `/api/upload` - Image upload handler with file type validation
- `/api/leads` - Lead submission endpoint with webhook forwarding

**Data Validation**
- Zod schemas for runtime type validation
- Drizzle-Zod integration for database schema validation
- Server-side validation of phone numbers, OTP codes, and file types

### External Dependencies

**Database (Configured but Optional)**
- PostgreSQL via @neondatabase/serverless
- Drizzle ORM for type-safe database operations
- Schema defined in `shared/schema.ts` with tables for users and leads
- Migration support via drizzle-kit

**Third-Party Integrations**
- Webhook endpoint for lead data forwarding (configurable via WEBHOOK_URL env variable)
- Google Fonts API (Inter and Plus Jakarta Sans fonts)
- SMS/OTP service integration point (currently mocked, ready for production service)

**File Storage**
- Local filesystem storage in `/uploads` directory
- Static file serving via Express
- Designed to be replaceable with cloud storage (S3, Cloudinary, etc.)

**Session Management**
- Cookie-based duplicate submission prevention
- OTP storage with time-based expiration (in-memory Map)
- 10-minute cooldown between submissions per mobile number

**Analytics & Tracking**
- UTM parameter capture from query strings
- User agent logging for device/browser analytics
- Campaign source tracking in lead metadata

**Design System**
- Custom CSS variables for theming (defined in `client/src/index.css`)
- Design guidelines documented in `design_guidelines.md`
- Color palette optimized for beauty/aesthetic industry
- Typography using Google Fonts with fallback system fonts