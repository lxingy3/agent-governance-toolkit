---
title: Rust agentmesh crate takes the ACS workspace dependency
last_reviewed: 2026-07-27
owner: liamcrumm
---

# Rust agentmesh crate takes the ACS workspace dependency

## Which Dependencies Changed And Why

No third-party crate was added or upgraded. `agent-governance-rust/Cargo.lock`
gains `agent_control_specification` because the `agentmesh` crate now evaluates
policy through the ACS engine instead of the vendored v4 path.

The dependency is a path entry pointing at `policy-engine/sdk/rust` inside this
repository, so it resolves to the workspace source rather than a registry
release. The lockfile was regenerated with `cargo generate-lockfile --offline`
and checked with `cargo metadata --offline --locked`, which fails if the
committed lockfile disagrees with the manifests.

## Security Advisory Relevance

No CVE or RustSec advisory is addressed. Nothing is pulled from crates.io that
was not already in the graph. The added crate is first-party and MIT licensed,
and it is the same engine the Python and TypeScript surfaces evaluate against,
so the three stop being able to disagree about a policy outcome.

## Breaking Change Risk Assessment

Low. A path dependency inside the workspace changes no published version
constraint for downstream crates, and `cargo check --offline --workspace`
compiles with the crate's four pre-existing warnings unchanged.

The user-visible breaking change is the removal of the v4 policy language
itself, which `BREAKING_CHANGES.md` covers.
