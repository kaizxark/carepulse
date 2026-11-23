# CarePulse

CarePulse is a Next.js application for managing patient onboarding and appointment workflows. It provides a patient-facing registration flow and an admin-facing workspace for reviewing, scheduling, and cancelling appointments.

This repository is a portfolio and development project. The implementation should not be treated as a production healthcare service without a security, privacy, authorization, and deployment review.

## Product scope

CarePulse currently covers these workflow areas:

- Patient registration and profile creation
- Patient document/image upload through Appwrite Storage
- Doctor selection and appointment requests
- Appointment confirmation and cancellation
- Admin appointment listing and status counts
- Appointment-status messaging through Appwrite
- Form validation with Zod and React Hook Form
- Sentry client, server, and edge configuration
- Responsive UI built with the Next.js App Router

The repository does not include evidence of production users, a live deployment, clinical integrations, or a completed security review.

## Technology

- Next.js 14.2.3
- React 18
- TypeScript 5
- Tailwind CSS 3.4.1
- Appwrite for users, database documents, storage, and messaging
- Sentry for error-monitoring configuration
- React Hook Form and Zod for form state and validation
- Radix UI, React Dropzone, and React Table

## Repository layout

```text
app/                    App Router pages and the Sentry example route
components/             Forms, dashboard views, tables, and shared UI
constants/               Shared application constants
lib/actions/             Patient and appointment server actions
lib/appwrite.config.ts   Appwrite client configuration
lib/validation.ts        Zod validation schemas
public/                  Static assets
types/                   Shared TypeScript types
```

The principal server-side workflow is implemented in:

- `lib/actions/patient.actions.ts`
- `lib/actions/appointment.actions.ts`
- `lib/appwrite.config.ts`
- `lib/validation.ts`

## Data flow

1. A patient submits registration and profile information.
2. Server actions validate the request and persist records through Appwrite.
3. Supporting files are uploaded to the configured Appwrite bucket.
4. Patients submit appointment requests with doctor, date, and time-zone information.
5. Staff review appointment states from the admin workspace.
6. Staff can schedule or cancel an appointment, after which the application sends an Appwrite message.

The application uses environment-backed identifiers for Appwrite collections and storage. Do not place credentials or passkeys in source control.

## Local development

### Prerequisites

- Node.js 18 or newer
- npm
- An Appwrite project with the required database collections, user access, storage bucket, and messaging configuration

### Install and run

```bash
git clone https://github.com/kaizxark/carepulse.git
cd carepulse
npm ci
cp .env.example .env.local
npm run dev
```

Open `http://localhost:3000` after the development server starts.

This repository currently does not include a committed `.env.example`. Create `.env.local` manually using the variable names below until a checked-in template is added.

## Environment configuration

The application expects values corresponding to the Appwrite configuration and admin gate:

```env
NEXT_PUBLIC_ENDPOINT=https://cloud.appwrite.io/v1
PROJECT_ID=
API_KEY=
DATABASE_ID=
PATIENT_COLLECTION_ID=
DOCTOR_COLLECTION_ID=
APPOINTMENT_COLLECTION_ID=
NEXT_PUBLIC_BUCKET_ID=
NEXT_PUBLIC_ADMIN_PASSKEY=
```

`API_KEY` is a server secret. `NEXT_PUBLIC_ADMIN_PASSKEY` is used by the current client-side admin gate and must not be treated as a complete authorization system. Use a server-side authentication and authorization design before deploying this application for real users.

## Available commands

```bash
npm run dev      # Start the development server
npm run lint     # Run ESLint
npm run build    # Create a production build
npm run start    # Start the built application
```

## Verification status

The audited checkout contains no automated test suite. The current lint command completes with warnings. The current production build does not pass TypeScript validation because `AppointmentForm.tsx` calls `updateAppointment` without the required `timeZone` property. Resolve that type error and add automated tests before describing the project as deployment-ready.

## Security and privacy notes

CarePulse handles patient-related fields and uploaded files. Before any real deployment, review at minimum:

- Server-side authentication and role-based authorization
- Appwrite collection permissions and storage permissions
- Protection of appointment and patient data
- Secret management and key rotation
- Admin access controls and audit logging
- Input validation at every server boundary
- Rate limiting, abuse protection, and CSRF strategy
- Data retention, deletion, and privacy requirements
- Error and source-map configuration in Sentry

The current repository is not evidence of regulatory compliance or production security.

## Project status

CarePulse is maintained in the `kaizxark/carepulse` repository. The current codebase demonstrates the core onboarding and appointment-management workflow, but it still requires build repair, tests, a committed environment template, stronger authorization, and a deployment review.

## Source provenance

This repository contains an adapted healthcare application codebase based in part on the public healthcare project at [github.com/adrianhajdin/healthcare](https://github.com/adrianhajdin/healthcare). CarePulse-specific documentation, repository packaging, configuration changes, and subsequent maintenance are maintained in this repository. Upstream attribution and any applicable license obligations remain preserved.
