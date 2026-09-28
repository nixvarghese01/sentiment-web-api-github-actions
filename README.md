# coit-backend1-GA: Web API with GitHub Actions and Kustomize

The Spring Boot sentiment-analysis web API ([coit-backend1](https://github.com/nixvarghese01/coit-backend1)), packaged for a GitOps-style delivery pipeline:

- **GitHub Actions**: CI on the `stage` branch and CD to production on `v*` tags
- **Kustomize overlays**: separate `stage` and `prod` configuration for GKE
- **cert-manager issuers** (Let's Encrypt staging and production) and **RBAC roles**

## Repository layout
```
coit-backend1/               Spring Boot app (Java 8, Maven)
backend1-kustomize-stage/    Issuers and RBAC roles for the stage namespace
backend1-kustomize-prod/     Issuers and RBAC roles for the prod namespace
.github/workflows/
  CI-backend1-stage.yml      SonarQube scan, tests, Docker build and push, deploy to stage (on push/PR to stage)
  deploy-to-prod.yml         Build, push and deploy to prod (on v* tags)
```

## Build locally
```bash
cd coit-backend1
./mvnw clean install
docker build -f Dockerfile-multistage -t <dockerhub-user>/coit-backend1 .
```

## Required GitHub secrets
`DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`, `GKE_PROJECT`, `GKE_SA_KEY`, `SONARQUBE_PROJECT`, `SONARQUBE_URL`, `API_KEY`

> **Note:** `CI-backend1-stage.yml` was copied from the frontend pipeline and still runs `cd coit-frontend` and `npm test`. Point those steps at `coit-backend1` (Maven) before relying on it.

## Branches
`feature` (default), `stage`
