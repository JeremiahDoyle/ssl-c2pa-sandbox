# C2PA Developer Tool - Replit Migration

## Overview
This is an SSL.com C2PA (Coalition for Content Provenance and Authenticity) API testing and documentation tool. The application provides a web interface for generating keypairs, requesting C2PA certificates, signing images, and inspecting C2PA manifests.

## Recent Changes
**October 18, 2025** - Migrated from Vercel to Replit
- Updated `package.json` scripts to bind to `0.0.0.0:5000` for Replit compatibility
- Configured development workflow for Next.js dev server
- Configured deployment settings for autoscale (production builds)
- All dependencies installed successfully including c2patool binary

## Project Architecture

### Tech Stack
- **Framework**: Next.js 15.2.4 with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Key Libraries**: 
  - `c2pa` - C2PA manifest creation and inspection
  - `pkijs` & `asn1js` - Certificate and cryptographic operations
  - `react-hot-toast` - Notifications
  - `zod` - Schema validation

### Directory Structure
```
├── app/                 # Next.js App Router pages
│   ├── api/            # API routes (cert-requests, sign, tsa)
│   ├── docs/           # Documentation page
│   ├── layout.tsx      # Root layout
│   └── page.tsx        # Home page
├── components/          # React components
├── lib/                # Utility functions and services
├── public/             # Static assets
│   ├── assets/         # Images (logo, background)
│   └── c2pa/          # C2PA WASM files
└── scripts/           # Build and setup scripts
```

### Key Features
1. **Certificate Management**: Generate EC P-256 keypairs and CSRs in-browser
2. **Certificate Issuance**: Request C2PA certificates from SSL.com API
3. **Image Signing**: Sign images with C2PA manifests and assertions
4. **Manifest Inspector**: Inspect and validate C2PA manifests from signed images
5. **API Documentation**: Comprehensive API docs with code examples

### Environment Variables
All environment variables are **optional** (defined in `lib/env.ts`):
- `AUTH_TOKEN` - SSL.com API bearer token (shared test token available)
- `API_BASE` - API base URL (defaults to https://api.c2patool.io)
- `TSA_URL` - Timestamp authority URL
- `CERT_PROFILE_ID` - Certificate profile UUID
- `CONFORMING_PRODUCT_ID` - Product identifier UUID
- `C2PATOOL_PATH` - Path to c2patool binary for "Quick demo mode" (set to `/home/runner/workspace/bin/c2patool/c2patool`)

A shared test token is provided in `.env.example` for quick testing.

## Replit Configuration

### Development
- Port: 5000 (bound to 0.0.0.0)
- Command: `npm run dev`
- The workflow is configured to show the webview

### Deployment
- Type: Autoscale (stateless web application)
- Build: `npm run build`
- Start: `npm run start` (Next.js production server on port 5000)

### Post-install Scripts
The project automatically runs:
1. `copy-c2pa-assets.cjs` - Copies WASM files to public directory
2. `install-c2patool.cjs` - Downloads and installs c2patool binary

## Security Features
- Content Security Policy (CSP) headers configured
- Client/server separation with API routes
- Environment variable validation with Zod
- Security headers (X-Content-Type-Options, Referrer-Policy, Permissions-Policy)
- No sensitive data in client-side code

## Notes
- The app uses WebAssembly for C2PA operations (requires `wasm-unsafe-eval` in CSP)
- Private keys are generated and stored client-side only (never uploaded)
- The c2patool binary (v0.9.12) is installed to `/home/runner/workspace/bin/c2patool/c2patool`
- The `C2PATOOL_PATH` environment variable is configured in Replit Secrets to enable "Quick demo mode" for image signing
