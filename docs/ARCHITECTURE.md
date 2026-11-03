# ZFT GM Copilot Architecture

## Status

This document defines the initial architectural direction for ZFT GM Copilot before runtime implementation begins.

Current state: repository foundation only. No Foundry runtime code has been released or live-verified.

## Primary target

Initial development targets:

- Foundry VTT V14
- ApplicationV2 for new user-facing UI
- ZFT version series beginning with `14.0.0-dev.1`
- GM-first workflow for initial development
- system-agnostic core where practical
- D&D5e-specific context adapters only where they provide clear value

Do not advertise compatibility that has not been live-tested.

## Core design goals

ZFT GM Copilot is intended to assist a GM without becoming an autonomous game authority.

The architecture should support:

1. read-only understanding of current Foundry context
2. bounded retrieval of relevant world data
3. story, encounter, dialogue, recap, and diagnostic assistance
4. optional image-generation workflows
5. optional external AI access through a controlled MCP-style interface
6. later Foundry mutations only through explicit, auditable authorization gates

## Initial trust boundary

The first implementation must be read-only with respect to Foundry world state.

The AI may inspect approved context but must not autonomously:

- create, update, or delete Foundry Documents
- change combat state
- move or alter Tokens
- modify Actors or Items
- write Journals
- change settings
- alter files
- execute arbitrary macros or JavaScript

Mutation support, if added later, must use narrowly defined operations, permission validation, user-visible intent, and explicit approval.

## Separation of concerns

Preferred high-level structure:

```text
Foundry UI / Documents / Hooks
            |
    Foundry Adapter Layer
            |
  Context & Tool Services
            |
 Conversation / AI Layer
            |
 Provider / Gateway Boundary
```

### Foundry adapter layer

Responsible for:

- resolving Scene, Actor, Token, Journal, Combat, and ChatMessage data
- validating Foundry Documents and UUIDs
- handling permissions and current client role
- translating Foundry data into bounded serializable context

### Context services

Each context collector should be narrow and independently testable.

Planned collectors include:

- current Scene
- controlled or selected Tokens
- referenced Actors
- current Combat and combatants
- recent ChatMessages
- relevant Journals
- bounded module diagnostics

Collectors should return structured summaries rather than unrestricted copies of entire world Documents unless explicitly requested and safe.

### Conversation / AI layer

Responsible for:

- maintaining conversation state
- constructing context packets
- selecting permitted tools
- enforcing size and retrieval limits
- returning assistant output to the Foundry UI
- keeping provider-specific behavior outside Foundry adapters

### Provider / gateway boundary

The Foundry client must not require a long-lived OpenAI or other provider secret embedded in browser-side source or world settings.

Preferred designs should allow:

- trusted backend/gateway API calls
- short-lived or scoped credentials where a provider supports them
- provider abstraction for future interoperability
- configurable rate, token, and cost limits

## MCP direction

MCP support is a planned architectural capability, not the sole runtime model.

The project should be able to evolve toward both:

```text
Foundry -> AI provider/gateway
```

and:

```text
External AI client -> controlled MCP bridge -> Foundry
```

External access must expose explicit tools and resources, not arbitrary access to `game`, `canvas`, the filesystem, or unrestricted JavaScript execution.

## Diagnostics

ZFT diagnostics begin with `[ZFT]`.

The module may maintain a bounded in-memory diagnostic buffer containing its own useful runtime events. Do not collect or expose credentials, secrets, large world Documents, personal data, or arbitrary server filesystem logs.

Access to server-side Foundry log files would require a separately designed trusted backend or sidecar and is outside the initial browser-module boundary.

## UI direction

The initial interface should follow current ZFT UI standards:

- ApplicationV2
- scoped styles only
- dark-plum / emerald ZFT baseline for new UI
- clear context and permission indicators
- explicit action boundaries
- no hidden gameplay mutations

## Persistence

Persist only what the feature genuinely requires.

Possible future persisted state may include:

- provider/gateway configuration metadata
- client UI preferences
- conversation metadata
- user-approved saved prompts or workflows

Secrets must not be stored in ordinary client-visible Foundry settings.

Any future schema must be namespaced and versioned.

## Validation requirements

Before any build is called live-verified, record:

- Foundry core version/build
- game system and version
- GM/player role
- configured provider/gateway
- exact test steps
- expected and observed results
- regression results
- rollback path

Static validation and simulated tests remain locally-validated only.
