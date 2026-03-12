# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

This is an RFC specification document for **Agent Skills Discovery via Well-Known URIs** (currently v0.2.0, Draft status). It defines how organizations publish discoverable Agent Skills at `/.well-known/skills/` using RFC 8615. The primary artifact is `README.md`, which contains the full specification text.

There is no build system, package manager, test suite, or CI pipeline. This is a documentation-first repo.

## Branch Context

This branch (`pr6-updates`) is based on the `v0.2.0` branch from the `upstream` remote (https://github.com/cloudflare/agent-skills-discovery-rfc). The `v0.2.0` branch has not yet been merged into `upstream/main`. The upstream PR for the `v0.2.0` branch is https://github.com/cloudflare/agent-skills-discovery-rfc/pull/6.

## Related Projects

- **Agent Skills specification**: https://agentskills.io/ (source: https://github.com/agentskills/agentskills)
- **Upstream repo**: https://github.com/cloudflare/agent-skills-discovery-rfc

## Repository Structure

- `README.md` — The RFC specification itself. This is the main document.
- `examples/` — Reference implementations for generating `index.json` (the skills discovery index):
  - `skills-index.nextjs.ts` — Next.js route handler
  - `skills-index.astro.ts` — Astro API route
  - `skills-index.tanstack.ts` — TanStack Start API route
  - `skills-index.cgi` — Perl CGI script
- `LICENSE` — Apache 2.0

## Key Spec Concepts

- **`index.json`** at `/.well-known/skills/` enumerates all published skills with SHA-256 digests for integrity verification
- **Progressive disclosure**: 3-level loading (index metadata → SKILL.md → supporting files on demand)
- **Skill-level digest**: deterministic hash computed from sorted file entries using `{path}\0{hex}\n` manifest format, then SHA-256'd
- **Per-file digest**: `sha256:{64-char-hex}` format for individual file integrity
- The spec uses RFC 2119/8174 keyword conventions (MUST, SHOULD, etc.)

## Working on This Repo

When editing `README.md`, maintain consistency with RFC 2119 keyword casing conventions. The spec version is tracked in both the frontmatter metadata at the top and in the index format examples throughout.

When editing reference implementations in `examples/`, all four implementations must stay in sync — they implement the same algorithm (scan skill directories, parse YAML frontmatter from SKILL.md, compute SHA-256 digests, output v0.2.0 index JSON). Changes to the index format require updating all four examples.
