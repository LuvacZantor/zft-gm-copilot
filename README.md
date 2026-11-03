# ZFT GM Copilot

**ZFT GM Copilot** is a planned Foundry Virtual Tabletop module for AI-assisted Game Master workflows.

The project is intended to provide a controlled bridge between Foundry VTT and AI services for tasks such as:

- understanding current scene, Actor, Token, combat, chat, and Journal context
- assisting with story development, encounter continuation, NPC dialogue, and session recaps
- exposing bounded diagnostics for troubleshooting
- supporting future image-generation workflows
- supporting future external AI access through a controlled MCP-style interface
- performing Foundry mutations only behind explicit permission and authorization boundaries

## Project status

Repository initialized. No runtime module implementation has been released yet.

The first implementation series is expected to target:

- Foundry VTT V14
- ZFT version series beginning at `14.0.0-dev.1`
- ApplicationV2 for new module UI
- read-only AI context access before any AI-initiated Foundry mutations
- provider/gateway abstraction so API credentials are not embedded directly in browser-side module code

Development will occur on an explicitly authorized development branch. `main` remains the stable repository baseline.

## Architecture direction

The planned module separates Foundry-facing adapters from AI/provider logic and keeps access to game state intentionally scoped.

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Product roadmap

The planned progression from read-only context access through drafting/staging, approved Foundry publishing, generated-asset provenance, private ChatGPT/MCP access, and future external integrations is recorded in [docs/ROADMAP.md](docs/ROADMAP.md).

These are planned capabilities, not claims of current implementation.

## Development handoff

Current repository state and the next expected implementation step are tracked in [docs/DEVELOPMENT-HANDOFF.md](docs/DEVELOPMENT-HANDOFF.md).

## Standards

This repository follows the current Zantor's Realms Foundry Tools standards maintained in:

- `LuvacZantor/zft-standards`
- `PROJECT-INSTRUCTIONS.md`
- `DEVELOPMENT.md`
- `VERSIONING.md`
- `ARCHITECTURE.md`
- `VALIDATION.md`
- `UI-STANDARDS.md`

## License

The source code and documentation in this repository are licensed under the [MIT License](LICENSE) unless a file or third-party component states otherwise.

Third-party trademarks, copyrighted campaign material, artwork, generated assets, API credentials, and user content are not granted rights merely by being used with this project.
