# Grafana MCP server

Inspect a dashboard and record an approved release annotation. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `GRAFANA_TOKEN`
- `GRAFANA_HOST`



Credential help: https://grafana.com/docs/grafana/latest/developer-resources/api-reference/http-api/api-legacy/folder_dashboard_search/

## Supported scope

- Requires a customer-owned public HTTPS Grafana instance exposing the documented legacy HTTP API. No localhost/private-IP endpoints.
- Dashboard GET binds the Grafana12 legacy API; Grafana13 deprecates /api routes while keeping them accessible. New /apis dashboard resources are outside this toolset.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call search_dashboards '{"query":"production","limit":10}'
```

## Use with an MCP client

Launch `node /absolute/path/grafana-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| search_dashboards | Read | Search dashboards. |
| get_dashboard | Read | Get dashboard. |
| create_annotation | Explicit approval required | Create annotation. |

## Concrete workflow

Inspect a dashboard and record an approved release annotation. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

1. search_dashboards
2. get_dashboard
3. create_annotation

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-grafana-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/grafana/).

## Provenance and maintenance

- https://grafana.com/docs/grafana/latest/developer-resources/api-reference/http-api/api-legacy/folder_dashboard_search/
- https://grafana.com/docs/grafana/v12.0/developers/http_api/dashboard/
- https://grafana.com/docs/grafana/latest/developer-resources/api-reference/http-api/api-legacy/annotations/

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
