# CI/CD Security Research Prototype

This repository accompanies the proposal "Integrating Automated Security Checks into CI/CD Pipelines for Enhanced Vulnerability Detection and Remediation" and includes:

- `proposal.md`: A polished Markdown version of the research proposal.
- `.gitlab-ci.yml`: A GitLab CI pipeline skeleton with SAST, SCA, IaC, and container image scanning.
- `docs/gantt.md`: A Mermaid Gantt chart for the project timeline.

## Prerequisites

- GitLab project with CI/CD enabled.
- Variables set in project settings when using the pipeline:
  - `SONAR_HOST_URL` (e.g., https://sonarqube.example.com)
  - `SONAR_TOKEN` (project token)
  - Optionally tune `ALLOW_FAILURE` in `.gitlab-ci.yml` to fail or pass on findings.

## Using the Pipeline

- Commit `.gitlab-ci.yml` to your GitLab repository.
- Ensure your project builds/pushes images if you want the Trivy image scan to run (set up a `docker build && docker push` job that tags `$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA`).
- For SAST with SonarQube, make sure the server is reachable from GitLab runners and the token is valid.

## Local Testing (Optional)

You can run the scanners locally using Docker. Examples:

```bash
# SCA: OWASP Dependency-Check (reports to ./dependency-check-report)
docker run --rm \
  -v "$PWD":/src \
  -v "$PWD/.depcheck-data":/usr/share/dependency-check/data \
  -v "$PWD/dependency-check-report":/report \
  owasp/dependency-check:latest \
  --scan /src --format ALL --out /report --noupdate

# IaC: Checkov (JUnit output)
docker run --rm -v "$PWD":/workdir bridgecrew/checkov:latest \
  -d /workdir -o junitxml > checkov-report.xml || true

# Container image: Trivy (scans local image tag)
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy:latest image --no-progress my-image:tag
```

## Notes

- Tune severity thresholds and failure behavior per stage to balance detection vs. developer experience.
- Use caching and parallel stages in GitLab to minimize performance impact.
- Expand with DAST or license compliance as needed for your stack.
