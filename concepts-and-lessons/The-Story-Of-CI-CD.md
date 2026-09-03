# 🔄 The Story of CI/CD — How Software Moves Through the World 🚀

CI/CD is usually introduced as a collection of tools and activities:

* pipelines
* builds
* automated tests
* artifacts
* deployments

But those are the visible parts of the machine.

To understand how they connect, begin with the deeper pattern:

```text
A program processes data.
A pipeline processes the program.
```

At runtime, software receives input, performs work, and produces output.

But software must first travel from a developer's machine into the world where users can reach it.

That journey also has inputs, transformations, outputs, boundaries, and failures.

```text
Code change
    ↓
Build
    ↓
Test
    ↓
Artifact
    ↓
Deployment
    ↓
Feedback
```

This is the deeper pattern behind CI/CD.

```text
CI/CD is I/O applied to the software itself.
```

Not as a literal technical definition.

As a systems-level compression.

The software is now the thing moving through the system.

---

# 🧠 1️⃣ The Level Shift — From Data Flow to Software Flow

In application I/O, data crosses the boundary of a running program:

```text
input → memory → processing → output
```

In CI/CD, a change crosses the boundaries of the software-delivery system:

```text
change → repository → pipeline → artifact → environment → feedback
```

The pattern is the same.

The level has changed.

| Application level | Delivery level |
|---|---|
| User request | Code change |
| Program | Pipeline |
| Processing | Build and test |
| Output data | Build artifact |
| External destination | Deployment environment |
| Error response | Failed pipeline or alert |

At the application level, the program transforms data.

At the delivery level, the pipeline transforms source code into trusted, deployable software.

---

# 🔤 2️⃣ What CI/CD Actually Means

CI means:

```text
Continuous Integration
```

Developers integrate small changes into a shared codebase frequently.

Each change is automatically checked.

Typical CI work includes:

* checking out the code 📥
* compiling it 🏗️
* running tests 🧪
* performing quality or security checks 🔍
* packaging the result 📦

CD has two related meanings.

## Continuous Delivery

```text
Every successful change is kept ready for release.
Production deployment may still require a human approval.
```

## Continuous Deployment

```text
Every successful change can move automatically into production.
```

So:

```text
CI = continuously prove that changes integrate safely
Delivery = keep proven software ready to release
Deployment = automatically release proven software
```

CI/CD is therefore not merely a tool.

It is a disciplined movement system.

---

# 🧍 3️⃣ Without CI/CD, the Human Becomes the Pipeline

Imagine deploying manually.

A developer:

1. pulls the latest code
2. remembers the correct commands
3. runs some tests
4. creates a JAR
5. copies it to a server
6. changes configuration
7. restarts the application
8. checks whether it worked

The process may succeed.

But the system depends on memory, timing, access, and perfect repetition.

```text
Manual delivery = a human carrying software across every boundary
```

This creates variation:

* different commands
* skipped tests
* missing files
* local-machine differences
* undocumented fixes
* inconsistent deployments

CI/CD turns remembered behaviour into executable behaviour.

```text
If a process must be repeated reliably,
encode it into the system.
```

---

# 📥 4️⃣ The Input — A Change Enters the Delivery System

A pipeline needs an input event.

Common inputs include:

* a commit pushed to a branch
* a pull request opened or updated
* a merge into `main`
* a version tag
* a manual release command
* a scheduled run

Example:

```text
Developer pushes commit
        ↓
GitHub receives change
        ↓
Workflow event fires
        ↓
Pipeline begins
```

The commit contains the new state.

The event announces that the state has changed.

```text
Commit = delivery input
Trigger = signal that starts processing
```

---

# 🗃️ 5️⃣ The Repository — Shared Source of Truth

The repository is where the team integrates its work.

It contains:

* source code
* tests
* dependency definitions
* configuration templates
* database migrations
* pipeline definitions
* version history

The repository is not merely storage.

It is the shared, versioned description of the system.

```text
Developer machine = private working state
Repository = shared source state
Running environment = deployed state
```

CI checks whether private work can safely become shared work.

CD moves trusted shared work toward running state.

---

# ⚙️ 6️⃣ The Runner — Temporary Memory and Processing Power

A workflow runs on a machine called a runner.

The runner may be:

* hosted by GitHub
* hosted by your organisation
* a Linux machine
* a Windows machine
* a macOS machine

The runner provides temporary compute and memory.

It checks out the repository, installs the required tools, and executes the pipeline steps.

```text
Repository state
        ↓
Runner
        ↓
Build and test processes
        ↓
Result
```

When the job ends, the temporary environment can disappear.

Anything that must survive needs to leave as an artifact, log, test report, deployment, or other recorded output.

The runner is the working memory of the pipeline.

---

# 🏗️ 7️⃣ The Build — Source Code Becoming Executable Form

Source code is written for humans and tools to understand.

It is not yet the final unit we deploy.

For a Java application, the build may:

* resolve Maven dependencies
* compile `.java` files
* process resources
* run tests
* package compiled code
* produce a `.jar` file

Flow:

