**Workflow Logic:**
- When does each stage run (always, on success, conditionally)
- Dependencies between stages
- Parallelization strategy
- Trigger conditions (branches, tags, manual)

## Platform and Tool Choices

**CI/CD Platform:** GitHub Actions (or GitLab CI, CircleCI, Jenkins, etc.)
- Why this platform: Already using GitHub, free for public repos, good ecosystem
- Alternatives considered: GitLab CI (would require migration), CircleCI (additional cost)

**Build Environment:**
- Runners: GitHub-hosted (linux-latest, windows-latest, macos-13, macos-14)
- Containerization: Docker for consistent environments (if applicable)
- Dependencies: Node 20, Python 3.11, etc. (specify versions)

**Tools and Technologies:**
- Build tools: npm, cargo, gradle, etc.
- Testing frameworks: jest, pytest, junit, etc.
- Artifact creation: specific tools for installers, packages
- Signing: code signing tools and certificate management
- Distribution: where artifacts are published

**Why these tools:**
Justify significant tool choices based on:
- Team familiarity
- Ecosystem support
- Cost considerations
- Integration capabilities
- Long-term maintenance

## Trigger Conditions

**Automatic Triggers:**
- Push to main branch → run tests and build
- Pull request opened/updated → run tests only
- Tag matching v*.*.* → full release pipeline
- Scheduled (cron): nightly builds, weekly security scans

**Manual Triggers:**
- Deploy to staging environment (manual approval required)
- Emergency hotfix deployment (skip certain checks)
- Rebuild artifacts without code changes (if needed)

**Conditional Logic:**
- Skip builds if only documentation changed
- Run expensive tests only on release candidates
- Deploy to production only on tagged releases

## Pipeline Configuration

**Stage Definitions:**

For each major stage, describe:
- What it does
- What tools it uses
- What it produces
- When it runs
- How long it should take
- What resources it needs

Example:
**Test Stage:**
- Runs all automated tests (unit, integration, e2e)
- Uses jest for JavaScript, pytest for Python
- Produces test reports and coverage data
- Runs on every commit and PR
- Target completion: under 5 minutes
- Resources: standard GitHub-hosted runner

**Build Stage:**
- Compiles application for target platforms
- Uses platform-specific build tools
- Produces executables or packages
- Runs after tests pass, on main branch and tags
- Target completion: under 10 minutes per platform
- Resources: platform-specific runners (Windows, macOS, Linux)

## Artifact Specifications

**Artifact Types:**

For each platform or output, specify:
- Format (executable, installer, container image, package)
- Naming convention (myapp-v1.0.0-windows-x64.exe)
- Location (GitHub Releases, package registry, artifact storage)
- Metadata (version, checksums, signatures)

**Examples:**
- Windows: MSI installer, signed, SHA256 checksum
- macOS: DMG disk image, notarized, includes both Intel and ARM binaries
- Linux: Flatpak package, published to Flathub
- Container: Docker image, pushed to Docker Hub, tagged with version

**Versioning:**
- How versions are determined (git tags, package.json, manual)
- Format (semantic versioning: v1.2.3)
- Where version appears (filenames, metadata, releases)

**Checksums and Signatures:**
- Generate SHA256 checksums for all artifacts
- Sign Windows executables with code signing certificate
- Notarize macOS applications with Apple Developer ID
- Include verification instructions in release notes

## Secrets and Configuration Management

**Required Secrets:**
- List all sensitive credentials needed
- Explain what each is used for
- Specify where they're stored (GitHub Secrets, AWS Secrets Manager, etc.)

Examples:
- `MACOS_CERTIFICATE`: Code signing certificate for macOS builds
- `MACOS_CERTIFICATE_PASSWORD`: Password for certificate
- `APPLE_ID`: Apple ID for notarization
- `DOCKER_HUB_TOKEN`: Authentication for pushing container images
- `DEPLOY_KEY`: SSH key for deployment to production servers

**Configuration Management:**
- Environment-specific configs (dev, staging, production)
- Feature flags or runtime configuration
- Platform-specific settings
- How configuration is injected (environment variables, config files, secrets)

