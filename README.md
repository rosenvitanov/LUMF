<p align="center">
  <img src="assets/lumf_logo_white.png" alt="LUMF Logo" width="800" />
</p>

# LUMF (Linked Unit Markdown Format)

*A project by* <img src="assets/river_lab.png" height="25" align="absmiddle" /> **RiverLab**

![Format](https://img.shields.io/badge/Format-Markdown%20%2B%20YAML-blue)
![Status](https://img.shields.io/badge/Status-Draft%20v1.0-orange)
![Paradigm](https://img.shields.io/badge/Paradigm-Local--first-success)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

🌍 *[Български](README.bg.md)*

LUMF (Linked Unit Markdown Format) is a decentralized, local-first format for managing notes, tasks, reminders, and projects. It is built entirely on plain text (Markdown) and structured data (YAML), ensuring that the data is always readable, future-proof, and completely owned by the user.

## Core Principles
1. **Source of Truth**: The `.md` files are the only source of truth. Any database or index is just a transient cache.
2. **Atomic Units**: One logical unit (task, note, project) = One file. This minimizes sync conflicts when used with Syncthing or Git.
3. **Graph Network**: Files are connected to each other using explicit YAML links (`parent`, `children`, `blocked_by`), forming a Directed Acyclic Graph (DAG).
4. **Easy Synchronization**: Designed from the ground up for seamless syncing across all your devices using tools like Syncthing or Git, without the risk of database corruption.

## Why for Developers?
LUMF is designed with developer experience (DX) in mind:
* **Zero Vendor Lock-in & Easy Parsing:** Relies on standard YAML and Markdown. No binary blobs, proprietary APIs, or hidden databases.
* **CLI Friendly:** You can filter, search, and mutate your database using basic terminal tools (`grep`, `awk`, `sed`, `ripgrep`).
* **Git/VCS Compatibility:** Unlike SQLite or JSON dumps, LUMF results in a perfect `diff` when tracking state changes over time.
* **Automation Freedom:** It's trivial to write local scripts, cron jobs, or Git hooks that react to task statuses like `todo` or `blocked`.

## 📚 Documentation
Dive into the details in the `docs/` folder:
1. **[Getting Started](docs/getting_started.md)** – Introduction and how to kick off your workspace.
2. **[Architecture Decisions (ADR)](docs/architecture_and_spec.md)** – Why LUMF is designed the way it is.
3. **[Folder Structure](docs/folder_structure.md)** – Recommended way to organize your directories.
4. **[YAML Specification](docs/yaml_schema.md)** – Detailed reference for all supported frontmatter fields.

## Scripting & Automation Examples
LUMF shines when you build simple scripts around it.

**Find all blocked tasks via ripgrep:**
```bash
rg "status: blocked" ./Projects -l
```

**Extract task dependencies via Python:**
```python
import frontmatter
import glob

for file in glob.glob("*.md"):
    post = frontmatter.load(file)
    if post.get('status') == 'todo':
        print(f"Task: {post['title']} | Blocks: {post.get('blocking', 'None')}")
```
*For more scripts, API wrappers, and workflows, see [docs/scripts_and_automation.md](docs/scripts_and_automation.md).*

## Tooling & Ecosystem (🚧 Under construction)
The format is designed as a foundation for a broader toolset:
* **(🚧 UNDER DEVELOPMENT) CLI Managers:** Fast task logging directly from the terminal.
* **(🚧 UNDER DEVELOPMENT) Editor Extensions:** Linters and autocomplete tools for VS Code/Neovim.
* **(🚧 UNDER DEVELOPMENT) Language SDKs:** Ready-to-use parsers for Python, C, C#, bash, power shell.

*Read more about expanding the format in [docs/ecosystem_and_tooling.md](docs/ecosystem_and_tooling.md).*

## LUMF vs Existing Solutions

| Feature                     | **LUMF**           | Notion / Jira   | Obsidian / Logseq | JSON / SQLite |
| :--- | :--- | :--- | :--- | :--- |
| **Human Readable (Raw)**  | 🟢 Yes (Excellent) | 🔴 No           | 🟢 Yes            | 🟡 Moderate / Poor |
| **Data Ownership**        | 🟢 100% Local      | 🔴 Cloud (Locked)| 🟢 100% Local     | 🟢 100% Local |
| **Git Diff / VCS Friendly**| 🟢 Perfect `diff`  | 🔴 N/A          | 🟢 Good           | 🔴 Poor (Conflicts) |
| **Native Dependency Logic**| 🟢 Built-in YAML   | 🟢 Built-in     | 🟡 Requires Plugins| 🟢 Schema dependent |
| **Needs App to Parse**    | 🟢 No (Text editor)| 🔴 Yes          | 🟡 Optional       | 🔴 Yes        |

## File Naming Convention
To avoid synchronization collisions and maintain human readability, every file follows this strict naming structure:

`[YYYYMMDDHHMM]-[XXXX]-[slug].md`

* **YYYYMMDDHHMM**: Timestamp (e.g., `202605271030`)
* **XXXX**: A 4-character random alphanumeric suffix to prevent collisions (e.g., `A1B2`)
* **slug**: A short, human-readable description in lowercase latin characters (e.g., `kitchen-renovation`)

*Example: `202605271030-A1B2-kitchen-renovation.md`*

## Workspace Directory Structure
To maintain system integrity and ease of use, a LUMF workspace maintains a basic standard structure, which can be viewed in detail with diagrams at:
**👉 [Folder Structure Documentation](docs/folder_structure.md)**

## System Templates
- **[Task Template](.system/templates/task_template.md)**: Blank template for creating a new task.
- **[Project Template](.system/templates/project_template.md)**: Blank template for starting a new project.

## Examples
To help you understand how files interconnect, check out these examples:
- **[Bathroom Renovation Project](examples/202605271200-P1R1-bathroom-renovation.md)** (Parent Project)
- **[Buy Tiles](examples/202605271205-T1S1-buy-tiles.md)** (Active task, blocks the next one)
- **[Install Tiles](examples/202605271210-T2S2-install-tiles.md)** (Blocked task, waiting for materials)

## Further Documentation
- **[YAML Schema Specification](docs/yaml_schema.md)**: Detailed explanation of all supported YAML frontmatter fields.

