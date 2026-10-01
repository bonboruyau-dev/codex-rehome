# ADR: Safe one-click package selection

**Status:** Accepted

## Context

ReHome already supports selecting all projects, conversations, Skills, Plugins, and generated images. A registered project can, however, be the user's entire home directory. Because ReHome's private staging directory and Codex home live below that directory, selecting it causes a safety rejection and risks scanning unrelated or credential-bearing files.

## Decision

Add a one-click package action that selects every migratable conversation and optional Codex content, while excluding project-file roots that contain the active Codex home. Conversations associated with an excluded project remain selected. The manual selection UI uses the same safety rule and explains every skipped project.

The existing backend overlap validation remains authoritative. The UI rule prevents a known-invalid request; it does not weaken package validation or credential exclusions.

## Alternatives considered

- Move staging under the package output directory: rejected because users can save packages inside a selected project, and packaging an entire user home remains unsafe.
- Add the staging directory to exclusions: rejected because it still scans an over-broad source and makes correctness depend on path-specific exceptions.
- Remove the broad project registration: rejected because export must not mutate Codex project state.

## Consequences

- Normal projects, all conversations, Skills, Plugins, and generated images can be packaged from one action.
- A home-directory project exports conversations but not the entire home directory as project files.
- Users who need files below a broad root must register/select a narrower project directory.
