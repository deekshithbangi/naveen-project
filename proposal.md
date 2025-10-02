# Integrating Automated Security Checks into CI/CD Pipelines for Enhanced Vulnerability Detection and Remediation

Forename Surname  
Student ID  
Programme  
National College of Ireland

---

## Abstract

The popularity of Continuous Integration and Continuous Delivery (CI/CD) has accelerated software delivery while increasing security risk: any vulnerability or misconfiguration can rapidly propagate through automated pipelines. This study examines how to integrate automated security checks into the software development lifecycle (SDLC) via CI/CD—focusing on Static Application Security Testing (SAST), Software Composition Analysis (SCA), and Infrastructure-as-Code (IaC) scanning—to improve vulnerability detection and remediation. Building on DevSecOps principles, the research analyzes tool capabilities and limitations, the trade-offs between security depth and pipeline performance, and practical best practices for tool selection, configuration, and integration. The methodology combines literature review, case-study analysis, and experimental testing on synthetic environments to derive actionable guidance that balances development velocity with robust security.

Keywords: CI/CD Security, DevSecOps, Secure Software Development, Vulnerability Scanning, Continuous Integration

---

## 1. Introduction

CI/CD practices have reshaped software delivery, enabling fast, iterative development and frequent deployments. However, this increased velocity raises the stakes for application security. With code changes continuously moving toward production, treating security as an afterthought or a late-stage gate is no longer viable. Without early, automated controls, vulnerabilities and misconfigurations can slip into releases, causing breaches, data loss, and regulatory exposure.

The research problem addressed in this paper is the lack of effective, automated security checks integrated into CI/CD pipelines. Although DevSecOps promotes shifting security left, organizations encounter practical hurdles: tool compatibility, false positives, pipeline slowdowns, and limited coverage across heterogeneous environments. Absent a systematic approach, security remains fragmented and inconsistently applied.

This problem is critical for secure software development in an era of pervasive agile practices and cloud-native architectures. Recent studies (Ugwueze & Chukwunweike, 2024; Yang, 2025; Baitha et al., 2024) recognize security automation as transformative, yet they highlight challenges in adoption, configuration trade-offs, and workflow alignment. Striking a balance between robust security and developer productivity is essential—and is the focus of this study.

Core research question:

- How can automated security checks be effectively integrated into CI/CD pipelines to improve the detection and remediation of vulnerabilities and misconfigurations throughout the SDLC?

Proposed approach: conduct a systematic study of SAST, SCA, and IaC tools; design and implement a prototype CI/CD integration; and evaluate tool effectiveness, usability, and performance trade-offs. The study aims to produce replicable best practices and a practical integration model that teams can adapt to their context.

---

## 2. Literature Review

### 2.1 CI/CD security: current practices and gaps

Modern CI/CD pipelines are prone to security lapses due to speed and automation. While agility increases throughput, manual security controls often lag behind, creating risk hot spots in third-party dependencies and cloud infrastructure (Abiona et al., 2024; Jani, 2023). Calls for automation are common, but differ on where and how to implement it within the pipeline. Training and governance help, yet they do not scale as effectively as automated, embedded controls (Kathiresan, 2022).

This suggests that security should be codified and automated directly within CI/CD, rather than relying solely on policies, reviews, or awareness campaigns.

### 2.2 Tools for security automation in CI/CD pipelines

Automation commonly spans three categories:

- SAST (e.g., SonarQube): analyzes source to find bugs and vulnerabilities. Effective at early detection but can yield false positives that frustrate developers (Lenarduzzi et al., 2023).
- SCA (e.g., OWASP Dependency-Check): identifies known vulnerabilities in open-source libraries; strong for CVE coverage but cannot find zero-day or logic flaws (Jani, 2023).
- IaC scanners (e.g., Checkov, TFLint): analyze Terraform/Kubernetes/CloudFormation for misconfigurations; helpful but can increase build latency and require careful integration.

Comparative studies note integration issues—such as build latency and IDE integration—that affect adoption and usability (Wadhams, Izurieta & Reinhold, 2024). Therefore, evaluations must consider not only detection accuracy but also developer experience and operational cost.

### 2.3 Effectiveness vs. developer experience: the trade-off dilemma

Automated checks can slow pipelines and produce alert fatigue if poorly tuned. For example, integrating IaC scanners has been associated with material build delays in containerized settings (War et al., 2023). Excess false positives reduce trust and lead to ignored results.

Conversely, teams that introduced severity thresholds, contextual filtering, and tailored rulesets reported improved utility (e.g., Snyk case discussion: Clark, Smalling & Moses, 2023). Configuration and workflow fit are as important as tool selection.

### 2.4 Case studies and patterns in DevSecOps implementation

Reported successes show that thoughtful pipeline design—parallelization, caching, and incremental rollout—can integrate SAST/DAST/container scans with minimal slowdown (e.g., GitLab’s internal case study, 2023). Challenges arise when toolchains are incompatible or adoption lacks developer buy-in (e.g., Terraform pipelines without alignment on scanner baselines). Organizational factors—training, feedback loops, and tooling maturity—matter as much as technical configuration (Rajapakse et al., 2022).

Common patterns include:

- Security is framed as a shared responsibility across teams.
- Tools are adopted incrementally with clear feedback and remediation paths.
- Pipelines leverage parallelism and caching to mitigate performance costs.

### 2.5 Identifying the research niche and contribution

The literature shows progress in CI/CD security automation but lacks an end-to-end, evidence-based integration model that addresses security coverage, pipeline performance, and developer experience together. This work contributes:

- A practical, replicable model for integrating SAST, SCA, IaC, and container scanning into CI/CD.
- An assessment framework measuring detection accuracy, false positives, integration complexity, and performance impact.
- Best practices for tool selection, configuration, and rollout strategies.

