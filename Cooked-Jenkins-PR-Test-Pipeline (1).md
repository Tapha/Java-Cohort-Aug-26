# ⚙️ Cooked Task — Jenkins PR Test Pipeline 🧪🐳

## Context

Cooked already contains JUnit tests that provide executable proof of backend behaviour.

Your next task is to automate that proof with Jenkins.

Jenkins must run through a Docker container so that the CI environment is repeatable across the team.

```text
JUnit defines the proof.

Jenkins repeats the proof.

Docker stabilizes the environment performing the proof.
```

---

## 1️⃣ Preparation

Before beginning the implementation:

1. Read `The-Story-of-JUnit.md`.
2. Re-read `The-Story-of-Jenkins.md`.
3. Inspect the current Cooked repository structure.
4. Identify where the backend build file, Maven wrapper and JUnit tests live.
5. Confirm how the existing Docker configuration works before changing it.

Do not begin by copying a generic pipeline. Derive the pipeline from the structure and commands used by this repository.

---

## 2️⃣ Task

Branch from the latest version of `main`.

Create a containerized Jenkins setup and a Jenkins pipeline that automatically runs the Cooked backend test suite whenever code is pushed to an open pull-request branch.

The intended flow is:

```text
Branch from main
        ↓
Implement the Jenkins pipeline
        ↓
Open a pull request into main
        ↓
Push or update the PR branch
        ↓
Containerized Jenkins detects the change
        ↓
Jenkins runs the JUnit tests
        ↓
The PR receives a clear pass/fail result
```

The source branch is not `main`. The pull request should target `main`.

---

## 3️⃣ Required Outcomes

Your solution must provide:

- A Jenkins instance that runs through Docker.
- Persistent Jenkins state across normal container restarts.
- A pipeline definition stored inside the Cooked repository.
- Automatic discovery of open pull requests.
- Automatic execution when a new commit is pushed to the PR branch.
- Execution of the repository’s existing backend JUnit tests.
- A failed pipeline when compilation or any test fails.
- A successful pipeline only when the test command succeeds.
- Test results that can be inspected from Jenkins.
- A setup another team member can reproduce from the repository documentation.

---

## 4️⃣ Design Questions

Your implementation should answer these questions through its structure:

- Which Docker image should Jenkins use?
- Which Java version must exist inside the Jenkins environment?
- Which Jenkins plugins are required?
- How will Jenkins configuration survive container recreation?
- How will Jenkins access the Cooked repository?
- How will it discover pull requests rather than only normal branches?
- How will it detect further pushes to an already-open PR?
- Which directory should the test command run from?
- Should the pipeline use the project’s Maven wrapper or a global Maven installation?
- Where does Maven write its JUnit reports?
- How will Jenkins publish those reports?
- How will secrets or repository credentials be kept out of source control?

Where multiple solutions exist, choose one and explain why it fits this project.

---

## 5️⃣ Constraints

- Begin from the latest `main`.
- Work on a dedicated feature branch.
- Run Jenkins through Docker rather than installing it directly on the machine.
- Use the build and test tooling already defined by Cooked.
- Keep the pipeline focused on backend tests.
- Do not merge directly into `main`.
- Do not hard-code passwords, tokens or private keys.
- Do not remove, skip or weaken tests to obtain a successful build.
- Do not add deployment to this pipeline.

---

## 6️⃣ Acceptance Criteria

### Containerized Jenkins

- Jenkins can be started through the project’s documented Docker workflow.
- The Jenkins UI becomes accessible from the host machine.
- Required pipeline, source-control and test-report capabilities are available.
- Existing Jenkins jobs and configuration remain after a normal restart.

### Pull-Request Trigger

- Jenkins can discover an open Cooked pull request.
- The pipeline runs when the PR is opened or first discovered.
- Pushing another commit to the same PR branch causes another run without manually starting the build.
- The pipeline is limited to the intended PR workflow and does not deploy anything.

### Test Execution

- Jenkins checks out the correct pull-request revision.
- Jenkins runs the backend test command from the correct location.
- Existing JUnit tests execute inside the Jenkins environment.
- Jenkins exposes the resulting test report.

### Pass/Fail Behaviour

- A passing test suite produces a successful pipeline.
- A deliberately introduced test failure produces a failed pipeline.
- Correcting that failure and pushing again produces a new successful run.

### Reproducibility

- A teammate can clone the repository, follow the submitted instructions and reproduce the Jenkins environment.
- Repository-specific assumptions are documented.
- No undocumented local dependency is required.

---

## 7️⃣ Required Evidence

Add the following evidence to the pull request:

- The branch used for the work.
- The files added or changed.
- Jenkins running through Docker.
- The discovered pull-request job.
- A successful automated test run.
- A failed automated test run caused by a controlled test failure.
- A later successful run after the failure was corrected.
- Confirmation that a second push retriggered the pipeline automatically.
- A short explanation of the chosen trigger mechanism.
- Clear setup and teardown instructions for another developer.

Screenshots must show the relevant job, stage and result—not only the Jenkins dashboard.

---

## 8️⃣ Pull Request

Open the completed work as a pull request into `main`.

The PR description should explain:

- What was implemented.
- How Docker is being used.
- How Jenkins detects PR changes.
- How JUnit tests are executed and reported.
- How the failure path was verified.
- Any limitations or environment assumptions.

Do not merge the pull request. It will be reviewed alongside the other implementations, and the strongest solution will be selected.

---

## ✅ Definition of Done

The task is complete when:

```text
Jenkins is reproducible through Docker.

An open PR is detected automatically.

Every new push causes the JUnit proof to run again.

Failure blocks confidence.

Success produces visible evidence.
```

You are not being assessed only on whether Jenkins turns green.

You are being assessed on whether you can build and explain the mechanism that makes green meaningful.
