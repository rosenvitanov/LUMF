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

* `status` *(String)*: Current state. Default values: `todo`, `in_progress`, `blocked`, `failed`, `done`, `cancelled`.
* `status_reason` *(String)*: Reason for `blocked`/`failed` (e.g., `blocked_by_materials`).
* `priority` *(String)*: Priority level. Values: `low`, `medium`, `high`, `critical`. 
* `start_after` *(Date/Time)*: Earliest start date/time (e.g., `YYYY-MM-DD`).
* `due` *(Date/Time)*: Deadline (e.g., `YYYY-MM-DD`).
* `duration_estimate` *(String)*: Estimated duration (e.g., `2d`, `6h`, `90m`).
* `color` *(String)*: Visual marker for UI. Can be a semantic name (`red`, `urgent-red`) or a Hex code (`#ff5733`).
* `context` *(Array)*: List of operational contexts or tags (e.g., `[home, office, supermarket, phone]`).
* `assigned_to` *(Array)*: Involved people/teams, not necessarily a single owner (e.g., `["Install Team", "Architect"]`).
* `location` *(String)*: Location or room (e.g., `"Bathroom"`).

### Custom statuses (flexible)
For user flexibility, you can define custom statuses that complement the standard ones.

* `custom_statuses` *(Array)*: List of allowed custom statuses (e.g., `[waiting_review, waiting_delivery]`).

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

## 5. Metadata, Extensibility & Integrity (Optional)
These blocks provide additional flexibility, security, and future-proofing for the format.

* `metadata` *(Object)*: Container for descriptive and systemic information.
  * `author` *(String)*: Creator or last editor of the file (e.g., `"John Doe"`, `"SystemBot"`).
  * `locale` *(String)*: Language and regional settings (e.g., `"bg-BG"`, `"en-US"`).
  * `dependencies` *(Array)*: External resources, schemas, or plugins required to fully load the file.
* `extensions` *(Object)*: Isolated container (Custom Data) for user or app-specific data that is not part of the official standard. Parsers can safely ignore this block.
* `checksum` *(String)*: Hash value (e.g., `sha256-...`) to verify the file's integrity during sync.

## Example
```yaml
---
id: 202605271030-A1B2
type: task
title: "Buy Ceramic Tiles"
created: 2026-05-27 10:30:00
updated: 2026-05-27 10:45:00

metadata:
  author: "Rosen"
  locale: "en-US"
  dependencies: ["https://schemas.example.com/tiles_plugin"]

checksum: "sha256-a1b2c3d4e5f6..."
extensions:
  custom_billing_id: "BILL-999"

status: todo
status_reason: blocked_by_materials
priority: high
start_after: 2026-05-28
due: 2026-05-30
duration_estimate: 2d
context: [supermarket, driving]
assigned_to: ["Install Team"]
location: "Bathroom"

custom_statuses: [waiting_review, waiting_delivery]

parent: 202605270900-R2D2
blocked_by: []
blocking: [202605271100-C3P0]

assets:
  - path: "_assets/2026/quote.pdf"
    label: "Store Quote"
---
```