---
name: figma
description: Use the Figma MCP server to fetch design context, screenshots, variables, and assets from Figma, and to translate Figma nodes into production code. Trigger when a task involves Figma URLs, node IDs, design-to-code implementation, or Figma MCP setup and troubleshooting.
author: openai
---

# Figma pro

## Figma MCP

Use the Figma MCP server for Figma-driven implementation. For setup and debugging details (env vars, config, verification), see `references/figma-mcp-config.md`.

### Figma MCP Integration Rules

These rules define how to translate Figma inputs into code for this project and must be followed for every Figma-driven change.

#### Required flow (do not skip)

1. Run get_design_context first to fetch the structured representation for the exact node(s).
2. If the response is too large or truncated, run get_metadata to get the high-level node map and then re-fetch only the required node(s) with get_design_context.
3. Run get_screenshot for a visual reference of the node variant being implemented.
4. Only after you have both get_design_context and get_screenshot, download any assets needed and start implementation.
5. Translate the output (usually React + Tailwind) into this project's conventions, styles and framework. Reuse the project's color tokens, components, and typography wherever possible.
6. Validate against Figma for 1:1 look and behavior before marking complete.

#### Implementation rules

- Treat the Figma MCP output (React + Tailwind) as a representation of design and behavior, not as final code style.
- Replace Tailwind utility classes with the project's preferred utilities/design-system tokens when applicable.
- Reuse existing components (e.g., buttons, inputs, typography, icon wrappers) instead of duplicating functionality.
- Use the project's color system, typography scale, and spacing tokens consistently.
- Respect existing routing, state management, and data-fetch patterns already adopted in the repo.
- Strive for 1:1 visual parity with the Figma design. When conflicts arise, prefer design-system tokens and adjust spacing or sizes minimally to match visuals.
- Validate the final UI against the Figma screenshot for both look and behavior.

#### Asset handling

- The Figma MCP Server provides an assets endpoint which can serve image and SVG assets.
- IMPORTANT: If the Figma MCP Server returns a localhost source for an image or an SVG, use that image or SVG source directly.
- IMPORTANT: DO NOT import/add new icon packages, all the assets should be in the Figma payload.
- IMPORTANT: do NOT use or create placeholders if a localhost source is provided.

#### Link-based prompting

- The server is link-based: copy the Figma frame/layer link and give that URL to the MCP client when asking for implementation help.
- The client cannot browse the URL but extracts the node ID from the link; always ensure the link points to the exact node/variant you want.

### Figma MCP config reference

Use this snippet to register the Figma MCP server in `~/.codex/config.toml` as a streamable HTTP server with bearer auth pulled from your env.

```toml
[mcp_servers.figma]
url = "https://mcp.figma.com/mcp"
bearer_token_env_var = "FIGMA_OAUTH_TOKEN"
http_headers = { "X-Figma-Region" = "us-east-1" }
```

#### Notes and options

- The bearer token must be available as `FIGMA_OAUTH_TOKEN` in the environment that launches Codex.
- Keep the region header aligned with your Figma region. If your org uses another region, update `X-Figma-Region` consistently.
- OAuth on streamable HTTP requires the RMCP client: set `[features].rmcp_client = true` (or `experimental_use_rmcp_client = true` on older builds) at the top level of `config.toml`.
- Optional per-server timeouts: `startup_timeout_sec` (default 10) and `tool_timeout_sec` (default 60) can be set inside `[mcp_servers.figma]` if needed.

#### Env var setup (if missing)

- One-time set for current shell: `export FIGMA_OAUTH_TOKEN="<token>"`
- Persist for future sessions: add the export line to your shell profile (e.g., `~/.zshrc` or `~/.bashrc`), then restart the shell or your IDE.
- Verify before launching Codex: `echo $FIGMA_OAUTH_TOKEN` should print a non-empty token.

#### Setup + verification checklist

- Add the snippet above to `~/.codex/config.toml` under `[mcp_servers.figma]`, and enable `[features].rmcp_client = true` (or `experimental_use_rmcp_client = true` on older releases).
- Restart Codex (CLI/IDE) after updating config and env vars.
- Ask Codex to list Figma tools or run a simple call to confirm the server is reachable.

#### Troubleshooting

- Token not picked up: Export `FIGMA_OAUTH_TOKEN` in the same shell that launches Codex, or add it to your shell profile and restart.
- OAuth errors: Verify `rmcp_client` is enabled and the bearer token is valid. Tokens copied from Figma should not include surrounding quotes.
- Network/headers: Keep the `X-Figma-Region` header; if your org uses another region, update the header consistently across config and requests.

