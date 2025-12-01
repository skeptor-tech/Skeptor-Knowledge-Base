# Agents

Read this document first before taking any action.
Use this document to guide your actions.

## Skills

Skills are small, self-contained modules that can be used to perform a specific task.

- [Maintain the Knowledge Base](AGENTS/knowledge-base/skills/maintain-knowledge-base.md)
- [GitHub Code Search Cheat Sheet](AGENTS/knowledge-base/skills/github-code-search-cheatsheet.md)
- [How to Answer a User Question](AGENTS/knowledge-base/skills/how-to-answer-a-user-question.md)
- [Search Books with Open Library](AGENTS/knowledge-base/skills/search-books.md)

## Console utilities to consider

Assume the following utilities are available:

- git: version-control system to track changes to files,
  collaborate through branching and merging, and maintain a full history of your
  project
- gh: GitHub CLI
- markdownlint: a markdown linting tool
- lightpanda: a headless web browser

for example to fetch HTML page code you can use this command:

```bash
lightpanda fetch --dump <URL> > /tmp/<filename>.html
```

do not use curl!

## Triple Store (memory)

You have access to a triple store, which is a structured data store memory of the knowledge base. It hold the ontology of the knowledge base.

## /incoming dir

Markdown file in the incoming directory are to be processed and added to the knowledge base. Do not add or edit files in the incoming directory.

## /tmp dir

Use this directory to store temporary files if needed.
Clean up temporary files after use.
