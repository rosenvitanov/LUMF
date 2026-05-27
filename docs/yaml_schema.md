# LUMF YAML Schema Specification

🌍 *[Български](yaml_schema.bg.md)* | 🔙 *[Back to README](../README.md)*

Every LUMF file must start with a YAML block (Frontmatter) bounded by `---`. This metadata is what makes the files parsable and allows them to act as a decentralized database.

## 1. System Fields (Required)
These fields are mandatory for every LUMF file.

* `id` *(String)*: The full unique identifier matching the filename prefix (e.g., `202605271030-A1B2`). Critical for linking files even if the filename slug changes.
* `type` *(String)*: The object type. Supported values: `task`, `note`, `project`, `reminder`, `asset`.
* `title` *(String)*: Human-readable title of the document. Must be wrapped in quotes if it contains a colon.
* `created` *(Datetime)*: Timestamp of creation in ISO 8601 format (`YYYY-MM-DD HH:mm:ss`).
* `updated` *(Datetime)*: Timestamp of last modification. Essential for Syncthing conflict resolution and cache invalidation.

## 2. Functional Fields (Optional)
Used specifically for tasks, reminders, and projects.

* `status` *(String)*: Current state. Default values: `todo`, `doing`, `done`, `blocked`, `ready`, `cancelled`.
* `priority` *(String)*: Priority level. Values: `low`, `medium`, `high`, `critical`. 
* `due` *(Date/Time)*: Deadline (e.g., `YYYY-MM-DD`).
* `color` *(String)*: Visual marker for UI. Can be a semantic name (`red`, `urgent-red`) or a Hex code (`#ff5733`).
* `context` *(Array)*: List of operational contexts or tags (e.g., `[home, office, supermarket, phone]`).

## 3. Graph Logic & Relationships (Optional)
Used for maintaining the directed acyclic graph (DAG) structure.

* `parent` *(String)*: The `id` of the parent project or overarching task.
* `children` *(Array)*: A list of `id`s for sub-tasks belonging to this item.
* `blocked_by` *(Array)*: A list of `id`s of tasks that *must* be completed before this file can become `ready` or `doing`.
* `blocking` *(Array)*: A list of `id`s of tasks that this task is currently preventing from starting.

## 4. Assets & Inventory (Optional)
* `assets` *(Array of Objects)*: Direct references to binary files located in `_assets/`.
  * `path` *(String)*: Relative path from the repository root (e.g., `_assets/2026/invoice.pdf`).
  * `label` *(String)*: What the asset is (e.g., `"Parts Invoice"`).
* `requires` *(Array)*: Inventory or tools needed to execute the task (e.g., `["Hammer", "2 bags of cement"]`).

## Example
```yaml
---
id: 202605271030-A1B2
type: task
title: "Buy Ceramic Tiles"
created: 2026-05-27 10:30:00
updated: 2026-05-27 10:45:00

status: todo
priority: high
due: 2026-05-30
context: [supermarket, driving]

parent: 202605270900-R2D2
blocked_by: []
blocking: [202605271100-C3P0]

assets:
  - path: "_assets/2026/quote.pdf"
    label: "Store Quote"
---
```