#### Usage reminders

- The server is link-based: copy the Figma frame or layer link, then ask the MCP client to implement that URL. The client will extract the node ID from the link (it does not browse the page).
- If output feels generic, restate the project-specific rules from the main skill and ensure you follow the required flow (get_design_context → get_metadata if needed → get_screenshot).

### tools and prompt patterns

Quick reference for the Figma MCP toolset, when to use each tool, and prompt examples to steer output toward your stack.

#### Core tools

- `get_design_context` (Figma Design, Figma Make): Primary tool. Returns structured design data and default React + Tailwind code. Selection-based prompting works on desktop; the remote server uses a frame/layer link to extract the node ID.
- `get_variable_defs` (Figma Design): Lists variables/styles (colors, spacing, typography) used in the selection. Useful to align with tokens.
- `get_metadata` (Figma Design): Sparse XML outline of layer IDs/names/types/positions/sizes. Use before re-calling `get_design_context` on large nodes to avoid truncation.
- `get_screenshot` (Figma Design, FigJam): Screenshot of the selection for visual fidelity checks.
- `get_figjam` (FigJam): XML + screenshots for FigJam diagrams (architecture, flows).
- `create_design_system_rules` (no file context): Generates a rule file with design-to-code guidance for your stack. Save it where the agent can read it.
- `get_code_connect_map` (Figma Design): Returns mapping of Figma node IDs to code components (`codeConnectSrc`, `codeConnectName`). Use to reuse existing components.
- `add_code_connect_map` (Figma Design): Adds/updates a mapping between a Figma node and a code component to improve reuse.
- `get_strategy_for_mapping` (alpha, local only): Figma-prompted tool to decide mapping strategy for connecting a node to a code component.
- `send_get_strategy_response` (alpha, local only): Sends the response after `get_strategy_for_mapping`.
- `whoami` (remote only): Returns the authenticated Figma user identity (email, plans, seat types).

#### Prompt patterns (design context)

- Change framework: "generate my Figma selection in Vue" or "in plain HTML + CSS" or "for iOS".
- Use my components: "generate my Figma selection using components from `src/components/ui`".
- Combine: "generate my Figma selection using components from `src/ui` and style with Tailwind".
- Note: On the remote server, selection-based prompting requires a frame/layer link; the server extracts the node ID from the URL.

#### Prompt patterns (variables/styles)

- "get the variables used in my Figma selection"
- "what color and spacing variables are used in my Figma selection?"
- "list the variable names and their values used in my Figma selection"

#### Prompt patterns (code connect)

- "show the code connect map for this selection"
- "map this node to `src/components/ui/Button.tsx` with name `Button`"

#### Best-practice flow reminder

Use `get_design_context` → (optionally `get_metadata` for large nodes) → `get_screenshot`, and keep project rules from `SKILL.md` in mind when applying the generated output.

## Core Operations

```typescript
// Get file
figma.getFile({ file_key: 'file-key' })

// Get file nodes
figma.getFileNodes({
  file_key: 'file-key',
  ids: ['node-id-1', 'node-id-2']
})

// Get file versions
figma.getFileVersions({ file_key: 'file-key' })
```

### Image Export

```typescript
// Export images
figma.getImage({
  file_key: 'file-key',
  ids: 'node-id',
  format: 'png',  // png, jpg, svg, pdf
  scale: 2  // @2x resolution
})

// Export multiple
figma.getImage({
  file_key: 'file-key',
  ids: 'node-1,node-2,node-3',
  format: 'svg',
  svg_outline_text: true
})
```

### Components

```typescript
// Get components
figma.getFileComponents({ file_key: 'file-key' })

// Get component sets
figma.getFileComponentSets({ file_key: 'file-key' })

// Get team components
figma.getTeamComponents({ team_id: 'team-id' })
```

### Styles

```typescript
// Get file styles
figma.getFileStyles({ file_key: 'file-key' })

// Get team styles
figma.getTeamStyles({ team_id: 'team-id' })
```

### Comments

```typescript
// Get comments
figma.getComments({ file_key: 'file-key' })

// Post comment
figma.postComment({
  file_key: 'file-key',
  message: 'Approved for development',
  comment_id: 'parent-comment-id'  // For replies
})
```

### Common Patterns

#### Design Token Export

