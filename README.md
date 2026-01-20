# Cursor AI Skills Collection

A collection of custom skills for Cursor AI that provide specialized knowledge and workflows.

## Available Skills

| Skill | Description |
|-------|-------------|
| [x-algorithm-posting](./x-algorithm-posting/) | Expert guidance for writing X (Twitter) posts optimized for the algorithm |

## Installation

### Option 1: Install Individual Skill

1. Download or clone this repository
2. Copy the skill folder you want to `~/.codex/skills/`

```bash
cp -r x-algorithm-posting ~/.codex/skills/
```

### Option 2: Install All Skills

```bash
cp -r */ ~/.codex/skills/
```

## How Skills Work

Skills automatically activate in Cursor AI when your conversation matches their description. Each skill contains:

- `SKILL.md` - Main instructions and quick reference
- `references/` - Detailed documentation loaded on-demand

## Skills Overview

### x-algorithm-posting

Based on analysis of the open-source X (Twitter) recommendation algorithm. Provides:

- **Scoring signals** - What boosts vs tanks your reach
- **Content templates** - Ready-to-use tweet formats  
- **Strategic tactics** - Algorithm-backed growth strategies
- **Deep dive reference** - Complete technical breakdown

**Triggers when:** Writing tweets, planning X content strategy, asking about Twitter algorithm

## Contributing

To add a new skill:

1. Create a folder with your skill name (lowercase, hyphenated)
2. Add a `SKILL.md` with YAML frontmatter (`name` and `description`)
3. Optionally add `references/`, `scripts/`, or `assets/` folders
4. Update this README

## License

MIT
