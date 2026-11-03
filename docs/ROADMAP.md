# ZFT GM Copilot Product Roadmap

## Purpose

This document records the current intended direction for ZFT GM Copilot before runtime development begins. It is planning guidance, not a claim that these capabilities already exist.

The long-term goal is a controlled GM copilot that can understand a live Foundry world, help create campaign content, generate and manage assets, and eventually perform approved Foundry changes through narrowly defined tools.

## Product model

The project should support two complementary interaction paths:

```text
Inside Foundry
Foundry -> ZFT GM Copilot -> trusted AI gateway/provider
```

and:

```text
Inside ChatGPT
ChatGPT -> private ZFT GM Copilot plugin / MCP connection -> controlled bridge -> Foundry
```

Neither path should expose unrestricted access to `game`, `canvas`, arbitrary JavaScript execution, or the host filesystem.

## Capability phases

### Phase 1 - Read-only GM Copilot

Initial runtime work should focus on understanding and assistance without changing world state.

Planned capabilities:

- inspect current Scene context
- inspect controlled or selected Tokens
- inspect relevant Actors
- inspect current Combat and combatants
- inspect recent ChatMessages
- retrieve relevant Journals
- expose bounded ZFT diagnostics
- summarize the current situation
- assist with story development, encounter continuation, NPC dialogue, clues, transitions, and recaps

The first implementation must remain read-only with respect to Foundry world Documents.

### Phase 2 - Draft and staging mode

The Copilot may prepare content and assets without publishing them directly into live world state.

Planned capabilities:

- draft Actors
- draft Items
- draft Journals and handouts
- draft RollTables or similar structured content
- generate token artwork
- generate Actor portraits
- generate handout and scene artwork
- stage generated files in an approved asset area
- show a preview and structured change summary before publication

Drafting and staging must be distinguishable from publishing.

### Phase 3 - Approved Foundry publishing

After the read-only and staging layers are proven, the Copilot may perform explicit write operations through validated tools.

Examples of future tools:

- `create_actor`
- `create_item`
- `update_actor`
- `create_token`
- `place_token_on_scene`
- `assign_actor_portrait`
- `assign_token_texture`
- `create_journal_entry`
- `attach_image_to_journal`
- `create_rolltable`
- `import_from_compendium`

These tools must be narrow, validated, auditable, and permission-aware.

High-impact operations should require user-visible approval before execution.

### Phase 4 - Extended integrations

Potential later integrations include:

- private ChatGPT plugin configuration
- MCP access from ChatGPT or another approved AI client
- Kanka publishing workflows
- gallery and campaign-art pipelines
- voice interaction
- controlled server-side diagnostics through a trusted sidecar
- additional storage or archival destinations
- reusable creation workflows and templates

Each external integration should remain optional unless deliberately made part of the advertised module purpose.

## Asset generation and publishing pipeline

Generated art should move through a staged workflow rather than being written directly into live content.

Preferred flow:

```text
request
  -> generate
  -> stage
  -> record provenance
  -> preview
  -> approve
  -> publish asset
  -> bind to Foundry Document
```

Possible asset categories include:

- Token artwork
- Actor portraits
- Item artwork
- scene art
- Journal handouts
- alternate-form artwork

The publish layer should use allowlisted destinations rather than unrestricted filesystem paths.

## Asset provenance and signing

Generated or transformed assets should carry useful provenance.

Preferred recorded metadata may include:

- asset ID
- filename
- SHA-256 hash
- generation timestamp
- generation provider and model
- prompt or prompt summary
- approving user
- target Foundry world/campaign
- target Foundry Document UUID where applicable
- GM Copilot module version
- workflow or source identifier

A sidecar manifest or structured asset record is preferred for auditability.

Visible branding or signatures may be supported as an optional presentation feature. Invisible provenance should remain independent of whether visible branding is used.

## Approval model

The module should distinguish at least three capability levels:

### Read

The Copilot can inspect explicitly exposed context and answer questions.

### Draft / stage

The Copilot can generate proposed Documents or assets but does not publish them into the live world.

### Publish / mutate

The Copilot can perform a defined Foundry write or asset publication only through an approved tool path.

Permission should be enforced at the actual operation boundary, not merely by hiding UI controls.

## Security boundaries

The design should avoid:

- long-lived OpenAI or other provider keys embedded in browser-side source
- secrets stored in ordinary client-visible Foundry settings
- unrestricted filesystem access
- unrestricted shell execution
- unrestricted JavaScript evaluation
- arbitrary macro execution by the AI
- silent world mutations
- broad delete or overwrite operations
- exposing large raw world Documents when a bounded summary is sufficient

Preferred controls include:

- trusted backend or gateway
- short-lived or scoped credentials where supported
- tool allowlists
- asset-path allowlists
- document-type allowlists
- rate and cost limits
- explicit approval for high-impact operations
- audit records
- rollback or targeted cleanup where practical

## ChatGPT and MCP direction

A private ZFT GM Copilot ChatGPT plugin is a planned external-client option, not a requirement for the first Foundry build.

Its purpose would be to let ChatGPT use explicit Foundry tools through MCP or another controlled bridge.

Read-only tools may include:

- current scene
- selected or controlled Tokens
- Actor lookup
- Combat state
- recent chat
- Journal search and retrieval
- GM context summary
- ZFT diagnostics

Future write tools should not be exposed until the module's permission, approval, and audit model has been validated.

## Initial implementation sequence

The first development series should favor proof of architecture over breadth.

Recommended progression:

1. establish the V14 module skeleton and ApplicationV2 UI
2. implement read-only context collectors
3. implement provider/gateway abstraction
4. implement conversation UI and bounded diagnostics
5. validate the read-only workflow live in Foundry
6. add draft/staging models
7. add image generation and asset provenance
8. add explicit approved Foundry write tools
9. add external ChatGPT/MCP integration
10. add extended publishing integrations such as Kanka only after the core trust model is stable

## Non-goals for the first build

The first development build should not include:

- autonomous GM behavior
- unrestricted AI control of the world
- arbitrary code execution
- server filesystem browsing
- silent Actor, Item, Token, Journal, Scene, or Combat mutation
- automatic publication of generated assets

## Status

All capabilities in this document are currently planned unless separately documented as implemented and validated.

The repository foundation exists on `main`. Runtime development has not yet begun.
