# evidence

## Cloud Lab 03 - Incident Resolution & Deployment Report

### 1. What Was Broken
- **Symptom**: The Media Hub web application was deployed on port `18082`, but the showcase image failed to load (displaying a broken asset placeholder).
- **Root Cause**: The container was initially configured with an incorrect storage container reference (`cloud03-media-missing`) and encountered private media read permission errors (`HTTP 403`), alongside lingering Docker container instances occupying port `18082`.

### 2. What Was Changed / Fixed
- Inspected active Docker containers and application logs using the Azure VM `RunShellScript` extension to diagnose the storage connection failure and port conflicts.
- Stopped and removed all conflicting legacy Docker containers binding to port `18082`.
- Redeployed the application container (`fmtalicloud03e6db6b92.azurecr.io/cloud03/lrn-0022:sha-c031da7a6f994f854817436ae67c4df`) with the corrected environment variables pointing to the active Azure Storage backend:
  - `AZURE_STORAGE_ACCOUNT=fmtalilrn0022b1c560a4`
  - `AZURE_MEDIA_CONTAINER=cloud03-media`
  - `MEDIA_SOURCE=azure`

### 3. Verification
- Verified clean container startup via Docker logs (`Media Hub ready at http://localhost:8080`) with zero `stderr` errors.
- Navigated to `http://102.133.227.161:18082` in the browser and confirmed that both the showcase image and the video preview load and play properly.
- Recorded video proof of the working application and saved it to `evidence/cloud03-evidence.mp4`.
