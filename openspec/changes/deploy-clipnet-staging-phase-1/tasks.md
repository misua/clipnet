## 1. Environment & Infrastructure
- [ ] 1.1 Create Vercel staging deployment for ClipNET at `staging.clipnet.ai` targeting the `develop` branch.
- [ ] 1.2 Provision Railway staging services: one API server, one worker service, one Redis instance, and one PostgreSQL database for ClipNET.
- [ ] 1.3 Configure Cloudinary to use the `/staging_clips/*` folder for all staging uploads and ensure production uses `/prod_clips/*`.
- [ ] 1.4 Configure the shared RunPod GPU inference endpoint for both staging and production, with staging using isolated model paths or namespaces where applicable.

## 2. CI/CD Pipeline to Staging
- [ ] 2.1 Configure Vercel so that pushes to the `develop` branch automatically deploy the frontend to `staging.clipnet.ai`.
- [ ] 2.2 Configure CI/CD (for example GitHub Actions) to build and deploy the Railway staging API service for builds marked for staging.
- [ ] 2.3 Configure CI/CD to build and deploy the Railway staging worker service using the same application version as the staging API.
- [ ] 2.4 Document and automate rollback procedures for Railway staging API and worker deployments (for example, redeploying a previous image or build).

## 3. Observability & Operations
- [ ] 3.1 Enable structured logging for the staging API and worker in Railway and ensure logs are filterable by environment.
- [ ] 3.2 Configure basic metrics and health checks for the staging API, worker, Redis, and PostgreSQL services.
- [ ] 3.3 Set up alerts for staging worker failures, Redis queue backlog (age/size), and elevated 5xx error rates on the staging API.
- [ ] 3.4 Document operational runbooks for common CLIPNET staging operations (deploy, rollback, clearing stuck jobs, re-running jobs).

## 4. Validation & Readiness
- [ ] 4.1 Define and implement smoke tests that validate the full ClipNET pipeline in staging (API → Redis → worker → DB → Cloudinary → dashboard).
- [ ] 4.2 Run ClipBOT flows end-to-end against staging (command routes → Redis → worker → notifications/posts).
- [ ] 4.3 Validate that staging infrastructure matches the Phase 1 plan: one API, one worker, one Redis, one PostgreSQL, one Cloudinary folder, and one Vercel staging frontend.
- [ ] 4.4 Obtain sign-off that CLIPNET staging is safe for QA and mirrors the production topology (scaled down) for Phase 1.
