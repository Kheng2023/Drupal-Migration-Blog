# SAcommunity.org Drupal Migration Guide

## A playlist for volunteers

Hi everyone! This is a guide for the **sacommunity.org web-rebuild team** to understand the basics of Drupal migration. We are focusing mainly on migrating from **JSON to Drupal 11**.

These guides accompany my video series and serve as written documentation for the commands and concepts we use.

### The Guides

* **[Part 1: Key Concepts and Advice](./01-concepts.md)**
    * Terminology you need to know (Content Types, Fields, YAML).
    * My personal lessons learned (why you shouldn't trust ChatGPT too much!).
    * Recommended resources.

* **[Part 2: Getting Started & Navigation](./02-getting-started.md)**
    * Installing `migrate_plus` and `migrate_tools`.
    * Creating your first Content Type.
    * Importing your YAML configuration.
    * Running migrations via UI vs CLI.

* **[Part 3: Explaining the Parts of YAML Code](./03-yaml-structure.md)**
    * Understanding the ETL structure (Source, Process, Destination).
    * Essential configurations (id, label, group).
    * How to configure the Source plugin to read JSON.
    * Basic Process mapping and Destination settings.

* **[Part 4: Modifying Drupal Migration & Migration Commands](./04-modifying-yaml-cli.md)**
    * How to add new fields to an existing migration.
    * The "UUID error" and how to fix it (Export/Import workflow).
    * Why your updates might not show up (The `--update` flag).
    * Essential CLI commands: `migrate:status`, `migrate:rollback`, and `migrate:reset-status`.

---
*More guides coming soon.*