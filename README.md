# LUMF (Linked Unit Markdown Format)

🌍 *[Български](README.bg.md)*

LUMF (Linked Unit Markdown Format) is a decentralized, local-first format for managing notes, tasks, reminders, and projects. It is built entirely on plain text (Markdown) and structured data (YAML), ensuring that the data is always readable, future-proof, and completely owned by the user.

## Core Principles
1. **Source of Truth**: The `.md` files are the only source of truth. Any database or index is just a transient cache.
2. **Atomic Units**: One logical unit (task, note, project) = One file. This minimizes sync conflicts when used with Syncthing or Git.
3. **Graph Network**: Files are connected to each other using explicit YAML links (`parent`, `children`, `blocked_by`), forming a Directed Acyclic Graph (DAG).

## File Naming Convention
To avoid synchronization collisions and maintain human readability, every file follows this strict naming structure:

`[YYYYMMDDHHMM]-[XXXX]-[slug].md`

* **YYYYMMDDHHMM**: Timestamp (e.g., `202605271030`)
* **XXXX**: A 4-character random alphanumeric suffix to prevent collisions (e.g., `A1B2`)
* **slug**: A short, human-readable description in lowercase latin characters (e.g., `kitchen-renovation`)

*Example: `202605271030-A1B2-kitchen-renovation.md`*

## Workspace Directory Structure
To maintain system integrity and ease of use, a LUMF workspace uses the following standard directory structure:

```text
/LUMF_Workspace (Root)
├── .system/           # Hidden directory for systemic elements
│   ├── cache/         # Fast-read transient indexes (not synced)
│   ├── scripts/       # Automation scripts (e.g., integrity checks, auto-taggers)
│   └── templates/     # Boilerplate MD files for new tasks or projects
├── 00_Inbox/          # Unprocessed raw notes or ideas (for quick capture)
├── Projects/          # Main projects (containing child-tasks or references)
├── Tasks/             # Standalone tasks
├── _assets/           # Centralized location for binary files (images, PDFs)
│   └── 2026/          # Segregated by date/topic for organization
└── Archive/           # Location for finished and old files
```

## Further Documentation
- **[YAML Schema Specification](docs/yaml_schema.md)**: Detailed explanation of all supported YAML frontmatter fields.
