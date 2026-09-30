# ADR-0018: Project-local Claude Code setup

## Context

gatekit previously shipped as an installable Claude Code plugin. That required
users to install a marketplace entry before its commands, skills, and hooks
were available. The intended audience needs a repository that works when it is
opened, without a terminal-based installation step.

## Decision

This repository is configured as a single Claude Code project. Its hook
registrations live in `.claude/settings.json`, commands live in
`.claude/commands/gatekit/`, and skills live in `.claude/skills/gatekit-*/`.
The reusable Python kernel and its data remain together in
`.claude/gatekit-core/`; `.claude-plugin/plugin.json` remains beside the
`gatekit/` package solely as the runtime layout marker used by
`paths.plugin_root()`.

Project hooks address gate scripts through `${CLAUDE_PROJECT_DIR}`. This is the
official project-root placeholder for settings hooks and avoids reliance on
the plugin-only `${CLAUDE_PLUGIN_ROOT}` environment.

## Consequences

- Opening this repository in Claude Code activates its project-local commands,
  skills, and hooks without installing a marketplace plugin.
- The former marketplace/plugin loading path is no longer the enforcement
  mechanism for this checkout; `.claude/settings.json` is.
- The kernel preserves its self-locating fallback, so direct launcher and
  module use remain independent of a plugin-root environment variable.
