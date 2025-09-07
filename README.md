# Cloud DevOps Crash Camp (4 Months) — Senior-Track, Full Course Content

**Audience**: Experienced Python backend/data engineers transitioning to Senior DevOps/Cloud Engineer.

**Tech Stack**: AWS, Linux, GitHub, GitHub Actions, Jenkins, Docker, Kubernetes, Terraform, Ansible, Prometheus/Grafana, ELK, Vault.

**Pace**: 16 weeks. Each week has detailed lessons, hands-on labs, real project deliverables, and interview prep. Expect 2–4 hours/day, 6 days/week.

**Projects**: Mix of Python (Flask, FastAPI, Django) and JavaScript (Node.js, Express, React, Next.js) applications deployed across different cloud patterns.

**How to work**: Ship daily to GitHub. Document everything in READMEs. Break things, then fix them. Treat each weekly project as portfolio-grade.

---

## Course Structure

This course is designed as a complete learning platform with:
- **Detailed Lessons**: Step-by-step explanations with code examples
- **Hands-on Labs**: Interactive exercises with validation
- **Real Projects**: Production-ready applications you can showcase
- **Troubleshooting Guides**: Common issues and solutions
- **Interview Prep**: Technical questions and system design challenges
- **Portfolio Building**: GitHub repos that demonstrate senior-level skills

---

## Month 1 — Foundations, Containers, and GitOps

### Week 1 — Linux, Git, Shell, and AWS Fundamentals
- **Learning objectives**
  - Master Linux tooling, core networking, Git workflows, and AWS IAM/Billing/CLI.
  - Build reproducible dev environments and automations.
- **Daily tasks**
  - Day 1: Linux mastery — namespaces, cgroups intro, `systemd`, services, logs. Build dotfiles.
  - Day 2: Networking — TCP/IP basics, DNS, TLS; tools: `curl`, `dig`, `ss`, `tcpdump`.
  - Day 3: Git deep-dive — rebase, bisect, subtree, git-flow vs trunk, semantic commits.
  - Day 4: AWS setup — org structure, IAM users/roles, MFA, budgets, AWS CLI + profiles.
  - Day 5: Shell automation — idempotent bash, traps, error handling, shebangs, `Makefile`.
  - Day 6: Project day — Provision IAM and budget alarms; write auditable bootstrap scripts.
- **Project deliverable**
  - Secure AWS onboarding kit with budget alarms and CLI profiles per account.
- **Repo**
  - `cloud-foundations-aws/`
  ```
  cloud-foundations-aws/
    README.md
    scripts/
      bootstrap_aws_user.sh
      create_budget_alarm.sh
    iam/
      policies/
      roles/
    .github/workflows/lint.yml
    Makefile
  ```
- **Self-assessment checkpoints**
  - Can you rotate access keys, enforce MFA, and audit CloudTrail findings?
  - Can you recover a broken Linux service with only logs and `systemctl`?
- **Interview prep**
  - Concept: What’s the difference between a role and a user in IAM? When to use each?
  - Troubleshoot: 403 AccessDenied on S3 with assumed role — where do you look?
  - System design: Secure multi-account AWS layout for dev/stage/prod.

### Week 2 — Docker and Image Supply Chain
- **Learning objectives**
  - Build minimal, secure images; multi-stage builds; cache optimization; SBOM and signing.
- **Daily tasks**
  - Day 1: Dockerfiles — least-privilege, non-root, minimal base images (distroless/alpine).
  - Day 2: Multi-stage builds for Python/Flask and Node apps; layer caching strategies.
  - Day 3: Image scanning — Trivy/Grype; SBOM (Syft); `cosign` signing and attestations.
  - Day 4: Private registries — ECR; lifecycle policies; immutable tags; pull-through cache.
  - Day 5: Local orchestration — docker-compose; healthchecks; `.env` and secrets patterns.
  - Day 6: Project day — Build SBOM+signed images; supply-chain README and demo.
