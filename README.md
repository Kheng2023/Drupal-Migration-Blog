# SAcommunity.org Drupal Migration Guide

## A playlist for volunteers

Hi everyone! This is a guide for the **sacommunity.org web-rebuild team** to understand the basics of Drupal migration. We are focusing mainly on migrating from **JSON to Drupal 11**.

These guides accompany my video series and serve as written documentation for the commands and concepts we use.

### 📢 Who is this for?
While this guide is written specifically for our volunteer team, the concepts here apply to **anyone learning Drupal Migration**. Whether you are a student, a junior developer, or just curious, feel free to use these resources to learn how to migrate data from JSON into Drupal 11.

> **🔒 Note on Data Privacy**
> To protect internal data and keep things clear, the examples in this guide (field names, JSON structures, company names) have been **generalised**.
>
> We use standard naming conventions (like `field_address` or generic company names) rather than our specific internal database schema. This makes the tutorials easier to follow for everyone, regardless of what project you are working on.

---

### The Guides

* **[Part 1: Key Concepts and Advice](./01-concepts.md)** *(23 Nov 2025)*
    * Terminology you need to know (Content Types, Fields, YAML).
    * My personal lessons learned (why you shouldn't trust ChatGPT too much!).
    * Recommended resources.

* **[Part 2: Getting Started & Navigation](./02-getting-started.md)** *(23 Nov 2025)*
    * Installing `migrate_plus` and `migrate_tools`.
    * Creating your first Content Type.
    * Importing your YAML configuration.
    * Running migrations via UI vs CLI.

* **[Part 3: Explaining the Parts of YAML Code](./03-yaml-structure.md)** *(24 Nov 2025)*
    * Understanding the ETL structure (Source, Process, Destination).
    * Essential configurations (id, label, group).
    * How to configure the Source plugin to read JSON.
    * Basic Process mapping and Destination settings.

* **[Part 4: Modifying YAML & CLI Commands](./04-modifying-yaml-cli.md)** *(29 Nov 2025)*
    * How to add new fields to an existing migration.
    * The "UUID error" and how to fix it (Export/Import workflow).
    * Why your updates might not show up (The `--update` flag).
    * Essential CLI commands: `migrate:status`, `migrate:rollback`, and `migrate:reset-status`.

* **[Part 5: 10 Process Plugins I Frequently Used](./05-process-plugins.md)** *(7 Dec 2025)*
    * A reference guide to the most common plugins and YAML examples.
    * How to use `explode`, `static_map`, `entity_lookup`, `sub_process`, and more.
    * Debugging migrations with the `log` plugin.

---
*More guides coming soon.*