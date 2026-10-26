# CI/CD interview questions and answers

This guide covers continuous integration, delivery, deployment, pipeline security, release strategies, and recovery. GitHub Actions supplies concrete examples, while the design questions apply to other CI/CD systems.

## Study checklist

- [ ] Describe the path from a commit to production for one project.
- [ ] Explain which checks run before merge and which run after deployment.
- [ ] Describe how to stop or reverse a faulty release.

## Pipeline foundations

Sources: [DORA continuous delivery](https://dora.dev/capabilities/continuous-delivery/), [GitHub Actions workflows](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows), [GitHub Actions deployment guide](https://docs.github.com/en/actions/get-started/continuous-deployment).

### 1. What is continuous integration?

Developers integrate changes into a shared branch frequently, and an automated pipeline builds and checks each change. The goal is to find integration problems while changes are small.

### 2. How do continuous delivery and continuous deployment differ?

Continuous delivery keeps the software releasable and allows a production release on demand. Continuous deployment automatically releases every change that passes the required pipeline. A manual approval can be part of a delivery process without making it continuous deployment.

### 3. What is a pipeline?

A defined sequence or graph of automated work, such as installing dependencies, checking code, building an artifact, testing it, and deploying it. A failed required gate stops the change from moving forward.

### 4. What should run on a pull request?

Run checks that give useful feedback before merge: formatting or linting, type checks, unit and integration tests, build verification, and relevant security checks. Keep the fast checks early and report failures clearly.

### 5. What is the difference between a job and a step in GitHub Actions?

A workflow contains jobs. Each job runs on a runner and contains steps that execute commands or actions. Jobs can run concurrently unless dependencies such as `needs` order them.

### 6. What triggers a GitHub Actions workflow?

Repository events such as `push` or `pull_request`, a manual `workflow_dispatch`, a schedule, or a call from another workflow can trigger it. Select triggers carefully because they affect permissions and trust.

### 7. Why should a pipeline be reproducible?

The same source, dependency versions, and build configuration should produce a predictable artifact. Lock dependencies, identify the build image, and record the commit and artifact digest so a release can be traced.

### 8. What is a build artifact?

The output promoted toward deployment, such as a compiled bundle, package, or container image. Build it once and promote the same artifact through environments when possible; rebuilding for production can produce a different result.

### 9. What is the difference between a dependency cache and a workflow artifact?

A cache speeds later runs by reusing dependencies or intermediate files and must be safe to miss. An artifact preserves a specific run's outputs, such as a built package, report, or logs, for later jobs or review.

## Tests and quality gates

Sources: [GitHub Actions dependency caching](https://docs.github.com/en/actions/concepts/workflows-and-actions/dependency-caching), [workflow artifacts](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflow-artifacts), [DORA metrics](https://dora.dev/guides/dora-metrics/).

### 10. How do you order pipeline checks?

Run cheap, high-signal checks early, then slower integration and browser tests. Parallelize independent checks, but keep failures visible rather than hiding them behind retries.

### 11. What is a flaky test, and how should CI handle it?

A flaky test changes result without a relevant code change. Investigate shared state, timing, data, and environment differences. Retries can collect evidence, but permanent retries can hide defects.

### 12. What is a quality gate?

A condition a change must satisfy before promotion, such as a passing build, required tests, review, or security policy. Gates should reflect actual release risk rather than arbitrary metrics.

### 13. Why is 100% code coverage not a sufficient gate?

Coverage shows which code ran, not whether assertions catch failures. Test critical behavior and failure paths, then use coverage to find gaps rather than to prove correctness.

### 14. How do you test database migrations in a pipeline?

Apply migrations to a clean database and test the app against the migrated schema. For a live system, check compatibility with both old and new app versions during rollout and plan a recovery path.

### 15. What should a preview environment provide?

An isolated deployment of a proposed change with enough dependencies and data to review real behavior. Protect secrets and private data, and clean up environments after their change is closed.

## Deployment and recovery

Sources: [GitHub deployment environments](https://docs.github.com/en/actions/concepts/workflows-and-actions/deployment-environments), [GitHub deployment controls](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/control-deployments), [GitHub Actions concurrency](https://docs.github.com/en/actions/concepts/workflows-and-actions/concurrency).

### 16. What is the difference between deployment and release?

Deployment places code or an artifact in an environment. Release makes behavior available to users. Feature flags can separate those moments, but flags need ownership and cleanup.

### 17. What is a rolling deployment?

Instances are replaced gradually while some old instances remain. The app and database must tolerate mixed versions during the rollout.

### 18. What are blue-green and canary deployments?

Blue-green switches traffic between two complete environments. A canary sends a small share of traffic to the new version first. Both need health signals and a way to stop or reverse traffic.

### 19. What should a production deployment verify after rollout?

Check application health, error rate, latency, key user journeys, and relevant business signals. A successful deployment command only proves that the rollout mechanism completed.

### 20. What is a rollback?

Returning traffic or code to a previous known-good version. Database changes and external side effects may not roll back with the application, so design migrations and writes with recovery in mind.

### 21. How do you avoid two deployments racing each other?

Serialize deployments to the same environment with a concurrency control or deployment lock. Decide whether an older pending release should be canceled or queued. Verify the deployed commit before promoting the next one.

### 22. What is the purpose of staging?

It provides a production-like place to check a release before production. Its value depends on how closely configuration, dependencies, and traffic behavior match production; it does not replace production monitoring.

### 23. Why are backward-compatible database migrations important?

A rolling deployment can have old and new app versions running together. Add and backfill new fields before switching readers or writers; remove old fields only after all old code is gone.

## Pipeline security and supply chain

Sources: [GitHub Actions secure use](https://docs.github.com/en/actions/reference/security/secure-use), [GitHub Actions OIDC](https://docs.github.com/en/actions/reference/security/oidc), [GitHub Actions deployment environments](https://docs.github.com/en/actions/concepts/workflows-and-actions/deployment-environments).

### 24. What is the principle of least privilege in CI/CD?

Give each job only the repository and cloud permissions it needs. Separate untrusted pull request checks from privileged publishing or deployment work.

### 25. Why are pull requests from forks a security concern?

They can execute untrusted code in a runner. Do not expose deployment credentials to that code. Be especially careful when a privileged workflow checks out or runs fork content.

### 26. Why pin third-party actions to a commit SHA?

A full commit SHA selects an immutable action version, unlike a tag that can move. Review the action's source and update the pin deliberately when a new version is needed.

### 27. What is OIDC used for in deployment pipelines?

The CI provider issues a short-lived identity token that a cloud provider validates against a trust policy. This can replace long-lived cloud keys stored as CI secrets. Restrict the trust policy to the intended repository, branch, workflow, or environment.

### 28. How should deployment secrets be handled?

Store them in a managed secret system or protected environment, grant access only to the deploy job, and avoid printing them. Rotate credentials when exposed. Never assume log masking catches transformed secret values.

### 29. What is an artifact attestation?

A signed statement about an artifact's origin and build process. It helps a deployer verify that the artifact came from an expected workflow and source revision; it does not by itself prove the software is bug-free.

### 30. Why should CI caches be treated as untrusted input?

A cache can be restored by a later run and may be writable from a different trust context. Do not place secrets in it, and avoid executing cached content without checking its source and integrity.

## Measurement and scenarios

Sources: [DORA software delivery metrics](https://dora.dev/guides/dora-metrics/), [GitHub Actions deployment controls](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/control-deployments).

### 31. What are the current DORA software delivery metrics?

DORA groups five measures into throughput and instability: change lead time, deployment frequency, failed deployment recovery time, change fail rate, and deployment rework rate. Use them to find friction in a team's own system, not to rank individual developers.

### 32. A pipeline passes, but users see errors after deployment. What do you do?

Confirm the impact, stop further rollout, and use the fastest safe mitigation: disable the feature, shift traffic, or roll back. Inspect deployment logs and production signals, then add a check for the missed failure mode.

### 33. CI takes 45 minutes and developers stop running it. What do you change?

Measure where time goes. Cache safe dependencies, parallelize independent jobs, move fast checks earlier, and reserve expensive end-to-end suites for the right stage. Do not remove checks without understanding the risk they catch.

### 34. A secret appears in a pipeline log. What is the immediate response?

Revoke or rotate it and restrict access to the log. Investigate where it was exposed, remove the logging path, and review whether the credential was used unexpectedly.

### 35. A release requires a manual production approval. Is the team practicing continuous delivery?

It can be, if every change remains releasable and the approval is the final release decision. It is not continuous deployment because production release is not automatic.

## Further reading

- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [DORA continuous delivery](https://dora.dev/capabilities/continuous-delivery/)
- [DORA metrics](https://dora.dev/guides/dora-metrics/)