- **Project deliverable**
  - Secure container pipeline with SBOM, vulnerability scanning, and signature verification.
- **Repo**
  - `secure-container-factory/`
  ```
  secure-container-factory/
    app/
      flask_app/
        app.py
        requirements.txt
    docker/
      Dockerfile
      docker-compose.yml
    supply-chain/
      generate_sbom.sh
      sign_image.sh
      verify_signature.sh
    .github/workflows/ci.yml
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you reduce image size by 80% while passing scans?
  - Can you prove provenance of an image and verify signature before deploy?
- **Interview prep**
  - Concept: Why non-root containers? What can still break?
  - Troubleshoot: App starts locally but healthcheck fails in compose — root causes?
  - System design: Private registry with promotion gates (dev→stage→prod).

### Week 3 — Intro to Kubernetes (Local) + Core Objects
- **Learning objectives**
  - Deploy, expose, and manage apps with Deployments, Services, Ingress, ConfigMaps, Secrets.
- **Daily tasks**
  - Day 1: k3d/kind setup; `kubectl` mastery; contexts; `k9s`.
  - Day 2: Deployments/Rollouts; probes; HPA basics; resource requests/limits.
  - Day 3: Services/Ingress; DNS; path vs host routing; TLS with cert-manager (local).
  - Day 4: ConfigMaps/Secrets; sealed-secrets introduction; RBAC basics.
  - Day 5: Observability basics — `kubectl top`, events, logs, describe; troubleshooting drill.
  - Day 6: Project day — 2 services + ingress + HPA + config/secrets.
- **Project deliverable**
  - Local k8s stack with blue/green rollouts and autoscaling triggers.
- **Repo**
  - `k8s-fundamentals-lab/`
  ```
  k8s-fundamentals-lab/
    manifests/
      namespace.yaml
      deployment-web.yaml
      deployment-api.yaml
      service-web.yaml
      service-api.yaml
      ingress.yaml
      hpa.yaml
      configmap.yaml
      secret.yaml
    scripts/
      deploy.sh
      rollback.sh
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you pause/resume rollouts and do a manual promotion?
  - Can you explain readiness vs liveness vs startup probes?
- **Interview prep**
  - Concept: How does kube-proxy route traffic? What is ClusterIP vs NodePort vs LoadBalancer?
  - Troubleshoot: Pod CrashLoopBackOff with OOMKilled — fix steps?
  - System design: Ingress for multi-tenant apps with cert-manager.

### Week 4 — Terraform (IaC) Fundamentals
- **Learning objectives**
  - Author reusable modules; remote state; workspaces; dependency graphs; cost-aware infra.
- **Daily tasks**
  - Day 1: Basics — providers, resources, variables, outputs; state and drift.
  - Day 2: Modules — structure, versioning, interfaces; enforce tagging across resources.
  - Day 3: Remote backend (S3 + DynamoDB); locking; workspaces for envs.
  - Day 4: Data sources; `for_each`; `depends_on`; dynamic blocks; conditional creation.
  - Day 5: Security — least-privilege execution role; tfsec/checkov; pre-commit hooks.
  - Day 6: Project day — VPC, subnets, NAT, IGW, route tables, ECR, S3 backed TF.
- **Project deliverable**
  - Production-grade network foundation via Terraform, with validated policies.
