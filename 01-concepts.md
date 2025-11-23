# Part 1: Key Concepts and Advice

Before we start, I want to cover some terminology and advice. This is based on my experience migrating for SAcommunity.org.

## Terminology

Here are some frequently used terms I’m going to use:

* **Content Type:** The bread and butter of the Drupal Content Management System; this is where our data will be migrated. Think of a content type as an **Object** from a Computer Science / OOP point of view.
* **Field:** Sort of the attributes or information about the content type/object/entity. Each field has to predefine its own characteristics (e.g., text, number, email, etc.).
* **YAML:** The file we need to write the Drupal migration instructions. Throughout the migration, you will be editing the YAML file constantly until you successfully migrate the whole database.
* **UI (User Interface):** Using the web browser Drupal program to handle work.
* **CLI (Command Line Interface):** Or "terminal." I will use the UI and CLI interchangeably. It is possible to run all migrations on the UI, but I sometimes prefer the CLI because it is much quicker (just my opinion).

## Lessons I Learned (and my advice)

### 1. Do not depend on LLMs like ChatGPT or Gemini too much
In the beginning, I just asked the AI to write the whole YAML for me — just basic human laziness, trusting it would do the job.

Even after defining very clearly how my JSON is structured, how I structured the content type, and what I wanted to migrate, the AI failed. Prompt engineering is not the issue; the existing knowledge in these LLMs about Drupal is extremely limited.

### 2. Use the official documentation
Try searching for websites that teach Drupal migration properly. Of course, I am going to teach what I learned here, but these are highly recommended:
* [31 days of Drupal migrations](https://understanddrupal.com/courses/31-days-of-migrations/)
* [Drupal Migrate API documentation](https://www.drupal.org/docs/drupal-apis/migrate-api)
* [List of migrate process plugins](https://www.drupal.org/docs/8/api/migrate-api/migrate-process-plugins/list-of-migrate-process-plugins) (I used this constantly to understand which plugins to use).

### 3. Start small
**Start migrating one field** to test your migration YAML, and continue adding more as each field is successfully migrated.

I initially wrote migrations for many fields right at the beginning and was confused about where it went wrong. It was hard to debug because you don’t know if the error is from the JSON, the process part of the migration, or the SQL database limits.

### 4. MySQL Workbench is your friend
It’s optional software. Initially, I did not know why we were installing this, but it ended up being my friendly companion to:
1.  **Understand sub-fields:** e.g., for a link-type field, there are subfields for the URI and another for the title.
2.  **Verify success:** After I execute the migration, I will run a simple `SELECT * FROM [field_name]` SQL command to see if the table is populated.

[< Back to Home](./index.md)