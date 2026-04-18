# System Snapshot Checklist

Use this reference when the user asks to understand an unfamiliar system or when you need a quick but reliable map before editing.

## Goal

Produce a compact, evidence-backed snapshot of:

- how the system starts
- how requests or events flow through it
- where state lives
- which modules are risky to touch
- how to verify a future change

## Read Order

Inspect sources in this order unless the repo suggests a better sequence:

1. `AGENTS.md`, `README*`, architecture notes, onboarding docs
2. manifest and toolchain files such as `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`
3. app entry points such as `main.*`, `index.*`, router/bootstrap files, worker entry files
4. build and runtime config such as `vite.config.*`, webpack config, Dockerfiles, env loaders, CI scripts
5. core domain modules, service layers, stores, reducers, data models
6. tests, fixtures, replay logs, seed data, and scripts that encode invariants
7. recent commits touching the same subsystem when the current behavior is still unclear

## What To Extract

Record these items explicitly:

- startup chain: what runs first, second, and third
- ownership map: which module owns truth for each important state
- data flow: inputs, transformations, outputs, and side effects
- async boundaries: workers, timers, queues, subscriptions, sockets
- persistence and IO: files, DB, cache, local storage, network APIs
- config boundaries: environment variables, flags, profiles, mode switches
- verification surfaces: tests, scripts, checks, logs, dashboards, replays
- uncertainty list: places where behavior is inferred instead of directly verified

## Fast Discovery Patterns

Prefer fast text search over browsing file-by-file. Useful search targets:

- `main`, `bootstrap`, `createApp`, `render`, `router`, `worker`
- `listen`, `emit`, `dispatch`, `subscribe`, `postMessage`, `onmessage`
- `fetch`, `axios`, `ws`, `socket`, `request`
- `store`, `state`, `reducer`, `use*Store`, `context`
- `config`, `env`, `featureFlag`, `mode`
- `test`, `spec`, `fixture`, `replay`, `record`, `snapshot`

## Good Snapshot Output

Prefer this shape:

### Current State

- `Entry path`: where the system boots and the main loop begins
- `Critical modules`: the 3-7 files or directories that actually matter
- `Data path`: how an input becomes a state change or output
- `High-risk seams`: concurrency, caches, workers, retries, handoffs, derived state
- `Verification hooks`: the fastest reliable checks to use before and after edits

### Unknowns

- call out missing ownership, weak tests, unclear runtime assumptions, and docs/code mismatches

## Anti-Patterns

- Do not summarize every folder equally. Weight by execution importance.
- Do not assume the README is current.
- Do not treat test names as proof of behavior without reading at least the critical assertions.
- Do not propose broad refactors before you can describe the current execution path cleanly.