- **Repo**
  - `terraform-aws-network-foundation/`
  ```
  terraform-aws-network-foundation/
    modules/
      vpc/
        main.tf
        variables.tf
        outputs.tf
    envs/
      dev/
        main.tf
        backend.tf
      prod/
        main.tf
        backend.tf
    policies/
      iam.tf
    .pre-commit-config.yaml
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you recover from state drift safely?
  - Can you promote a module version across environments?
- **Interview prep**
  - Concept: Plan/apply lifecycle; when to split states?
  - Troubleshoot: Orphaned NAT Gateway costs — detection and remediation?
  - System design: Multi-account remote state architecture with least-privilege roles.

---

## Month 2 — Config Management, CI/CD, and AWS Deployments

### Week 5 — Ansible for Immutable + Mutable Patterns
- **Learning objectives**
  - Idempotent playbooks, roles, inventories, and secrets; image baking vs runtime config.
- **Daily tasks**
  - Day 1: Inventories (static/dynamic), groups, host vars; `ansible-vault`.
  - Day 2: Roles/collections; handlers; tags; check mode; diff mode.
  - Day 3: Provision EC2 and configure NGINX, system users, firewalld.
  - Day 4: Packer + Ansible for AMI baking; golden image strategy.
  - Day 5: Testing — Molecule + local docker/VM testing.
  - Day 6: Project day — Bake hardened AMI + idempotent runtime config.
- **Project deliverable**
  - Hardened AMI and Ansible roles with Molecule tests.
- **Repo**
  - `ansible-ops-kit/`
  ```
  ansible-ops-kit/
    inventories/
      dev/
        hosts.ini
    roles/
      base_hardening/
      nginx/
    playbooks/
      site.yml
      web.yml
    molecule/
      base_hardening/
    packer/
      web_ami.pkr.hcl
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you run playbooks in check mode and ensure no changes?
  - Can you test roles locally with Molecule?
- **Interview prep**
  - Concept: When to bake configs into images vs apply with Ansible at runtime?
  - Troubleshoot: SSH connection errors and privilege escalation issues.
  - System design: Immutable infra pipeline using Packer + Ansible + ASG.

### Week 6 — GitHub Actions CI/CD
- **Learning objectives**
  - Build/test/lint, matrix builds, environment protections, OIDC to AWS, secrets strategy.
- **Daily tasks**
  - Day 1: Workflows/triggers; reusable workflows; cache strategies.
  - Day 2: Build Docker images; push to ECR; OIDC federation for deploys.
  - Day 3: Environment approvals; deployment protection rules; manual gates.
  - Day 4: Automated tests, security scans (CodeQL/trivy), SBOM and cosign in CI.
  - Day 5: Deployment strategies — canary/blue-green via GitHub Environments.
  - Day 6: Project day — End-to-end CI/CD for a Flask API to EC2/ECS.
- **Project deliverable**
  - Secure CI/CD with signed artifacts and gated promotions.
- **Repo**
  - `gha-flask-cicd/`
  ```
  gha-flask-cicd/
    app/
      api.py
    .github/workflows/
      build.yml
      deploy-dev.yml
      deploy-prod.yml
      reusable/
        docker_build_and_sign.yml
    infra/
      ecs/
      iam/
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you deploy with OIDC without static AWS keys?
  - Can you pause a prod deploy with an approval gate?
- **Interview prep**
  - Concept: GitHub Environments vs Encrypted Secrets vs OIDC providers.
  - Troubleshoot: Stuck runners and cache poisoning.
  - System design: Monorepo CI matrix for microservices, with shared reusable workflows.

### Week 7 — Jenkins for Enterprises
- **Learning objectives**
  - Declarative Pipelines, agents, shared libraries, credentials, pipelines as code, RBAC.
- **Daily tasks**
  - Day 1: Jenkins install via Docker; controllers vs agents; ephemeral agents.
  - Day 2: Declarative pipelines; stages; post conditions; artifacts.
  - Day 3: Shared libraries; global vars; linting Jenkinsfiles.
  - Day 4: Credentials; AWS deploy with OIDC or IRSA; folder-level RBAC.
  - Day 5: Scaling agents (Kubernetes plugin); job retention; audit.
  - Day 6: Project day — Jenkins pipeline deploying to ECS/EKS with shared library.
- **Project deliverable**
  - Enterprise-grade Jenkins with shared library and EKS deploy stage.
- **Repo**
  - `jenkins-cd-blueprints/`
  ```
  jenkins-cd-blueprints/
    Jenkinsfile
    vars/
      dockerBuild.groovy
      deployToEks.groovy
    k8s/
      jenkins-agents.yaml
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you run ephemeral k8s agents and cache Docker layers efficiently?
  - Can you audit who deployed what, when?
