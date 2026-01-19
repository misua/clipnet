## Context
CLIPNET Phase 1 needs a dedicated staging environment so that new features can be validated end-to-end before production.
This change introduces a standard deployment and operations model for CLIPNET staging, including environment provisioning, CI/CD, configuration management, and observability.

## Goals / Non-Goals
- Goals:
  - Provide a predictable, reproducible CLIPNET staging environment aligned with the Phase 1 staging plan.
  - Enable push-button deployments of CLIPNET Phase 1 to staging via CI/CD.
  - Keep staging configuration and data isolated from production while preserving behavioral parity where required.
  - Provide enough observability to diagnose issues found in staging.
- Non-Goals:
  - Define detailed production deployment strategy (covered by separate change).
  - Re-architect CLIPNET application internals.

## Decisions
- Decision: CLIPNET Phase 1 staging will consist of exactly one Vercel frontend deployment (`staging.clipnet.ai` from the `develop` branch), one Railway API service, one Railway worker service, one Railway Redis instance, one Railway PostgreSQL database, and a Cloudinary `/staging_clips/*` folder.
- Decision: The single staging worker will execute all ClipNET pipeline tasks, including ClipBOT work, FFmpeg processing, ML scoring, Cloudinary uploads, auto-posting, metadata generation, and retry logic; ClipBOT will not have a separate worker in Phase 1.
- Decision: Staging and production will share a single RunPod GPU inference environment, with logical isolation achieved via configuration (for example, model paths or namespaces) rather than separate GPU instances.
- Decision: Staging configuration (OAuth keys, DB/Redis URLs, API base URLs, Cloudinary folders) will be fully isolated from production while following the same schema and configuration structure.
- Decision: CI/CD will deploy the frontend to Vercel from the `develop` branch and will deploy the API and worker services to Railway staging using a consistent versioning and promotion strategy.

## Risks / Trade-offs
- Risk: Staging may diverge from production over time if not maintained; mitigation is to document expected parity and periodically validate staging against production configuration.
- Risk: Limited observability in early stages may make some issues harder to diagnose; mitigation is to start with minimal useful telemetry and extend as needed.

## Migration Plan
- Introduce CLIPNET staging environment in parallel with any existing environments.
- Roll out CI/CD deployment to staging and validate with smoke tests.
- Onboard teams to use staging for Phase 1 validation and refine configuration and observability based on feedback.

## Open Questions
- What thresholds (for example, queue size, processing latency, or error rates) should trigger splitting additional workers in Phase 2?
- How will QA test scheduling be coordinated in Phase 1 to avoid collisions on the single shared staging worker?

