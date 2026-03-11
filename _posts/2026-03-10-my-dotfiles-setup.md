---
layout: post
title: "Inside My Dotfiles Repo: Profile-Aware, Idempotent, and Agent Ready"
permalink: my-dotfiles-repo/
date: 2026-03-10
categories: [engineering, tooling, dotfiles]
tags: [dotfiles, macos, mise, homebrew, automation, claude, codex]
excerpt: "How my dotfiles repository turns machine setup into a repeatable system with profiles, symlink convergence, runtime management, and Dev OS automation."
type_of_writing: "exploration"
---

A good dotfiles repo can be an operating model for your development environment.

Mine is built around a few practical goals:

- One command to bootstrap a machine
- Safe to re-run at any time (idempotent)
- Different profiles for different contexts (`work`, `personal`, `server`, `container`)
- Clear separation between declared state and machine drift
- Integrated agentic tooling (Claude/Codex hooks, cues, skills, governance)

## The High-Level Design

At the center is a simple pipeline:

1. Bootstrap machine dependencies (`git`, `gh`, `brew`, `mise`, auth helpers)
2. Install runtimes with `mise`
3. Install packages from layered Brewfiles
4. Converge `$HOME` to the repo’s `home/` tree via symlinks
5. Install profile-specific agentic/dev tooling and governance assets

This gives us a declarative source of truth with convergent behavior: run it once or run it ten times, and it moves the machine toward the same intended state.

## Profiles Drive Behavior

`DOTFILES_PROFILE` controls what gets installed and linked.

- `work`: full macOS setup + work identity/tooling
- `personal`: full macOS setup + personal identity/tooling
- `server`: minimal CLI-focused environment
- `container`: lightweight devcontainer-friendly setup

Profiles affect:

- Brew layers (`base`, `gui`, `dev`, `infra`, `creative`, etc.)
- Git identity includes
- SSH includes
- Which dotfiles get linked
- Claude/Codex settings merge behavior

## The Tools That Power It

- `mise`: runtime manager + task runner
- Homebrew + layered Brewfiles: package declaration by context
- `bin/link-dotfiles` + `bin/make-symlink`: idempotent symlink orchestration with backup behavior
- `dotfiles` CLI: operational commands (`doctor`, `link`, `update`, `lint`, `hooks`, `governance`)
- Pre-commit + ShellCheck + JSON/YAML checks: guardrails on changes
- BATS test suite: hook and script behavior coverage
- Governance + decision journal tooling: provenance, tradeoff capture, policy traceability

## Agentic/Dev OS Integration

This repo treats agentic config like first-class infra:

- Hook pipeline for telemetry and safeguards
- Cue system for context-aware prompt guidance
- Skill system for reusable workflows
- Governance checks to keep prompts and policies traceable
- Tradeoff capture utilities to document engineering decisions

Instead of “agentic settings in one laptop,” this makes agentic behavior portable, reviewable, and reproducible.

## Why This Model Works

The biggest win is predictability.

When your machine setup is code:

- onboarding is faster,
- drift becomes visible,
- changes are reviewable,
- and your personal workflows become team-shareable.

Dotfiles stop being a backup folder and become a platform.