- **Interview prep**
  - Concept: Jenkins vs GitHub Actions tradeoffs.
  - Troubleshoot: Credential binding failures in agents.
  - System design: HA Jenkins on Kubernetes with S3-backed config-as-code.

### Week 8 — AWS Deployments: EC2, ECS, and EKS
- **Learning objectives**
  - Choose the right compute; implement blue/green and autoscaling; cost awareness.
- **Daily tasks**
  - Day 1: EC2 ASG + ALB basics; user-data vs baked AMIs.
  - Day 2: ECS Fargate vs EC2; task defs; service discovery; blue/green.
  - Day 3: EKS — cluster bootstrap with Terraform (eksctl or module); IRSA; nodegroups.
  - Day 4: Ingress (ALB Controller) and ExternalDNS.
  - Day 5: Cost monitoring — tags; AWS Budgets; Compute Optimizer; Savings Plans overview.
  - Day 6: Project day — Pick one path (EC2/ECS/EKS) and ship a production-ready stack.
- **Project deliverable**
  - Production deploy on chosen AWS compute with autoscaling and route management.
- **Repo**
  - `aws-deployments-starter/`
  ```
  aws-deployments-starter/
    terraform/
      vpc/
      eks/
      ecs/
      ec2_asg/
    k8s/
      ingress/
      external-dns/
    app/
      k8s/
      ecs/
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you justify ECS vs EKS vs EC2 for a given workload?
  - Can you enable IRSA and verify it works?
- **Interview prep**
  - Concept: ALB Ingress Controller vs NGINX Ingress.
  - Troubleshoot: Pods can’t pull from ECR — why?
  - System design: Multi-region active/active strategy for an API.

---

## Month 3 — Observability, Security, SRE

### Week 9 — Prometheus, Grafana, and Alerting
- **Learning objectives**
  - Metrics/labels, service discovery, recording rules, alerts, and SLOs.
- **Daily tasks**
  - Day 1: Prometheus Operator; CRDs (ServiceMonitor/PodMonitor); scrape configs.
  - Day 2: Grafana dashboards; provisioning; folder-as-code.
  - Day 3: Alertmanager routing; silences; on-call policies.
  - Day 4: RED/USE dashboards; histogram buckets; high-cardinality pitfalls.
  - Day 5: SLOs with burn-rate alerts; latency and error budgets.
  - Day 6: Project day — Full metrics + alerts for your Week 8 service.
- **Project deliverable**
  - SLO-driven alerts with Grafana dashboards, provisioned from code.
- **Repo**
  - `observability-stack-k8s/`
  ```
  observability-stack-k8s/
    k8s/
      prometheus/
      grafana/
      alertmanager/
    dashboards/
      services.json
    alerts/
      slo_rules.yaml
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you simulate burn-rate alerts and verify paging?
  - Can you avoid high-cardinality label explosions?
- **Interview prep**
  - Concept: Difference between metrics, logs, traces; when to sample?
  - Troubleshoot: Missing metrics after deploy — where’s the break?
  - System design: SLOs for a multi-tenant API.

### Week 10 — Logging with ELK/OpenSearch
- **Learning objectives**
  - Structured logging, indexing strategy, pipelines, retention, cost optimization.
- **Daily tasks**
  - Day 1: Fluent Bit/Fluentd on k8s; log enrichment; JSON structure.
  - Day 2: Elasticsearch/OpenSearch setup; index lifecycle management (ILM).
  - Day 3: Kibana dashboards; saved searches; RBAC for teams.
  - Day 4: Correlate logs with metrics (labels/trace IDs).
  - Day 5: Cost controls — retention tiers, compression, hot/warm, sampling.
  - Day 6: Project day — Centralized logs with parsing, enrichment, and RBAC.
