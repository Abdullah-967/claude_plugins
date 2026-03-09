# Feynman Course Plugin

A Claude Code plugin that transforms complex topics into structured, first-principles courses published to Notion with Mermaid diagrams.

## How It Works

```
/feynman <topic> <level>
    -> Diagnose learner needs
    -> Decompose concept (first principles)
    -> Build 3-7 lesson course
    -> Embed Mermaid diagrams (1 per lesson)
    -> Publish to Notion
    -> Return course link
    -> Offer optional mastery check
```

## Usage

```
/feynman quantum computing beginner
/feynman machine learning intermediate
/feynman distributed systems advanced
```

**Levels:** beginner, novice, intermediate, advanced, expert

After studying, return for a mastery check: the plugin will run a 5-question adaptive assessment.

## Prerequisites

- **Notion MCP server** configured with a valid `NOTION_API_KEY`

## Setup

1. Set your Notion API key as an environment variable:
   ```
   export NOTION_API_KEY=your_key_here
   ```

2. Test the plugin locally:
   ```
   claude --plugin-dir ./feynman-course-plugin
   ```

## Plugin Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/feynman` | Entry point - accepts topic and level |
| Agent | `learning-orchestrator` | Orchestrates the full pipeline |
| Skill | `diagnose-and-map` | Learner diagnosis and prerequisite mapping |
| Skill | `build-course` | Lesson content generation |
| Skill | `design-visuals` | Mermaid diagram specifications |
| Skill | `publish-course` | Notion page publishing |
| Skill | `mastery-check` | Optional adaptive assessment |

## MCP Servers

Configure in `.mcp.json` at plugin root. Required:
- **Notion** (`@notionhq/notion-mcp-server`)

## Version

0.3.0
