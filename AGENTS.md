# Repository Instructions for AI Coding Agents

Apply these instructions whenever you inspect, write, modify, test, or review code in this repository. Follow the existing project conventions and configured runtime; do not introduce changes outside the task's scope.

## Core principles

- Prefer simple, focused, maintainable solutions.
- Avoid over-engineering, enterprise abstractions, unnecessary interfaces, excessive builder patterns, and unnecessary DTO layers.
- Keep APIs minimal, especially for MVP work.
- Prefer explicit, purpose-specific methods over generic `execute` methods.
- Keep classes and functions small and cohesive; avoid god classes.
- Prefer composition and delegation over deep inheritance.
- Do not invent requirements or functionality.
- Do not add dependencies, layers, files, or architectural patterns without a concrete need.
- Do not create README files, Markdown documentation, Docker files, Kubernetes manifests, or CI/CD configuration unless explicitly requested by the user or required for the task.

## Java

- Use Java 8-compatible syntax and APIs by default, unless the repository's configured Java version explicitly supports newer features.
- Follow the version configured by the build files and existing code.
- Use lambdas and functional interfaces where they improve clarity.
- Prefer streams for transformations when clearer than loops; do not force streams where a loop is simpler.
- Prefer interfaces over abstract classes when an interface is the right contract.
- Prefer composition over inheritance.
- Prefer immutable models and value objects by default.
- Favor pure functions and avoid side effects where practical.
- Keep APIs small and single-purpose.
- Avoid reflection, unnecessary mutable state, static utility classes, and generic abstractions unless justified.
- Use `Optional` only when it meaningfully expresses an optional API result or contract. Do not use it indiscriminately or as a blanket replacement for nullable values.
- Use records (Java 14+) and sealed classes only when supported by the configured Java version and appropriate to the design.
- Do not introduce Spring unless explicitly requested or already part of the project. When Spring is used, keep framework-specific code at the appropriate application/adapter/infrastructure boundaries.

## Architecture and boundaries

Keep business logic independent of frameworks and infrastructure.

- **Domain:** Owns business rules and domain behavior. Keep it framework-independent; use focused models and immutable value objects where appropriate.
- **Application/use cases:** Orchestrate domain behavior to fulfill use cases. Keep use cases focused and free of unnecessary framework decisions.
- **Adapters/controllers/handlers:** Translate external input and output formats and call application use cases. Controllers must not call repositories directly. Keep business logic out of controllers and handlers.
- **Infrastructure:** Database, cache, messaging, and external API adapters implement appropriate repository/gateway contracts and remain replaceable. Keep adapters thin.
- Dependencies point inward toward the domain. Domain code must not depend on adapters or frameworks.
- Avoid layer skipping, circular dependencies, deep inheritance trees, and god classes.
- Keep transport concerns separate from business logic and resource interfaces.
- Keep persistence models separate from domain models when doing so prevents meaningful coupling.
- Use dependency injection where appropriate.
- Prefer small composable modules and behavior-oriented interfaces. Add an interface or layer only when it provides a concrete benefit.

## API and model design

- Keep APIs minimal and models focused.
- Prefer explicit methods with clear intent over generic execution methods.
- Introduce DTOs only when they serve a boundary, contract, validation, serialization, or compatibility purpose.
- Avoid massive resource interfaces, unnecessary DTO layers, and enterprise abstractions that add indirection without solving a real problem.
- Use `Optional` intentionally, not universally.

## Security

- Never hardcode secrets, credentials, tokens, or private keys.
- Never expose credentials or sensitive data in logs or error responses.
- Validate and constrain untrusted input.
- Use parameterized queries for database access.
- Prevent command and shell injection.
- Avoid shell execution where possible. When execution is necessary, prefer safe process APIs and pass arguments separately rather than concatenating untrusted input into command strings.
- Sanitize and validate command arguments.
- Avoid insecure temporary-file handling.
- Treat credentials, tokens, private keys, `.env` files, and other secret-bearing files as sensitive. Do not inspect them unless the task explicitly requires it and the user has approved.

## Testing

- For Java code, use JUnit 5.
- Prefer focused, mock-based unit tests.
- Avoid integration tests unless explicitly requested or necessary to satisfy the task.
- Keep tests isolated and proportional to the behavior being tested.
- Avoid excessive test scaffolding and prefer readable assertions.
- Do not claim that tests passed unless they were actually run and their results support that claim.

## Comments and documentation

- Do not generate comments by default. Comments should be rare.
- Add a comment only when all three conditions apply:
  1. The behavior or constraint cannot reasonably be expressed through clear code, naming, or structure.
  2. It matters for correctness, security, compatibility, data integrity, or another critical constraint.
  3. Without it, a future developer could reasonably misunderstand the behavior and introduce a bug.
- When needed, explain the reason, invariant, or non-obvious constraint concisely, durably, and next to the relevant code.
- Do not comment on obvious behavior, syntax, control flow, naming, or self-explanatory code.
- Do not add comments about task history, tickets, conversations, user requests, specifications as sources, implementation status, testing status, TODOs/FIXMEs, or what the next line does.
- Never write comments such as “As requested,” “As discussed,” “Per the requirements,” “The user requested,” “This was changed because,” or “Tested and working.”
- Do not create Markdown files or README files unless explicitly requested. These agent instruction files are an explicit exception because they are requested.

## Git commit messages

When asked to create a commit message:

- Use Conventional Commits with a type such as `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `perf`, `build`, or `ci`.
- Format the subject as `<type>: <concise subject>`.
- Keep the subject preferably under 72 characters, start the subject with a lowercase word, use imperative present-tense wording, and do not end it with a period.
- Focus on the final behavior or capability, not implementation history.
- If a body is useful, use 2–4 short bullets and no more than 3–4 bullets total.
- Group related changes into one logical commit when they implement one cohesive capability, even across layers or modules.
- Describe only the final intended state if later changes replace or correct earlier work.
- Do not list files, test results, debugging details, intermediate implementation history, unnecessary issue references, or commit hashes unless explicitly requested.
- Do not invent functionality.
- Never add Git trailers such as `Co-authored-by`, `Signed-off-by`, or `Reviewed-by`.
- If asked for only a commit message, output only the message without explanation, code fences, or alternatives.
- When handling multiple commits, preserve their order and do not invent commits.

## Terminal and change safety

- Prefer safe, read-only commands without repeatedly asking for permission.
- Safe commands include listing/searching/reading files, inspecting project structure/configuration, read-only Git inspection (`git status`, `git diff`, `git log`, `git show`, `git branch`, `git remote -v`), running tests/linters/type checks/builds, and checking runtime versions.
- Ask for confirmation before deleting files; modifying files outside the task's normal scope; changing system settings; installing/upgrading/uninstalling packages; starting/stopping/modifying system services or processes; making external network requests or downloading files when not explicitly required; accessing secrets; changing permissions/ownership; deployments/production operations; or consequential Git operations.
- Consequential Git operations requiring confirmation include `git commit`, `git reset`, `git rebase`, `git merge`, `git cherry-pick`, working-tree-changing `git checkout`, `git clean`, force-push, and history rewriting.
- Approval for a safe command does not authorize a destructive or sensitive command. When uncertain, ask before proceeding.
- **Never run `git push` or `git pull` under any circumstances.** This is an absolute restriction and cannot be overridden by a general workflow instruction.
