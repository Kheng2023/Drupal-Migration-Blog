# Part 3: Explaining the Parts of YAML Code

As promised in previous guides, this part is about understanding the parts of the Drupal migration YAML.

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%;">
  <iframe 
    src="https://www.youtube.com/embed/OWZRkyHbYRs?si=lPW2ARZ9hiXGW563" 
    title="YouTube video player"
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" 
    frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
    referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
  </iframe>
</div>

---

## The ETL Concept

Just like the ETL process in data engineering, a Drupal Migration YAML covers 3 main parts:

1.  **Source** == Extraction
2.  **Process** == Transformation
3.  **Destination** == Loading

## 1. The Configuration (Metadata)

Before starting the 3 main parts, there are 3 configurations we need to define at the very top of the file:

```yaml
id: organisation_migration
label: "Organisation Node Migration"
migration_group: demo_migration
```

  * **id:** The unique ID you give to this migration (give it a meaningful name).
  * **label:** A short description of the migration (string).
  * **migration_group:** Which group you want to organize this migration into.

## 2. Source (Extraction)

This part describes where the data comes from. For this example, I will show how to read from a JSON file.

```yaml
source:
  plugin: url
  data_fetcher_plugin: file
  data_parser_plugin: json
  urls:
    - "public://migrate/demo.json"
  item_selector: organizations
  fields:
    - name: id
      label: ID
      selector: id
    - name: name
      selector: name
    - name: short_description
      selector: 'Short Description'
  ids:
    id:
      type: integer
```

### Breakdown of keys:

  * **plugin:** How to get data (url, embedded_data, table, etc.).
  * **data_fetcher_plugin:** Choice of `http` (fetch data online) or `file` (fetch data from a local file).
  * **data_parser_plugin:** Choices include XML, SimpleXML, JSON, Soap.
  * **urls:** Where the file is located.
      * *Note:* `public://` actually refers to the Drupal files path `/sites/default/files/`. So, in the example above, the system is reading from `/sites/default/files/migrate/demo.json`.
  * **item_selector:** The top-level key in the JSON which contains the data/list to migrate.
  * **fields:** This is the part you will add to iteratively.
      * `name`: The internal variable name we will use in the migration.
      * `selector`: The specific key in your JSON file.
  * **ids:** Specifically identifies which field makes each record unique (to avoid duplicates).

## 3. Process (Transformation)

This is where you will work the most—understanding plugins and mapping data.

For this guide, we will use **Direct Mapping**. We map the `id` to `field_id`, `name` to `title`, etc.

```yaml
process:
  field_id: id
  title: name
  field_short_description: short_description
```

### What is happening underneath?

When you do a direct mapping, Drupal is actually running the `get` plugin in the background. The code above is essentially a shortcut for this:

```yaml
# This is the long version of the code above
field_id:
  plugin: get
  source: id
```

## 4. Destination (Loading)

Finally, we define where to load the data into Drupal.

```yaml
destination:
  plugin: "entity:node"
  default_bundle: organisation
```

  * **plugin:** What kind of Drupal entity are we creating?
      * Node (Content Type): `entity:node`
      * Taxonomy term: `entity:taxonomy_term`
      * User: `entity:user`
      * Paragraph: `entity_reference_revisions:paragraph`
  * **default_bundle:** The machine name of the content type (in our case, `organisation`).

-----

I will stop here as this is getting long! In the next guide, I will show different migration commands, common mistakes I made, and how to debug them.

[< Back to Home](./index.md)