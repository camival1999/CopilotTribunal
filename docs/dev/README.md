# Development Tracking

This folder is maintained by the **Scribe** agent. It tracks ongoing development progress, known issues, and project roadmap.

## Contents

| File | Purpose | Updated By |
|------|---------|------------|
| [ROADMAP.md](ROADMAP.md) | Future plans, milestones, version targets | Scribe |
| [CHANGELOG.md](CHANGELOG.md) | Version history, what changed when | Scribe |
| [KNOWN-ISSUES.md](KNOWN-ISSUES.md) | Active bugs, workarounds, resolutions | Scribe |
| [FEATURES/](FEATURES/) | Per-feature status and implementation notes | Scribe |

## How This Works

The Scribe agent automatically updates these files when:

| Event | Action |
|-------|--------|
| Feature completed | Update FEATURES/, CHANGELOG.md, ROADMAP.md |
| Bug discovered | Add to KNOWN-ISSUES.md |
| Bug fixed | Move to resolved in KNOWN-ISSUES.md, add to CHANGELOG.md |
| Milestone reached | Update ROADMAP.md, CHANGELOG.md |

## For Humans

Feel free to manually update these files as well. The Scribe will preserve your changes and continue tracking from there.

## Related

- [Scribe Agent](../../.github/agents/scribe.agent.md)
- [Project Specifications](../specs/)
