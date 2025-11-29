# Part 4: Modifying Drupal Migration and My Frequently Used Migration Commands

This guide covers how to update your existing migration configuration when you add new fields, and a breakdown of the essential command line interface (CLI) commands I use frequently.

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%;">
  <iframe 
    src="https://www.youtube.com/embed/356ct-76LF8?si=IiYFVeJnIAYLK3Fv" 
    title="YouTube video player"
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" 
    frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
    referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
  </iframe>
</div>

---

## Modifying Existing YAML

We're adding a new field: a **website URL** to our demo data. To migrate this, we need to modify our YAML file and update the configuration in Drupal.

### 1. The YAML Modifications

First, ensure you have created the new **Website** field in your Organisation content type in the UI. Then, add these lines to your YAML file:

**A. Update the `source` section:**
Add the new field definition to the `fields` array within the `source` block.

```yaml
source:
  # ... other source configuration ...
  fields:
    # ... existing fields (id, name, short_description) ...
    - name: website
      selector: url
````

**B. Update the `process` section:**
We are mapping the source variable (`website`) to the destination field (`field_website`). Since this is a Link field type in Drupal, it requires subfields (`uri` and `title`).

```yaml
process:
  # ... other existing process mappings ...
  field_website/uri: website
  field_website/title:
    plugin: default_value
    default_value: 'Visit Website'
```

### 2. Updating the Configuration (The Correct Way)

If you try to simply re-import the updated YAML, you'll get an error: *"An entity with this machine name already exists but the import did not specify a UUID."*

This is because Drupal's configuration management system requires a **UUID (Universally Unique Identifier)** to recognize and update an existing configuration. Otherwise it is confused whether you are uploading a new migration with a similar migration ID, which is not allowed.

#### 🔧 Method A: UI (Quickest for one change)

1.  Go to **Drupal admin > Configuration > Development > Configuration Synchronization**.
2.  Select the **Export** tab, then **Single item** tab.
3.  Under the 'Configuration type' dropdown, select **migration**, and choose the migration you previously imported (e.g., `organisation_migration`).
4.  The full migration YAML configuration, **including the necessary UUID**, will appear in the box. Your can choose to copy the **entire block** or just the **UUID**.
5.  **Paste the UUID** into the the YAML with your new `source` and `process` lines.
6.  Go back to the **Import** tab, then **Single item**, select **migration**, and paste the **full, updated YAML configuration**.

#### 💻 Method B: CLI and File System (Best for iterative development)

1.  **Export the current configuration** (this pulls the UUIDs from the database into your local files):
    ```bash
    ddev drush cex 
    ```
    (Select `yes` and Enter to confirm the export.)
2.  Search for your migration YAML file in your local sync path (e.g., `web/sites/default/files/sync/`), it should start with **migrate**.
3.  Modify the YAML file directly, save it.
4.  **Import the updated configuration** from your files back into the database:
    ```bash
    ddev drush cim 
    ```
    (This ensures the YAML configuration is updated with the correct UUID.)

## 3. The Migration Execution Caveat (The `--update` Flag)

After updating the YAML, if you run a standard `migrate:import` command (or just click "Execute" in the UI), **nothing will happen** for previously imported items. The URL fields will not be imported.

Why? Drupal assumes items already successfully migrated are finished. To force it to look at existing data again, you must use the **Update** option.

  * **In UI:** Expand the **"Additional execution options"** and check the **"Update"** box.
  * **In CLI:** Use the **`--update`** flag.

## 4. Frequently Used CLI Migration Commands

All commands start with `ddev drush`. You can check the full documentation for flags here: [Drush Migrate Import Docs](https://www.drush.org/12.x/commands/migrate_import/).

| Command | Purpose | Essential Flags/Notes |
| :--- | :--- | :--- |
| `migrate:status` | Check the status of all migrations (Idle, Importing, Finished, etc.). | |
| `migrate:import [IDs]` | Imports data for the specified migration(s). | `--all`: Process all migrations. <br> `--update`: Update previously-imported items (CRUCIAL for modifying fields). <br> `--tag=TAG`: Import migrations based on a common tag (e.g., `--tag=paragraph`). <br> `--limit=LIMIT`: Only process a set number of items (great for testing, e.g., `--limit=10`). |
| `migrate:reset-status [ID]` | Force a migration that is stuck in "importing" mode back to "idle." | Used for debugging if a migration crashes mid-process. |
| `migrate:rollback [IDs]` | Reverts all or selected items migrated by the specified migration. | `--all`: Rollback all migrations. |
| `migrate:messages [ID]` | Shows all error and success messages for the specified migration. | Mostly used for debugging to see why an import failed. |

That’s all for this guide! We now know how to manage and update our migration configuration and use the powerful command line tools.

[< Back to Home](README.md)