# Commit Msg Rules

##### <p style="color:">Template</p>

`type`: `subject`

`body`

`footer`

##### Details

`type`:This indicates the type of change being made. It can be one of the following:

- **feat:**

  - indicates the addition of a new feature or functionality to the project. This type of commit is used to highlight changes that introduce new capabilities or improvements to the software.


- **fix:**

  - Bug fixes


- **docs:**

  - Documentation changes


- **style:**

  - Changes that are purely aesthetic and do not impact the logic or output of the code.


- **refactor:**

  - A "refactor" commit involves restructuring or improving the existing code without changing its external behavior.
  - It focuses on improving the codebase's structure, organization, or readability without introducing new functionality or fixing bugs.
  - efactoring commits aim to make the codebase cleaner, more maintainable, or more efficient while preserving its functionality.


- **chore:**
(choreography)
  - A "chore" commit typically refers to changes that are administrative or housekeeping in nature.
  - It includes tasks like **updating dependencies**, **configuring build tools**, fixing formatting or coding style issues, or making changes that don't affect the behavior of the code.
  - Essentially, "chore" commits are for changes that are necessary for maintaining the project but don't directly relate to adding new features or fixing bugs.


- **config:**

  - A "config" commit typically refers to changes related to configuration settings or files within the project.
  - It includes modifications to configuration files such as .gitignore, .editorconfig, environment configuration files, or any other settings that affect how the project behaves or is built.
  - Essentially, "config" commits are for changes that configure or customize the project's environment or behavior.

  



- **test:**

  - Adding or modifying tests
   
   
   
`subject`: A brief summary of the changes in present tense, typically around 50 characters or less. It should be clear
and descriptive.

`body`:
A more detailed description of the changes. This section can include why the change was made, any relevant context, and
how it affects the codebase. It's optional but encouraged for more complex changes.

`footer`:
Additional information, such as references to issue trackers (e.g., "Closes #123"), breaking changes, or co-authored-by
information.
