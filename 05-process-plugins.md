# Part 5: 10 Process Plugins I Frequently Used

This guide is a factual reference for the Drupal Migration process plugins I used most frequently during the SAcommunity.org migration.

> **Note:** There is no video for this section as it is a reference list.

## General Advice

It is useful to get an overall picture of what Drupal Migration Process plugins are available. I recommend you check out the official documentation:

* **[Drupal Migrate Process Plugins Documentation](https://www.drupal.org/docs/8/api/migrate-api/migrate-process-plugins/list-of-migrate-process-plugins)**

**My Recommendation:**
1.  **Overview first:** Just have an overall picture of what is available; you do not need to go deep in the beginning.
2.  **Just-in-Time Learning:** Whenever you are adding a new field migration, check if there is a process plugin you might need, then go deep from there.
3.  **Custom Code:** If you are unable to find the plugin you need, you can write your own custom plugin (if you know PHP), or do your own data preprocessing using a language you know (like Python) before importing.

---

## Process Plugin Structure

Every process plugin configuration starts with the `plugin` key, followed by plugin-specific arguments:

```yaml
process:
  destination_field:
    plugin: plugin_name
    source: source_field
    # Additional plugin-specific arguments
```

**Common Arguments:**

  * `plugin`: (Required) The name of the process plugin to use.
  * `source`: (Usually required) The source field or value to process.

---

## The Plugins

### 1. default_value

**Purpose:** Provides a fallback value when the source is empty.

**Example:**

```yaml
process:
  field_status:
    plugin: default_value
    source: status
    default_value: "Active"
```

**Arguments:**

  * `source`: (Required) The source field to check.
  * `default_value`: (Required) The value to use if source is empty.

### 2. explode

**Purpose:** Splits a string into an array of strings based on a delimiter.

**Example:**
If the source data is `"Car, Motorbike, Bus"`:

```yaml
process:
  field_tags:
    plugin: explode
    source: tags_string
    delimiter: ","
```

*Result:* `["Car", " Motorbike", " Bus"]`

**Arguments:**

  * `source`: (Required) The source string to split.
  * `delimiter`: (Required) The boundary string that separates values.
  * `limit`: (Optional) Maximum number of splits.
      * Positive number: Return at most this many elements (last element contains remainder).
      * Negative number: Return all elements except the last N.
      * Zero: Treated as 1.

### 3. callback

**Purpose:** Executes a PHP function or custom method on the source value.

**Example:**

```yaml
process:
  field_title:
    plugin: callback
    source: title
    callable: trim
```

**Common Use Cases:**

  * `trim`, `rtrim`, `ltrim`: Remove whitespace.
  * `strtolower`, `strtoupper`: Convert case.
  * `ucfirst`: Capitalize first letter.

**Arguments:**

  * `source`: (Required) The source value to process.
  * `callable`: (Required) The name of the PHP function or path to a custom function.
  * `unpack_source`: (Optional) If `true`, treats the source as an array and unpacks it as function arguments.

### 4. skip_on_empty

**Purpose:** Skips processing when the source value is empty.

**Example:**

```yaml
process:
  field_address/country_code:
    - plugin: skip_on_empty
      source: address_line_1
      method: process
    - plugin: default_value
      default_value: AU
```

**Arguments:**

  * `source`: (Required) The value to check.
  * `method`: (Required) What to skip when empty.
      * `row`: Skips the entire row/record (won't create this entity).
      * `process`: Skips only the remaining process steps for this field.
  * `message`: (Optional) Custom message to log when skipping occurs.

### 5. static_map

**Purpose:** Maps source values to target values using a predefined lookup table.

**Example:**

```yaml
process:
  field_address/administrative_area:
    plugin: static_map
    source: s_state
    map:
      "South Australia": SA
      "Victoria": VIC
      "New South Wales": NSW
    default_value: SA
```

**Arguments:**

  * `source`: (Required) The source value to map.
  * `map`: (Required) An associative array defining `source` → `target` mappings.
  * `bypass`: (Optional, default: `false`)
      * `false`: Throws an exception if value not found in map.
      * `true`: Returns the original source value.
  * `default_value`: (Optional) Value to use when source is not found in the map.

### 6. entity_lookup

**Purpose:** Searches for existing entities and returns their IDs. Useful for referencing existing taxonomy terms, nodes, or users.

**Example:**

```yaml
process:
  field_tags:
    plugin: entity_lookup
    source: tags
    entity_type: taxonomy_term
    value_key: name
    bundle_key: vid
    bundle: tags
    ignore_case: true
```

**Arguments:**

  * `source`: (Required) The value to search for.
  * `entity_type`: (Required) Type of entity (e.g., `taxonomy_term`, `node`, `user`).
  * `value_key`: (Required) The entity field to search in (e.g., `name`, `title`).
  * `bundle`: (Optional) Machine name of the bundle (e.g., vocabulary name).
  * `bundle_key`: (Optional) Field name that contains the bundle type (e.g., `vid` for taxonomy).
  * `ignore_case`: (Optional, default: `false`) Whether the search is case-insensitive.
  * `access_check`: (Optional) Whether to check access permissions.
  * `operator`: (Optional) Comparison operator for the query.

### 7. entity_generate

**Purpose:** Extends `entity_lookup`. It searches for an existing entity first, and if not found, generates a new one.

**Arguments:**

  * **All arguments from `entity_lookup` (see above).**
  * `default_values`: (Optional) Static values to set on generated entities.
  * `values`: (Optional) Dynamic values from source data for generated entities.

### 8. migration_lookup

**Purpose:** Looks up IDs from other migrations. Essential for creating relationships (e.g., chaining migrations, Paragraphs, which are a tricky topic that will be explained in another blog).

**Example:**

```yaml
process:
  field_phone:
    plugin: migration_lookup
    source: contact_id
    migration: contact_paragraph
    no_stub: true
```

**Arguments:**

  * `source`: (Required) The source ID to look up.
  * `migration`: (Required) Single migration ID or array of migration IDs to search.
  * `no_stub`: (Optional, default: `false`)
      * `false`: Creates a stub entity if lookup fails.
      * `true`: Returns NULL if lookup fails (no stub created).
  * `source_ids`: (Optional) Override source ID configuration.
  * `stub_id`: (Optional) Specific stub ID to use.

### 9. sub_process

**Purpose:** Processes arrays of data where each element needs individual transformation. Useful for complex nested data structures like **Opening Hours**.

**Example Source Data:**

```json
"office_hours": [
  { "day": 1, "start": 900, "end": 1700 },
  { "day": 2, "start": 900, "end": 1700 }
]
```

**Migration Configuration:**

```yaml
process:
  field_office_hours:
    plugin: sub_process
    source: office_hours
    process:
      day: day
      starthours: start
      endhours: end
```

**Arguments:**

  * `source`: (Required) The source field containing an array.
  * `process`: (Required) Nested process configuration for each array element.
  * `key`: (Optional) Source key to use for the array key.
  * `include_source`: (Optional) Whether to include the source data in output.
  * `source_key`: (Optional) Specific key to extract from source.

### 10. log

**Purpose:** Acts like a print statement for debugging. It allows you to see the value of a field at a specific point in the pipeline. I usually remove them after finishing with debugging.

**Example:**
Source String: `"Car, Motorbike, Bus"`

```yaml
process:
  field_tags:
    - plugin: explode
      source: tags_string
      delimiter: ","
    - plugin: log
    - plugin: callback
      callable: trim
    - plugin: log
```

First log (Before Trim):

```text
Array
(
    [0] => Car
    [1] =>  Motorbike
    [2] =>  Bus
)
```

Second log (After Trim):

```text
Array
(
    [0] => Car
    [1] => Motorbike
    [2] => Bus
)
```

**Arguments:**

  * No arguments. It simply logs the current value to the console/terminal.

[< Back to Home](/README.md)
