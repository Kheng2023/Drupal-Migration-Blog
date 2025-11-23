# Part 2: Let’s Get Started

In this section, we will look at creating Content Types, Fields, and navigating the migration interface.

## 1. Installation

Before starting to migrate, we first need to install the Drupal Core Migrate and 2 additional extensions:
1.  **Migrate Plus:** Enables us to migrate from files like JSON and group the migrations.
2.  **Migrate Tools:** Enables migration CLI drush commands (like `migrate:status`, `migrate:import`) and the Web UI.

**Run these commands in your terminal:**

```bash
ddev ssh
composer require migrate_plus migrate_tools
drush en migrate migrate_plus migrate_tools
```

## 2\. Create Content Type & Fields

To migrate data into Drupal, we first need to create a content type and the field where the data will be stored.

Let’s start by creating an **"Organisation"** content type with some sample fields to demonstrate.

  * **Name:** We do not need to create a field called "name" because that will be saved in the default `Title` field in Drupal.
  * **ID:** Number field.
  * **Short Description:** Text (long).

*Note: I will be using some sample data of 5 random companies that I asked AI to create for me in JSON format.*

## 3\. Importing your Migration YAML

We need YAML code to migrate the data (I will explain the YAML details in the next guide). This step is about **where** to paste that code.

1.  Go to your Drupal development website.
2.  Navigate to Drupal admin: **Configuration → Development → Configuration Synchronization**.
3.  Select the **‘Import’** tab.
4.  Select the **‘Single Item’** tab.
5.  In the dropdown for *Configuration Type*, select **Migration**.
6.  Paste your YAML code into the box below.
7.  Click **‘Import’** and **‘Confirm’**.

*Note: This is my prefered way of importing the migration YAML. I believe there are other methods to import migration but it was too confusing for me.*

## 4\. How to Execute the Migration

There are two methods to execute the migration:

### Method A: The UI (User Interface)

1.  Navigate to Drupal Admin: **Structure \> Migrations**.
      * *Note: It may take a little while for the migration to appear; you can try to rebuild the cache.*
2.  Click the **‘List Migration’** button.
3.  Click the **‘Execute’** button.
4.  Click another ‘Execute’ button on the next page.

### Method B: The CLI (Terminal)

1.  Go to your terminal.
2.  Enter the following command:
    ```bash
    ddev drush migrate:import [migration_id]
    ```

That’s all for today. I will teach more about different migration CLI commands and how to write the migration YAML in future guides\!

[\< Back to Home](https://www.google.com/search?q=./index.md)