```text
Java source
    +
dependencies
    +
resources
        ↓
Maven build
        ↓
tested JAR artifact
```

The build is a transformation.

```text
Source shape → deployable shape
```

This is why a pipeline can be understood as a program that processes a program.

---

# 🧪 8️⃣ Tests — Automated Questions at the Boundary

Every code change makes a claim:

```text
The system still works.
```

Tests challenge that claim.

Different tests ask different questions:

| Test | Question |
|---|---|
| Unit test | Does this small behaviour work? |
| Integration test | Do these parts work together? |
| API test | Does the boundary behave correctly? |
| End-to-end test | Does the user journey work? |
| Smoke test | Is the deployed system alive? |

In a pipeline, tests become gates.

```text
Tests pass → movement may continue
Tests fail → movement stops
```

The purpose is not to prove that failure is impossible.

The purpose is to detect failure earlier, closer to the change that caused it.

```text
Fast feedback reduces the distance between cause and discovery.
```

---

# 📦 9️⃣ The Artifact — Software Made Transferable

An artifact is the output produced by a build.

Examples:

* a Java `.jar`
* a Docker image
* a frontend bundle
* a library package
* a test report

For deployment, the artifact is the packaged form that can cross into another environment.

```text
Source code
    ↓
Build
    ↓
Artifact
    ↓
Deployment
```

This resembles serialization at a higher level:

```text
Internal working form → transferable form
```

The analogy is structural, not literal.

A strong delivery system builds once and promotes the same artifact through environments.

```text
Build once.
Test that build.
Move that build.
```

If each environment rebuilds the code independently, each environment may receive a different result.

---

# 🌍 1️⃣0️⃣ Environments — Different Worlds, Controlled Boundaries

Software usually moves through environments.

| Environment | Purpose |
|---|---|
| Local | Developer experimentation |
| CI | Automated build and verification |
| Test | Wider technical testing |
| Staging | Production-like validation |
| Production | Real users and real consequences |

Each environment is a boundary.

Crossing into a more important environment should require stronger evidence.

```text
Local
  ↓
CI
  ↓
Test
  ↓
Staging
  ↓
Production
```

The code should remain stable while environment-specific configuration changes around it.

```text
Same artifact.
Different configuration.
```

---

# 🔐 1️⃣1️⃣ Configuration and Secrets

Applications need values that should not live directly in source code:

* database passwords
* API keys
* cloud credentials
* signing keys
* production URLs

These values cross sensitive boundaries.

They should be supplied securely by the environment or secret store.

```text
Source code describes behaviour.
Configuration describes the environment.
Secrets grant access.
```

Never commit secrets into the repository.

A pipeline should receive only the permissions it needs for the task it is performing.

```text
Read code ≠ permission to deploy
Deploy to test ≠ permission to deploy to production
```

Boundaries should control authority as well as data.

---

# 🚀 1️⃣2️⃣ Deployment — The Output Changes the Running World

A build artifact is potential software.

Deployment makes that version active in an environment.

The exact mechanism depends on the platform:

* copying a JAR to a server
* publishing a Docker image
* updating an Azure App Service
* applying a Kubernetes deployment
* releasing static files to a web host

Conceptually:

```text
Trusted artifact
        ↓
Deployment mechanism
        ↓
Running environment changes state
```

Deployment is not simply moving a file.

It is a controlled state transition.

```text
Version A running
        ↓
deployment
        ↓
Version B running
```

That transition must be observable and recoverable.

---

# 👁️ 1️⃣3️⃣ Feedback — Output Returning as New Input

Deployment is not the end.

The running system produces signals:

* logs
* metrics
* traces
* health checks
* test results
* user reports
* alerts

These outputs return to the team as new inputs.

```text
Code change
    ↓
Pipeline
    ↓
Deployment
    ↓
Runtime behaviour
    ↓
Feedback
    ↓
Next decision or code change
```

This closes the loop.

```text
CI/CD without feedback is movement without sight.
```

The complete system is not a line.

It is a learning loop.

---

# ⚠️ 1️⃣4️⃣ Pipelines and Boundary Failure

Every boundary can fail.

* dependencies may be unavailable
* compilation may fail
* tests may fail
* credentials may be rejected
* an artifact may not publish
* a deployment may time out
* the new version may start but remain unhealthy

Professional pipelines make failure:

* visible
* attributable
* contained
* recoverable

```text
Hidden failure = corrupted confidence
Visible failure = actionable information
```

A red pipeline is not the pipeline failing at its purpose.

It may be the pipeline successfully preventing an unsafe change from moving further.

---

# ↩️ 1️⃣5️⃣ Recovery — Rollback, Roll Forward, Retry

Automation does not remove risk.

It makes risk manageable.

Common recovery strategies include:

* **retry** — repeat a failed transient operation
* **rollback** — restore the previous known-good version
* **roll forward** — deploy a corrective change
* **feature flag** — disable a feature without replacing the entire release

The right response depends on the failure.

```text
Temporary network failure → retry may help
Broken application version → rollback or roll forward
Unsafe feature behaviour → feature flag may contain it
```