---

## 3. Research Method and Specification

### 3.1 Proposed solution and research approach

We will design, integrate, and evaluate a prototype DevSecOps pipeline that embeds SAST, SCA, IaC, and container scanning into CI/CD. The approach is applied, experimental, and engineering-focused, combining:

- Descriptive research: review current practices and tools.
- Design science: build a modular prototype pipeline.
- Empirical experimentation: quantify effectiveness and trade-offs.

### 3.2 Research plan and expected activities

We will follow a milestone-based schedule aligned with capstone timelines:

- Literature review and synthesis
- Tool selection and baseline configuration
- Prototype pipeline design (parallel stages, caching)
- Incremental integration: SAST → SCA → IaC → container scan
- Experimental evaluation (metrics capture, thresholds tuning)
- Results analysis and best-practice formulation
- Documentation, ethics, and dissemination

A Gantt chart illustrating phases, dependencies, and durations is provided in `docs/gantt.md`.

### 3.3 Tools, platforms, and test data

Tools:

- CI/CD: GitLab CI/CD (optional Jenkins comparison)
- SAST: SonarQube (community edition)
- SCA: OWASP Dependency-Check
- IaC: Checkov (Terraform/Kubernetes), TFLint (optional)
- Container scanning: Trivy
- Repo hosting: GitHub/GitLab (private)
- Build env: Docker + Linux VMs
- Metrics: Prometheus + Grafana (optional)

Test data/applications:

- Sample microservices (e.g., Spring PetClinic; Flask/Django apps)
- Deliberately vulnerable IaC templates (e.g., Vulnado, Damn Vulnerable Terraform)
- Public CVE databases for SCA benchmarking

### 3.4 Evaluation strategy

Primary metrics:

- Detection rate: count/severity of vulnerabilities found by each tool.
- False positives: manually validated proportion of incorrect findings.
- Pipeline performance: build time delta before vs. after integration.
- Integration complexity: effort/time to implement each tool.
- Developer experience impact: simulated delays, usability notes, and failure threshold efficacy.

Comparisons will be made across stages (no tools → single tool → full stack). Effectiveness is defined as materially improved detection without unacceptable impact on build time and developer workflow.

### 3.5 Ethical considerations

No real user data, private repositories, or proprietary applications will be used. Safeguards:

- All scans run in isolated, local environments.
- Tools are open-source and suitable for academic use.
- No confidential data is processed.
- Publicly shared reports will be anonymized.
- The work follows NCI’s ethical framework; required declarations will be submitted.

Any real-world deployment will require organizational consent, access control, and standard DevSecOps governance.

---

## 4. References

- Clark, B., Smalling, E. and Moses, J. (2023). Critical WebP 0-day security CVE-2023-4863 impacts wider software ecosystem. Snyk. https://snyk.io/blog/critical-webp-0-day-cve-2023-4863/ (accessed 1 Aug 2025).
- Gopinath Kathiresan (2022). Evaluating the impact of DevSecOps on software quality: A systematic review and empirical study. World Journal of Advanced Research and Reviews, 14(1), 644–653. https://doi.org/10.30574/wjarr.2022.14.1.0267
- Jani, Y. (2023). Implementing Continuous Integration and Continuous Deployment (CI/CD) in Modern Software Development. International Journal of Science and Research (IJSR), 12(6), 2984–2987. https://doi.org/10.21275/sr24716120535
- Lenarduzzi, V., Pecorelli, F., Saarimaki, N., Lujan, S. and Palomba, F. (2023). A critical comparison on six static analysis tools: Detection, agreement, and precision. Journal of Systems and Software, 198, 111575. https://doi.org/10.1016/j.jss.2022.111575
- None Oluwatosin Oluwatimileyin Abiona, Oladapo, J., Oluwole, N., None Oyekunle Claudius Oyeniran, Okechukwu, A. and Abiola, N. (2024). The emergence and importance of DevSecOps: Integrating and reviewing security practices within the DevOps pipeline. World Journal of Advanced Engineering Technology and Sciences, 11(2), 127–133. https://doi.org/10.30574/wjaets.2024.11.2.0093
- Rajapakse, R.N., Zahedi, M., Babar, M.A. and Shen, H. (2022). Challenges and solutions when adopting DevSecOps: A systematic review. Information and Software Technology, 141, 106700. https://doi.org/10.1016/j.infsof.2021.106700
- Wadhams, Z.D., Izurieta, C. and Reinhold, A.M. (2024). Barriers to Using Static Application Security Testing (SAST) Tools: A Literature Review. Proceedings of the 39th IEEE/ACM International Conference on Automated Software Engineering Workshops, 161–166. https://doi.org/10.1145/3691621.3694947
- War, A., Habib, A., Diallo, A., Klein, J. and Bissyandé, T.F. (2023). Security Vulnerabilities in Infrastructure as Code: What, How Many, and Who? Research Square. https://doi.org/10.21203/rs.3.rs-3600645/v1

---

Appendix A: Pipeline skeleton and usage

- See `.gitlab-ci.yml` for a modular CI/CD pipeline with SAST, SCA, IaC, and container scanning stages designed for GitLab CI/CD.
- Local quick checks (optional) can be run with Dockerized tools (see README for commands).

Appendix B: Gantt chart

- See `docs/gantt.md` for a Mermaid Gantt chart visualizing project milestones and timelines.
# Create volumes for persistence
docker volume create sq-data
docker volume create sq-extensions

# Start SonarQube (Community Edition)
docker run -d --name sonarqube \
  -p 9000:9000 \
  -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true \
  -v sq-data:/opt/sonarqube/data \
  -v sq-extensions:/opt/sonarqube/extensions \
  sonarqube:community