- **Project deliverable**
  - End-to-end log pipeline with ILM and search dashboards.
- **Repo**
  - `central-logging-k8s/`
  ```
  central-logging-k8s/
    k8s/
      fluent-bit/
      opensearch/
      kibana/
    pipelines/
      parsers/
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you trace a request from logs to metrics?
  - Can you prevent noisy logs from exploding costs?
- **Interview prep**
  - Concept: Trade-offs ELK vs hosted solutions (CloudWatch, Datadog).
  - Troubleshoot: Fluent Bit backpressure; dropped logs.
  - System design: Multi-tenant log ingestion with RBAC.

### Week 11 — Secrets Management and Security Hardening
- **Learning objectives**
  - Vault, IRSA/KMS, secret rotation, K8s secrets patterns, policy as code.
- **Daily tasks**
  - Day 1: HashiCorp Vault with AWS auth; policies; dynamic DB creds.
  - Day 2: Vault Agent Injector on k8s; secret templates; renewal/rotation.
  - Day 3: KMS + SOPS/SealedSecrets; GitOps-friendly secrets flows.
  - Day 4: ECR/CIS hardening; security scanning in CI; admission controllers (OPA/Gatekeeper).
  - Day 5: IAM best practices; minimal privileges; access boundaries; SCPs.
  - Day 6: Project day — Secrets platform with rotation and app integration.
- **Project deliverable**
  - Vault-backed secrets with auto-rotation and app-side injection.
- **Repo**
  - `secrets-hardened-platform/`
  ```
  secrets-hardened-platform/
    vault/
      policies/
      kubernetes/
    k8s/
      injector/
      sops/
    ci/
      scans.yml
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you rotate DB creds without app downtime?
  - Can you block non-compliant manifests with policy?
- **Interview prep**
  - Concept: IRSA vs node role — when and why?
  - Troubleshoot: Pod unable to read Vault secret — auth flow?
  - System design: Enterprise secret management with audit and break-glass.

### Week 12 — SRE, Reliability, and Incident Management
- **Learning objectives**
  - SLOs/SLIs, error budgets, on-call, runbooks, chaos drills, capacity planning.
- **Daily tasks**
  - Day 1: SLO design; SLIs; golden signals; dashboards per service.
  - Day 2: Incident process; IMOCs; comms checklist; postmortems (blameless).
  - Day 3: Runbooks; playbooks; auto-remediation hooks.
  - Day 4: Chaos experiments (Pod kill, node drain, AZ loss) and recovery.
  - Day 5: Capacity/scaling tests; load testing (k6/Vegeta).
  - Day 6: Project day — Define SLOs, load test, set alerts, run chaos drills.
- **Project deliverable**
  - SRE pack: SLOs, runbooks, chaos scripts, and postmortem template.
