# PodBox skills

This repository is the PodBox skills and plugin. It is synced from [`podbox-sh/podbox`](https://github.com/podbox-sh/podbox). Do not edit it here. Changes made here will be overwritten.

The skills live under `skills/`. [`skills/use-podbox`](skills/use-podbox/SKILL.md) is the platform skill. Each other folder makes one kind of episode, such as [`skills/make-podcast-recap`](skills/make-podcast-recap/SKILL.md). An agent that is already connected can also read the same pages from the PodBox MCP server.

## Install

PodBox has two parts, and you need both:

1. **The PodBox connector** (MCP server) at `https://api.podbox.sh/mcp/v1`. It gives your agent the tools that publish episodes to your private feed. Sign in when your agent asks.
2. **The PodBox skills** in this repo (every folder under `skills/`). use-podbox tells your agent how to set up your show, write episodes for the ear, and make covers. make-podcast-recap recaps other podcasts.

### Claude (claude.ai, desktop and mobile apps)

Skills need **Code execution and file creation** turned on in **Settings > Capabilities** (on Team and Enterprise, an owner controls this). Skills work on Free, Pro, Max, Ultra, Team and Enterprise plans.

**Easiest: add the PodBox plugin.** Customize > Plugins > Add > Add marketplace, enter `podbox-sh/skills`, find PodBox, Add, then open the plugin's Connectors tab and connect PodBox. The plugin is saved to your Claude account, so it also shows up in Cowork and Claude Code.

**Or upload the skills only.** Download each skill's zip, use-podbox.zip and make-podcast-recap.zip, from the [latest release](https://github.com/podbox-sh/skills/releases/latest) (don't unzip). Upload each one with Customize > Skills > + > + Create skill > Upload a skill. Add the connector via https://podbox.sh/resources/guides/claude.

### Claude Code

Plugin: `/plugin marketplace add podbox-sh/skills` then `/plugin install podbox@podbox`, choose a scope, run `/mcp` and sign in.

Shell equivalents: `claude plugin marketplace add podbox-sh/skills`, then `claude plugin install podbox@podbox`.

Or copy the skills:

```bash
git clone https://github.com/podbox-sh/skills podbox-skills
mkdir -p ~/.claude/skills
cp -R podbox-skills/skills/* ~/.claude/skills/
```

Use `.claude/skills/` instead of `~/.claude/skills/` to install them for one project. Then add the connector and sign in:

```bash
claude mcp add --transport http podbox https://api.podbox.sh/mcp/v1
```

Run `/mcp` and sign in.

### ChatGPT

- Business, Enterprise, Healthcare and Edu: download each skill's zip, use-podbox.zip and make-podcast-recap.zip, from the [latest release](https://github.com/podbox-sh/skills/releases/latest). Upload each one with Sidebar Plugins > Skills tab > Create > Upload from your computer. ChatGPT scans it first; an admin may need to allow skill uploads.
- Other plans: install the PodBox plugin from the Plugins directory. That listing is in review, so this path waits on the ChatGPT plugin listing.

On any plan, connect PodBox via https://podbox.sh/resources/guides/chatgpt if the plugin did not connect it.

### Codex (CLI and ChatGPT desktop app)

Copy the skills:

```bash
git clone https://github.com/podbox-sh/skills podbox-skills
mkdir -p ~/.agents/skills
cp -R podbox-skills/skills/* ~/.agents/skills/
codex mcp add podbox --url https://api.podbox.sh/mcp/v1
codex mcp login podbox
```

Codex picks up new skills automatically. Restart Codex if the skills do not appear.

Or use the marketplace: `codex plugin marketplace add podbox-sh/skills`, then `/plugins`, install PodBox, and start a new session.

### Cursor

Copy the skills with `cp -R podbox-skills/skills/* ~/.cursor/skills/`. Add the connector to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "podbox": {
      "url": "https://api.podbox.sh/mcp/v1"
    }
  }
}
```

Sign in from Customize.

Cursor also reads `~/.agents/skills/` and `~/.claude/skills/`, so no extra copy is needed if the skills are already installed for Codex or Claude Code.

To install the plugin from this GitHub repo, import `podbox-sh/skills` in Customize. This repo includes `.cursor-plugin/marketplace.json`.

### Grok Bot

Grok Bot keeps skills in your private skill library rather than a folder.

1. Connect apps > Add a custom connector > Remote HTTPS, name PodBox, URL `https://api.podbox.sh/mcp/v1`, Add plugin, sign in.
2. Ask your Bot: "Save the PodBox skills from https://github.com/podbox-sh/skills as private skills."
3. If a Bot can't find it: Connect apps > connected count next to the title > enable under Private skills.

Want daily podcast recaps without setup? Start from the [Podcast Recap template](https://x.ai/bot/VUCIS8sodhRjK6LBm7epy).

### Claude API

Upload each folder under `skills/` with the Skills API (`/v1/skills`), then list each `skill_id` in the `container` of a Messages request with the code execution tool. See https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview. Your app also needs the PodBox MCP server and a PodBox sign-in.

### Muse and other agents

Muse doesn't let you install skills. Connect PodBox as a connector and the agent uses its tools directly. For any other agent that reads MCP resources, connecting PodBox is enough: the server serves use-podbox at `podbox://skill` and each episode skill at `podbox://skill/<name>`.