**Security Considerations:**
- Secrets never logged or exposed in output
- Least privilege access (only stages that need secrets get them)
- Rotation policy for credentials
- Audit logging for secret access

## Resource and Performance Constraints

**Time Budgets:**
- Total pipeline time target: Complete within 30 minutes
- Critical path: Test and build stages should complete in 15 minutes
- Acceptable delays: Packaging and signing can take longer (up to 45 minutes)

**Resource Allocation:**
- CPU and memory: Standard runners sufficient, or need larger runners?
- Storage: How much artifact storage needed (per build, total retention)
- Network: Any large downloads or uploads that affect time?

**Cost Management:**
- Estimated monthly cost for runner minutes
- Artifact storage costs
- Cloud resource costs (if provisioning infrastructure)
- Strategies to reduce costs (caching, selective builds, cleanup policies)

**Caching Strategy:**
- What to cache (dependencies, build artifacts, Docker layers)
- Cache invalidation rules (when to rebuild)
- Cache size limits and cleanup

## Failure Handling and Recovery

**Failure Scenarios:**

Anticipate what can go wrong and how to handle it:

**Build Failures:**
- Test failures: Mark build as failed, notify team, don't proceed to deployment
- Compilation errors: Fail fast, provide clear error messages
- Platform-specific issues: Continue other platforms, mark partial success

**Infrastructure Failures:**
- Runner unavailable: Retry automatically (up to 3 times)
- Network timeout: Retry with exponential backoff
- External service down: Skip optional steps, fail required steps

**Partial Failures:**
- Some platforms succeed, others fail: Publish successful artifacts, report failures
- Optional steps fail: Log warning but continue pipeline
- Critical steps fail: Stop entire pipeline

**Recovery Strategies:**
- Automatic retries for transient failures
- Manual retry option for infrastructure issues
- Rollback capability if deployment causes problems
- Notification system for alerting team to failures

## Monitoring and Observability

**Metrics to Track:**
- Build success rate (percentage of builds that pass)
- Build duration (how long each stage takes)
- Queue time (how long builds wait for runners)
- Artifact size (track growth over time)
- Deployment frequency (how often we ship)

**Alerting:**
- Notify team on build failures (Slack, email, GitHub notifications)
- Alert on infrastructure issues (runner failures, quota exhaustion)
- Escalation for critical production deployment failures

**Dashboards:**
- Build history and trends
- Platform-specific success rates
- Performance over time (are builds getting slower?)
- Cost tracking (runner minutes used, storage consumed)

**Logging:**
- Pipeline logs stored and searchable
- Structured logging for easier debugging
- Retention policy (how long to keep logs)

## Operational Principles

These are the guiding principles for this infrastructure:

**Reliability:**
- Pipeline should succeed consistently when code is good
- Transient failures should be retried automatically
- Critical path should be robust and well-tested

**Fast Feedback:**
- Developers should know quickly if their changes break something
- Test stage completes in minutes, not hours
- Failures are reported immediately with actionable information

**Reproducibility:**
- Same code produces same artifacts every time
- Build environment is consistent and versioned
- Dependencies are locked and cached

**Observability:**
- Always possible to see what's happening
- Failures include enough context to debug
- Metrics available for performance and reliability tracking

**Cost Consciousness:**
- Don't waste resources on unnecessary work
- Cache aggressively to avoid redundant computation
- Clean up old artifacts to manage storage costs

**Security:**
- Secrets are protected and rotated
- Supply chain is verified (dependencies, tools)
- Artifacts are signed and verifiable

## Implementation Phases

For complex infrastructure, break into phases:

**Phase 1: Basic Pipeline (Week 1)**
- Set up testing stage
- Configure build for main platform
- Basic artifact upload to GitHub Releases
- Success metric: Tests run automatically on every commit

**Phase 2: Multi-platform Builds (Week 2)**
- Add Windows, macOS, Linux builds
- Implement caching for dependencies
- Parallel build execution
- Success metric: All platforms build successfully