- **Repo**
  - `sre-readiness-kit/`
  ```
  sre-readiness-kit/
    slo/
      service_slos.yaml
    runbooks/
      api_runbook.md
    chaos/
      experiments/
      scripts/
    load/
      k6/
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you quantify error budget burn and adjust release cadence?
  - Can you run and recover from a simulated incident within 30 minutes?
- **Interview prep**
  - Concept: Toil vs automation; removing toil effectively.
  - Troubleshoot: Latency spikes under specific traffic; histogram analysis.
  - System design: Nines budget and trade-offs.

---

## Month 4 — GitOps, Capstone, and Interview Readiness

### Week 13 — GitOps with Argo CD or Flux
- **Learning objectives**
  - Declarative deployments, app-of-apps, multi-env promotion, drift detection.
- **Daily tasks**
  - Day 1: Argo CD install; app-of-apps pattern; RBAC; SSO.
  - Day 2: Environment overlays with Kustomize/Helm; parameterization.
  - Day 3: Progressive delivery with Argo Rollouts; canary/analysis.
  - Day 4: Policy-as-code (OPA/Gatekeeper) integrated with GitOps.
  - Day 5: Drift detection and auto-reconcile; sync windows.
  - Day 6: Project day — GitOps pipeline for two services dev→stage→prod.
- **Project deliverable**
  - Multi-env GitOps with progressive delivery gates.
- **Repo**
  - `gitops-platform-argo/`
  ```
  gitops-platform-argo/
    apps/
      dev/
      stage/
      prod/
    argo/
      project.yaml
      app-of-apps.yaml
    policies/
    README.md
  ```
- **Self-assessment checkpoints**
  - Can you recover from drift without manual kubectl?
  - Can you roll back failed canary automatically?
- **Interview prep**
  - Concept: GitOps vs CI-driven deploys.
  - Troubleshoot: Argo OutOfSync loops; hook failures.
  - System design: Multi-cluster GitOps with shared CI.

### Week 14 — Capstone Scaffolding and Architecture
- **Learning objectives**
  - Design the full system; plan repos; wire environments; define SLOs and budgets.
- **Daily tasks**
  - Day 1: Requirements and architecture doc (ADR). Choose EKS path (recommended).
  - Day 2: Plan repo structure, environments, and promotion strategy.
  - Day 3: Define IAM model, network layout, IRSA roles, secrets design.
  - Day 4: Draft SLOs, monitoring plan, and alert routes.
  - Day 5: Cost model and scaling strategy; tagging strategy.
  - Day 6: Project day — Commit ADRs, diagrams, and repo skeletons.
- **Project deliverable**
  - Capstone blueprint and empty-but-runnable skeletons.
- **Repo**
  - `capstone-cloud-native-platform/` (monorepo or polyrepo; example below in Week 16)

### Week 15 — Capstone Implementation (Infra, App, CI/CD)
- **Learning objectives**
  - Stand up AWS infra with Terraform; cluster bootstrap; app containerization; CI/CD.
- **Daily tasks**
  - Day 1: Terraform VPC, EKS, nodegroups, ECR, S3 state; `terraform apply`.
  - Day 2: IRSA roles for app and controllers; ALB Ingress; ExternalDNS; cert-manager.
  - Day 3: Containerize app; SBOM; sign image; push to ECR.
  - Day 4: GitHub Actions: build, scan, sign, deploy to dev via GitOps.
  - Day 5: Stage environment; promotion gate; run integration tests.
  - Day 6: Project day — Documentation and dry-run for prod.
- **Project deliverable**
  - Running dev+stage environments with CI/CD and GitOps promotion.

### Week 16 — Capstone Completion (Observability, Security, SRE, Prod)
- **Learning objectives**
  - Observability stack, security hardening, SLOs/alerts, prod cutover, incident drill.
- **Daily tasks**
  - Day 1: Prometheus/Grafana, SLO dashboards, burn-rate alerts.
  - Day 2: Logs: Fluent Bit → OpenSearch, ILM policies, dashboards.
  - Day 3: Vault/KMS; secret injection; rotation; policy enforcement.
  - Day 4: Policy gates (OPA/Gatekeeper); admission controls; image signature verify.
  - Day 5: Prod promotion; chaos test; load test; postmortem dry-run.
  - Day 6: Project day — Final README, diagrams, demo script, and resume bullets.
- **Project deliverable**
  - Production-ready platform with app, infra, CI/CD, monitoring, logging, and security.

---

## Capstone: Production-Ready Cloud-Native Platform on AWS (EKS)

- **Goal**
  - App → Docker → signed artifact → CI/CD → Terraform infra → Ansible/Packer optional → EKS → GitOps → Observability/Alerts → Security hardening.
- **Repos (polyrepo example)**
  - `capstone-infra-aws/` — Terraform EKS/VPC/ECR/IRSA
  - `capstone-app/` — Flask API + Docker + Helm chart
  - `capstone-gitops/` — Argo CD app-of-apps, env overlays
  - `capstone-observability/` — Prometheus/Grafana/Alertmanager configs
  - `capstone-security/` — Vault + policies; Gatekeeper constraints
- **Example structures**
  - `capstone-infra-aws/`
  ```
  capstone-infra-aws/
    modules/
      vpc/
      eks/
      iam-irsa/
    envs/
      dev/
        backend.tf
        main.tf
      stage/
      prod/
    README.md
  ```
  - `capstone-app/`
  ```
  capstone-app/
    app/
      main.py
      requirements.txt
    docker/
      Dockerfile
    charts/app/
      Chart.yaml
      templates/
    tests/
      unit/
    .github/workflows/
      build_sign_push.yml
    README.md
  ```
  - `capstone-gitops/`
  ```
  capstone-gitops/
    argo/
      project.yaml
      app-of-apps.yaml
    apps/
      dev/app/
      stage/app/
      prod/app/
    policies/
      gatekeeper/
    README.md
  ```
  - `capstone-observability/`
  ```
  capstone-observability/
    k8s/
      kube-prometheus-stack/
    dashboards/
      app.json
    alerts/
      slo_burn.yaml
    README.md
  ```
  - `capstone-security/`
  ```
  capstone-security/
    vault/
      policies/
      injector/
    admission/
      gatekeeper/
      cosign-verify/
    README.md
  ```
- **Step-by-step exercises**
  1) Terraform: create VPC, EKS, nodegroups, ECR, OIDC provider, IRSA roles.
  2) Bootstrap controllers: ALB Ingress, ExternalDNS, cert-manager, metrics-server.
  3) Build app: minimal Docker, SBOM, scan, sign; push to ECR.
  4) GitHub Actions: build→scan→sign→push; on tag, update `capstone-gitops` with new Helm values; PR approvals gate stage/prod.
  5) GitOps: Argo CD app-of-apps deploys app and platform add-ons; drift detection on.
  6) Observability: scrape app metrics; Grafana dashboards; SLO alerts wired to Slack.
  7) Logging: Fluent Bit to OpenSearch; ILM; correlation IDs.
  8) Security: Vault Injector for DB creds; Gatekeeper blocks unsigned images; IRSA enforced.
  9) SRE: Define SLOs, run k6 load, chaos experiments, document postmortem.
- **Demo script**
  - Trigger CI on tag, see signed image; Argo sync to dev; promote to stage via PR; show dashboards, alerts firing on induced error; show policy blocking unsigned image; show Vault-rotated creds with no downtime.

---

## Weekly “Apply While Learning”

- **GitHub**
  - Commit daily with descriptive messages; open PRs even for solo work; use Issues/Projects.
  - Include architecture diagrams (PlantUML/Mermaid) and ADRs per major decision.
- **LinkedIn**
  - Post weekly: what you built, metrics improved, a lesson learned, a graph/screenshot.
- **Resume bullets (convert weekly deliverables)**
  - Example: “Designed and deployed EKS platform on AWS using Terraform and Argo CD; implemented OIDC-based CI/CD with signed container artifacts, improving deployment lead time by 65% while enforcing admission policies for supply-chain security.”

---

## Daily Coding and Deployment Ritual

- Start with a tiny test or probe.
- Implement; write docs and Makefile targets.
- Validate with linters/scanners; run one failure injection.
- Push branch, open PR, self-review; squash and merge.
- Tag releases; update `CHANGELOG.md`.
- End with a short retro: what broke, how you fixed it, what you automated.

---

## Sample Daily Tooling References

- **Linux**: `journalctl -u <svc> -xe`, `ss -tulpen`, `tc`, `ip`, `nsenter`
- **Git**: `git bisect run`, `git worktree`, `git rerere`, `pre-commit`
- **Docker**: multi-stage builds, `--mount=type=cache`, `buildx bake`
- **K8s**: `kubectl diff`, `kubectl rollout status`, `kustomize`, `helm template`
- **Terraform**: `tflint`, `tfsec`, `terraform graph | dot -Tpng`
- **Ansible**: `--check --diff`, Molecule, Packer
- **CI/CD**: GHA reusable workflows, OIDC to AWS, code signing (cosign)
- **Observability**: Prometheus Operator, k6, Alertmanager, Grafana provisioning
- **Logging**: Fluent Bit, OpenSearch ILM
- **Security**: Vault Injector, SOPS/SealedSecrets, Gatekeeper, IRSA

---

## Weekly Interview Prep (Recap by Theme)

- Week 1: IAM design, Linux service triage, multi-account strategy.
- Week 2: Supply chain security, image hardening, private registries.
- Week 3: Probes, rollouts, HPA, service types, DNS.
- Week 4: Terraform state, modules, backends, least-privilege.
- Week 5: Ansible idempotency, inventories, AMI baking vs runtime config.
- Week 6: CI/CD gates, OIDC federation, SBOM/signing in pipelines.
- Week 7: Jenkins agents, shared libraries, RBAC, scaling.
- Week 8: ECS vs EKS vs EC2 decisions, IRSA, Ingress controllers.
- Week 9: SLOs, burn-rate alerts, recording rules, dashboards.
- Week 10: ILM, structured logging, enrichment, cost control.
- Week 11: Vault auth flows, rotation strategies, admission policies.
- Week 12: On-call, runbooks, chaos drills, capacity planning.
- Week 13: GitOps patterns, drift detection, progressive delivery.
- Weeks 14–16: Architecture, trade-offs, defense of capstone design, cost/SLA.

---

## Self-Assessment Rubric (Senior Readiness)

- **Architecture**
  - You justify AWS service choices, scaling, cost, and security trade-offs.
- **Reliability**
  - You define SLOs and tie alerts to user impact (burn rates).
- **Delivery**
  - You run OIDC-based CI/CD with signed artifacts and gated promotions.
- **Security**
  - You enforce policies to block unsigned or privileged pods; IRSA everywhere.
- **Operations**
  - You can diagnose failures from metrics/logs to root cause and write/run runbooks.
- **GitOps**
  - You reconcile drift and recover without imperative commands.
- **Cost**
  - You can show tagging, budgets, and ILM/Autoscaling cost controls.

---

## How to Defend in Interviews

- **Portfolio-first**: Walk through the capstone diagram, then a 3-minute demo video repo link.
- **Tell reliability stories**: “We injected failures X and Y; alerts fired; MTTR was Z.”
- **Metrics**: Show SLO dashboard and cost graphs.
- **Security**: Demonstrate admission policy blocking unsigned images and Vault rotation logs.
- **Trade-offs**: Be explicit about ECS vs EKS vs EC2; why you chose EKS for capstone.

---

## Suggested Repo Naming Map (All Weeks)

- `cloud-foundations-aws`
- `secure-container-factory`
- `k8s-fundamentals-lab`
- `terraform-aws-network-foundation`
- `ansible-ops-kit`
- `gha-flask-cicd`
- `jenkins-cd-blueprints`
- `aws-deployments-starter`
- `observability-stack-k8s`
- `central-logging-k8s`
- `secrets-hardened-platform`
- `sre-readiness-kit`
- `gitops-platform-argo`
- `capstone-infra-aws`
- `capstone-app`
- `capstone-gitops`
- `capstone-observability`
- `capstone-security`

---

## Final Notes

- Keep READMEs executable: every repo should have “Prereqs”, “Makefile targets”, “How to test”, “How to roll back”.
- Track learnings in `docs/journal.md` per repo: problems, fixes, automation added.
- Use issues/projects/milestones to mirror this syllabus; close them weekly.

Ship daily. Automate what hurt. Measure what matters.


