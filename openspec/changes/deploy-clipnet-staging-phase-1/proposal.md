# Change: Deploy CLIPNET Phase 1 to staging

## Why
CLIPNET Phase 1 requires a reproducible staging environment that aligns with the "CLIPNET Phase 1 Staging Plan" so that features can be validated safely before production rollout.

## What Changes
- Stand up the CLIPNET Phase 1 staging environment on the target platforms: Vercel (staging frontend at `staging.clipnet.ai` from the `develop` branch), Railway (staging API service, staging worker service, staging Redis, and staging PostgreSQL), Cloudinary (`/staging_clips/*` folder), and a shared RunPod GPU inference server.
- Configure staging services to be fully isolated from production through separate DB, Redis, OAuth credentials, API base URLs, and Cloudinary folders, while reusing the same RunPod GPU instance.
- Implement CI/CD so that changes on the `develop` branch deploy the frontend to Vercel staging and coordinated builds deploy the API and worker services to Railway staging.
- Configure the single staging worker and Redis queues so that all ClipNET pipeline jobs (including ClipBOT jobs) run through one worker instance in Phase 1.
- Set up minimal but sufficient logging/monitoring for the staging API, worker, and queues, and define deployment and rollback procedures for CLIPNET Phase 1 staging.

## Impact
- Affected specs: clipnet-staging-deployment
- Affected code: infrastructure definitions, deployment configuration and CI/CD workflows, application configuration for staging, and operational runbooks/docs.
