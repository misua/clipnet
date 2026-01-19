## ADDED Requirements

### Requirement: CLIPNET staging environment
The system SHALL provide a CLIPNET Phase 1 staging environment composed of exactly one staging API service, one staging worker service, one staging Redis instance, one staging PostgreSQL database, one staging Cloudinary folder, and one staging frontend deployment.

#### Scenario: Staging environment available
- **WHEN** CLIPNET Phase 1 staging is provisioned
- **THEN** the environment SHALL include a Vercel frontend at `staging.clipnet.ai`, a Railway API service, a Railway worker service, a Railway Redis instance, a Railway PostgreSQL database, and a Cloudinary `/staging_clips/*` folder configured for staging.

### Requirement: CI/CD deployment to CLIPNET staging
The system SHALL support automated deployment of CLIPNET Phase 1 to the staging environment, including the Vercel staging frontend and Railway staging API and worker services.

#### Scenario: Automated deployment to staging
- **WHEN** changes are merged into the `develop` branch or a build is marked for staging deployment according to the branching strategy
- **THEN** the Vercel staging frontend SHALL be updated for `staging.clipnet.ai` and the Railway staging API and worker SHALL be deployed with matching versions using the CI/CD pipeline.

### Requirement: Staging configuration and secrets isolation
The system SHALL manage configuration and secrets for CLIPNET staging separately from production while allowing shared defaults where appropriate.

#### Scenario: Staging configuration applied
- **WHEN** CLIPNET is deployed to the staging environment
- **THEN** the deployment SHALL use staging-specific configuration and secrets, not production configuration or secrets.

### Requirement: Staging observability
The system SHALL provide basic observability for CLIPNET staging, including logging and metrics sufficient to diagnose issues found during Phase 1 testing.

#### Scenario: Observability for staging issues
- **WHEN** an error or performance issue occurs in the CLIPNET staging environment
- **THEN** logs and metrics from staging SHALL be available to help diagnose the issue.

### Requirement: Deployment and rollback procedure for staging
The system SHALL define and support a clear deployment and rollback procedure for CLIPNET staging deployments.

#### Scenario: Rollback from failed staging deployment
- **WHEN** a CLIPNET staging deployment is determined to be faulty or blocking testing
- **THEN** operators SHALL be able to roll back the CLIPNET staging environment to the last known good version using a documented procedure.

### Requirement: Single worker per staging environment
The system SHALL use a single worker service in CLIPNET Phase 1 staging to execute all pipeline jobs, including ClipBOT-related jobs.

#### Scenario: Jobs executed by single worker
- **WHEN** jobs such as `clip:process`, `ffmpeg:polish`, `ml:score`, `whisper:batch`, `clipbot:post`, and `clipbot:notify` are enqueued in staging
- **THEN** they SHALL be executed by the single staging worker service and not by separate per-feature or ClipBOT-specific workers.

### Requirement: Single Redis queue per staging environment
The system SHALL use a single Redis instance as the job queue for CLIPNET Phase 1 staging.

#### Scenario: Jobs queued in staging Redis
- **WHEN** the staging API enqueues ClipNET jobs
- **THEN** those jobs SHALL be written to the single staging Redis instance and held there until the staging worker processes them.

### Requirement: Staging and production isolation
The system SHALL ensure that staging and production are isolated through separate databases, Redis instances, API deployments, OAuth credentials, and Cloudinary folders.

#### Scenario: Staging does not affect production
- **WHEN** ClipNET jobs are executed in the staging environment
- **THEN** no staging operation SHALL read from or write to production Redis, production PostgreSQL, production OAuth credentials, or the `/prod_clips/*` Cloudinary folder.

### Requirement: Shared RunPod GPU inference
The system SHALL use a single shared RunPod GPU inference environment for both staging and production in Phase 1 while ensuring logical isolation of workloads.

#### Scenario: Staging uses shared GPU safely
- **WHEN** staging jobs invoke Whisper, BLIP, motion detection, or CLIP fallback on the RunPod GPU
- **THEN** they SHALL use the same physical GPU environment as production but remain logically isolated via configuration (for example, model paths, queues, or namespaces) without requiring a dedicated staging GPU.
