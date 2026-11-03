# ZFT GM Copilot Development Handoff

## Repository

- Repository: `LuvacZantor/zft-gm-copilot`
- Stable branch: `main`
- Module ID: `zft-gm-copilot`
- Current state: repository foundation initialized; no runtime module code yet
- License: MIT

## Authoritative standards

Before development, read the current `main` branch of:

- `LuvacZantor/zft-standards/PROJECT-INSTRUCTIONS.md`
- `DEVELOPMENT.md`
- `VERSIONING.md`
- `ARCHITECTURE.md`
- `VALIDATION.md`
- `UI-STANDARDS.md`

Do not rely on historical project snapshots when current GitHub standards are available.

## Baseline

The repository was intentionally initialized without Foundry runtime code.

The stable baseline currently contains:

- `README.md`
- `LICENSE`
- `.gitignore`
- `docs/ARCHITECTURE.md`
- `docs/ROADMAP.md`
- `docs/DEVELOPMENT-HANDOFF.md`

There is no `module.json`, no scripts, no styles, no templates, no tests, and no released package yet.

## Initial product direction

ZFT GM Copilot is a GM-assistance module intended to connect Foundry VTT with AI capabilities for controlled context-aware assistance.

Initial feature direction:

- read current Scene context
- inspect controlled Tokens and relevant Actors
- inspect current Combat state
- inspect recent ChatMessages
- retrieve relevant Journals
- provide story, encounter, dialogue, recap, and diagnostic assistance
- expose only bounded module diagnostics
- support a provider/gateway abstraction
- preserve a future path for MCP-based external AI access

## Recorded long-term direction

The current product roadmap is in `docs/ROADMAP.md`.

It records the intended staged progression:

1. read-only GM Copilot
2. draft/staging mode for Foundry Documents and generated assets
3. approved Foundry publishing through narrow validated write tools
4. extended integrations such as private ChatGPT/MCP access, Kanka publishing, voice, and controlled server-side diagnostics

The roadmap also records the intended generated-asset pipeline:

```text
request -> generate -> stage -> record provenance -> preview -> approve -> publish -> bind
```

Planned provenance includes stable asset IDs, filenames, SHA-256 hashes, timestamps, provider/model metadata, approval metadata, target UUIDs where applicable, and module version information.

Visible branding/signatures may be optional. Provenance should be independent of visible branding.

## Initial safety boundary

The first runtime development build should be read-only with respect to Foundry world state.

Do not include AI-initiated document mutation, arbitrary macro execution, unrestricted JavaScript execution, filesystem access, or hidden privileged operations in the first baseline.

Future mutation support must distinguish read, draft/stage, and publish/mutate permissions and enforce authorization at the actual operation boundary.

## Expected first development series

The first runtime build is expected to begin at:

```text
14.0.0-dev.1
```

The target is Foundry V14. Do not set `verified` compatibility until the exact environment has been live-tested.

The initial runtime implementation should use ApplicationV2 for new UI and follow the architecture in `docs/ARCHITECTURE.md`.

## Next repository operation

The next logical GitHub operation is to create a development branch from this stable baseline and begin the `14.0.0-dev.1` implementation.

Creating or updating that remote branch requires fresh, explicit, task-specific owner authorization under ZFT development policy.

## GitHub state at this handoff

No tag or GitHub Release exists.

No Foundry package has been built or published.

No runtime behavior has been claimed as locally validated or live-verified.