**Phase 3: Release Automation (Week 3)**
- Add code signing and notarization
- Implement versioning from git tags
- Create installers and packages
- Success metric: Full release on tag creation

**Phase 4: Monitoring and Optimization (Week 4)**
- Add metrics and dashboards
- Implement failure notifications
- Optimize build times with better caching
- Success metric: Team visibility into build health

## Risks and Mitigation

**Technical Risks:**
- Platform-specific build issues: Test early, have platform maintainers available
- Dependency availability: Use caching, have mirrors for critical dependencies
- Runner capacity: Monitor queue times, upgrade plan if needed

**Operational Risks:**
- Secret compromise: Rotate regularly, monitor for unauthorized access
- Cost overruns: Set budget alerts, optimize resource usage
- Single point of failure: Document all configuration, cross-train team

**Process Risks:**
- Breaking changes to workflow: Communicate changes, provide migration guide
- Complexity growth: Regular reviews, simplify where possible
- Knowledge concentration: Document thoroughly, pair on maintenance

## Acceptance Criteria

**Infrastructure is Operational When:**
- [ ] Pipeline triggers automatically on specified conditions
- [ ] All stages complete successfully for valid code
- [ ] Artifacts are produced in correct formats for all platforms
- [ ] Artifacts are uploaded to specified locations
- [ ] Team receives notifications on failures
- [ ] Monitoring dashboards show pipeline health
- [ ] Documentation exists for common operations
- [ ] Secrets are properly configured and secured
- [ ] Cost is within expected budget

**Verification Steps:**
1. Push commit to main → pipeline runs and passes
2. Create tag → full release pipeline creates artifacts
3. Introduce test failure → pipeline fails and notifies team
4. Check artifact locations → all platforms present and correct
5. Review dashboards → metrics are being collected
6. Test failure scenarios → recovery mechanisms work
7. Verify costs → within budget expectations

Optional sections (include only if relevant):

## Compliance and Security Requirements
*If regulatory compliance or security certifications matter*
- Audit logging requirements
- Data residency constraints
- Compliance validation steps

## Migration from Existing Infrastructure
*If replacing an existing system*
- Current system capabilities
- Migration strategy (big bang vs gradual)
- Coexistence period
- Rollback plan

## Integration with Existing Tools
*If this needs to work with other systems*
- How this connects to issue tracking (Jira, GitHub Issues)
- How this integrates with monitoring (Datadog, New Relic)
- How this relates to deployment systems (Kubernetes, serverless)

## Training and Documentation Needs
*What the team needs to learn*
- Developer guide for triggering builds
- Operator guide for troubleshooting
- Architecture documentation for maintainers
- Runbook for common issues

FINAL STEP — Save the file

- Create or overwrite `PLAN.md` in the repo root
- After writing, confirm with:
  - "Wrote PLAN.md"
  - Brief summary of the infrastructure being built and key operational considerations
- Do not repeat the full file contents unless the user asks

TONE

- Practical and operations-focused
- Clear about technical decisions and trade-offs
- Realistic about costs, risks, and complexity
- Focused on reliability and maintainability
- Acknowledges that infrastructure serves the team

SPECIAL CONSIDERATIONS BY INFRASTRUCTURE TYPE

**For CI/CD pipelines:**
- Be explicit about trigger conditions and stage dependencies
- Include timing expectations for developer feedback
- Address parallel execution and caching strategies
- Consider cost of runner minutes

**For deployment automation:**
- Define environments clearly (dev, staging, production)
- Specify approval gates and manual interventions
- Include rollback procedures
- Address zero-downtime requirements

**For development tooling:**
- Focus on developer experience and setup time
- Document prerequisites and dependencies
- Include troubleshooting for common issues
- Consider cross-platform support

**For monitoring and observability:**
- Define key metrics and SLIs/SLOs
- Specify alerting thresholds and escalation
- Include dashboard designs
- Address data retention and costs

**For infrastructure as code:**
- Specify provisioning tools (Terraform, CloudFormation)
- Define resource organization and naming
- Include state management approach
- Address security and access control