```typescript
// Export design tokens from Figma styles
const { data: { styles } } = await figma.getFileStyles({ file_key: fileKey })

const tokens = {
  colors: {},
  typography: {},
  spacing: {}
}

for (const style of Object.values(styles)) {
  if (style.style_type === 'FILL') {
    tokens.colors[style.name] = extractColor(style)
  } else if (style.style_type === 'TEXT') {
    tokens.typography[style.name] = extractTextStyle(style)
  }
}

// Write to tokens.json
fs.writeFileSync('tokens.json', JSON.stringify(tokens, null, 2))
```

### Component Sync

```typescript
// Sync Figma components to code
const { data: components } = await figma.getFileComponents({ file_key: fileKey })

for (const component of Object.values(components)) {
  // Export component as SVG
  const { data: images } = await figma.getImage({
    file_key: fileKey,
    ids: component.node_id,
    format: 'svg'
  })
  // Save to assets folder
  const svg = await fetch(images[component.node_id]).then(r => r.text())
  fs.writeFileSync(`assets/icons/${component.name}.svg`, svg)
}
```

### Screenshot Generation

```typescript
// Generate screenshots for documentation
const screens = ['home-screen', 'login-screen', 'dashboard']

for (const screenId of screens) {
  const { data: images } = await figma.getImage({
    file_key: fileKey,
    ids: screenId,
    format: 'png',
    scale: 2
  })

  const imageUrl = images[screenId]
  const response = await fetch(imageUrl)
  const buffer = await response.buffer()

  fs.writeFileSync(`docs/screenshots/${screenId}.png`, buffer)
}
```

### Design Review Automation

```typescript
// Check for new comments
const { data: comments } = await figma.getComments({ file_key: fileKey })

const unresolved = comments.filter(c => !c.resolved_at)

if (unresolved.length > 0) {
  console.log(`${unresolved.length} unresolved design comments`)
  // Create Jira issues for unresolved comments
  for (const comment of unresolved) {
    await jira.issues.createIssue({
      fields: {
        project: { key: 'DESIGN' },
        summary: `Design feedback: ${comment.message.substring(0, 50)}`,
        description: comment.message,
        issuetype: { name: 'Task' }
      }
    })
  }
}
```

### Design System Documentation

```typescript
// Generate component documentation
const { data: components } = await figma.getFileComponents({ file_key: fileKey })

let markdown = '# Design System Components\n\n'

for (const [id, component] of Object.entries(components)) {
  // Export thumbnail
  const { data: images } = await figma.getImage({
    file_key: fileKey,
    ids: id,
    format: 'png',
    scale: 1
  })

  markdown += `## ${component.name}\n\n`
  markdown += `![${component.name}](${images[id]})\n\n`
  markdown += `**Description:** ${component.description || 'No description'}\n\n`
}

fs.writeFileSync('docs/design-system.md', markdown)
```

## Best Practices

✅ **DO:**

- Cache file data to reduce API calls
- Use version history for tracking changes
- Export assets at appropriate resolutions
- Document component usage
- Use meaningful component names
- Keep design tokens in sync
- Handle rate limits (requests/minute)

❌ **DON'T:**

- Export entire files repeatedly
- Ignore version control
- Hardcode file keys
- Skip error handling
- Export at wrong resolutions
- Commit API tokens

## Configuration

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-figma"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "your-personal-access-token"
      }
    }
  }
}
```

**Setup:**

1. Generate personal access token: Account Settings → Personal Access Tokens
2. Grant appropriate scopes (file content, comments)
3. Store token securely

## Integration Patterns

### CI/CD Asset Pipeline

```bash
# Export icons on every design update
figma-export --file-key=$FIGMA_FILE --format=svg --output=src/assets/icons

# Optimize SVGs
svgo --folder src/assets/icons

# Commit if changes detected
git diff --quiet src/assets/icons || git commit -m "chore: Update design assets"
```

### Design-to-Code Workflow

```typescript
// 1. Detect design changes
const currentVersion = await figma.getFile({ file_key: fileKey })
const lastVersion = loadLastProcessedVersion()

if (currentVersion.version !== lastVersion) {
  // 2. Export updated components
  await exportComponents(fileKey)

  // 3. Generate code
  await generateComponentCode()

  // 4. Run tests
  await runVisualRegressionTests()

  // 5. Create PR if tests pass
  if (testsPass) {
    await createPullRequest('Update components from Figma')
  }
}
```

## References

- `references/figma-mcp-config.md` — setup, verification, troubleshooting, and link-based usage reminders.
- `references/figma-tools-and-prompts.md` — tool catalog and prompt patterns for selecting frameworks/components and fetching metadata.