The delivery system should preserve a known path back to safety.

---

# 🚦 1️⃣6️⃣ Gates — Evidence Before Movement

Not every successful compile deserves production access.

A pipeline may require:

* passing tests
* successful security scans
* code review
* branch protection rules
* an approved pull request
* environment approval
* successful health checks

A gate asks:

```text
Do we have enough evidence to cross the next boundary?
```

Good gates reduce risk.

Too few gates allow unsafe movement.

Too many slow, low-value gates create friction without creating confidence.

The goal is controlled flow.

---

# 🧾 1️⃣7️⃣ The Pipeline Definition — Process as Code

A CI/CD workflow can be stored in the repository as code.

Different tools use different files:

| Tool | Common pipeline file |
|---|---|
| Jenkins | `Jenkinsfile` |
| GitHub Actions | `.github/workflows/*.yml` |
| GitLab CI/CD | `.gitlab-ci.yml` |

This means the delivery process can be:

* versioned
* reviewed
* tested
* changed through pull requests
* shared by the team

```text
Application code describes the product.
Pipeline code describes how the product is trusted and moved.
```

The process no longer lives only in one person's memory.

It becomes part of the system.

---

# ⚙️ 1️⃣8️⃣ A First CI Pipeline — Before the Syntax

Before learning any particular automation tool, understand the job:

```text
Trigger: pull request or push
        ↓
Create a clean execution environment
        ↓
Check out repository
        ↓
Set up the correct Java version
        ↓
Run Maven build and tests
        ↓
Report success or failure
```

In pseudocode:

```text
WHEN code changes

    GET the shared code
    PREPARE Java
    RUN mvn verify

    IF build and tests pass
        REPORT success
    ELSE
        STOP and REPORT failure
```

Read it as a data journey.

```text
Code-change event enters
        ↓
Pipeline runner processes the repository
        ↓
Maven builds and tests the application
        ↓
Success or failure leaves as feedback
```

This is CI.

It verifies the change but does not deploy it.

CD would add the controlled movement of a trusted artifact into an environment.

Jenkins can execute this process from a `Jenkinsfile`.

---

# 🔄 1️⃣9️⃣ Pull Request Flow — CI in Team Motion

Imagine a developer changes the meal-suggestion service.

```text
Developer creates branch
        ↓
Developer pushes change
        ↓
Pull request opens
        ↓
CI builds application
        ↓
CI runs tests
        ↓
Team reviews code
        ↓
Change merges into main
```

The pull request is more than a conversation page.

It is an integration boundary.

Code review provides human evidence.

CI provides automated evidence.

Branch protection can require both before the boundary opens.

---

# 🍅 2️⃣0️⃣ Example — Fridge2Meal Delivery Flow

Suppose a student updates `MealService`.

The application-level flow is:

```text
JSON request
    ↓
MealRequest DTO
    ↓
MealService
    ↓
MealResponse DTO
    ↓
JSON response
```

The delivery-level flow is:

```text
MealService code change
        ↓
Git commit
        ↓
Pull request
        ↓
GitHub Actions runner
        ↓
Maven compile and tests
        ↓
JAR artifact
        ↓
Deployment environment
        ↓
Logs, metrics and user behaviour
```

These are two rings of the same system.

The inner ring moves data through the application.

The outer ring moves the application through the world.

---

# 🪆 2️⃣1️⃣ The Rings of the System

We can now see the architecture at several scales:

```text
Runtime ring
Data moves through the program

Application ring
Requests move through controllers, services and repositories

Delivery ring
Code moves through build, test and deployment

Operational ring
Feedback moves from production back to the team
```

Each ring has:

* inputs
* outputs
* state
* transformations
* boundaries
* failure modes
* feedback

The tools change.

The systems law remains.

---

# 🗺️ 2️⃣2️⃣ The Bigger Map

```text
Git records change.
GitHub shares and coordinates change.
Pull requests control integration.
CI builds and tests change.
Artifacts make software transferable.
CD moves trusted software through environments.
Secrets control access across boundaries.
Deployment changes running state.
Observability returns runtime output as feedback.
```

These are not disconnected DevOps topics.

They form one software-delivery system.

---

# 🚀 Final Compression

```text
Commit = input change
Repository = shared source of truth
Trigger = signal that begins processing
Runner = temporary execution environment
Build = source-to-artifact transformation
Test = automated evidence
Artifact = transferable software package
Environment = destination boundary
Gate = rule controlling movement
Deployment = controlled runtime state change
Observability = output from the running system
Feedback = output returning as new input
CI = continuously integrate and verify changes
Continuous Delivery = keep changes ready to release
Continuous Deployment = release successful changes automatically
```

---

# 🧠 Ultimate Compression

```text
A program processes data.
A pipeline processes the program.

I/O moves data across application boundaries.
CI/CD moves software across delivery boundaries.

CI asks: can this change safely join the system?
CD asks: can this trusted version safely enter the world?

Deployment produces reality.
Feedback reveals the result.
The next change begins the loop again.
```

You are not just learning how to automate a build.

You are learning how software moves through the world.
