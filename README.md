# Starlight Command Center — Public Template

This repository contains the open-core template for the Starlight Command Center. It provides the structured operational logging, ingestion flows, cockpit UI views, and routing prompts for running multi-agent, multi-repository workspaces.

## Repository Contents

- `command-center.html`: Static, standalone cockpit interface reading `state.json` (falls back to embedded sample data).
- `command-center-live.html`: Live cockpit interface with real-time Linear integration (using an MCP connector) and Chart.js dashboards.
- `evals/RUBRIC.md`: Standardized agent evaluation rubric.
- `prompts/`: Standard session start and handover templates.
- `commands/handover.md`: Standard portable `/handover` prompt.
- `state.sample.json`: Sample structured session registry.
- `ecosystem.sample.json`: Sample ecosystem architecture roadmap.

## Live Integration Setup (Linear)

The live dashboard (`command-center-live.html`) integrates with your team's Linear workspace via a model context protocol (MCP) connector.

To set up live synchronization:
1. In `command-center-live.html`, find the line defining the `LIN` connector prefix:
   ```javascript
   const LIN = "mcp__<CONNECTOR_ID>__";
   ```
2. Replace `<CONNECTOR_ID>` with your specific Linear MCP connector prefix ID (e.g., `mcp__89363126-5dff-4149-afcc-8c3804dffa60__` or the exact name of your Linear integration tools).
3. If Linear is unavailable or not connected, the live cockpit degrades gracefully and displays a connection notice while fallback local session records are rendered.

## License

This project is licensed under the MIT License.
