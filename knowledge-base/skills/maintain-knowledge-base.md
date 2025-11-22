# Maintain the Markdown File-based Personal Knowledge Base

Best practices to keep the `knowledge-base/` directory healthy, fully
traversable from `index.md`, and easy to extend.

## Core Structure

- **Folder-based hierarchies**: Represent domains and subtopics with nested
  folders (`domain/topic/subtopic/.../article_title.md`). Each directory holds a
  `topic_index.md` describing the folder and linking to scoped articles.
- **Dedicated entry points**: Keep a root `index.md` and a `topic_index.md` in
  every directory synced with the folder contents. From the top-level index,
  users should reach any topic within three hops.
- **Index notes as maps**: Create `index-topic.md` style maps for rich areas to
  fan out into related material (PARA sections, tag hubs, project dashboards).

## Workflow: Add or Update a Note

1. **Search first**
   - Run `rg "keyword"` in `knowledge-base` and inspect tag or domain
     indexes to avoid duplicates.
   - Reuse or expand an existing note when possible.
2. **Choose the hierarchy**
   - Place the new note in the folder that best matches the concept.
   - Create new folders only when the idea does not fit an existing branch.
3. **Prepare the note**
   - Pick a clear title and ensure tags remain lowercase kebab-case.
   - Start with an intro paragraph, then break down content with `##` headings.
4. **Link aggressively**
   - Add forward links to related notes, tag hubs, project dashboards, and
     glossary entries.
   - Include a `## See also` section when directly highly related material exists, related mean have very similar or overlapping context and are helpful for the reader to understand the context of the current note.
5. **Backlink sweep**
   - Visit linked notes and add reciprocal context where it improves navigation.
6. **Publish to the graph**
   - Update affected index pages (tag hubs, project maps).
   - Run a quick orphan check so the note is not isolated.

## Crosslinking & Discovery

- **Links everywhere**: Use links to keep subtopics reachable. Include `#heading`
  fragments to point at specific sections when helpful.
- **Bidirectional visibility**: Monitor backlinks after each change. High-traffic
  pages should reference new content so navigation never dead-ends.
- **Orphan sweeps**: Regularly run Foam or Obsidian orphan detectors (or
  `rg -g "*.md" --files` with link reports) to eliminate notes without inbound
  or outbound links.
- **Graph audits**: Use graph visualizers to inspect connectivity and spot
  clusters that need bridges.

## Tagging Strategy

- **Reuse before inventing**: Search (`rg '#tag'`) or browse tag hubs before
  adding new tags.
- **Hierarchical tags**: Follow `#domain/subdomain/topic` patterns so tags mirror
  the folder tree and remain filterable.
- **Front matter source of truth**: Maintain tag lists in YAML front matter for
  deterministic parsing and automation.
- **Tag hubs**: Maintain dedicated pages for major tags with descriptions,
  entry links, and related resources. Promote heavily used tags into full
  domain sections when warranted.

## For articles

- If applicable, add a BibTeX citation or other academic attribution.
- If code is available, add a link to the repository and include the `code` tag.
- Add important highly related notes to a `## See also` section.

## For Web Resources

- When asked to add a web resource, explore it first to prepare a brief
  description and appropriate tags. Explore if a resource provides an automated ways to access the existing or new information, via RSS for example or via providing API or a search function. If so add `## API's & info access sources` section to the resource note providing a link and instructions on how to use it, for example:

  ```bash
  curl -s "https://res.domain/?s=[keyword or phrase]"
  ```

## Navigation & Index Health

- **Top-down reachability**: `index.md` should list domains, tag hubs, and key
  projects with one-line descriptions. Every note must link upward to at least
  one of these hubs.
- **Sideways links**: Encourage lateral navigation (related concepts, opposing
  viewpoints, alternative workflows) to create a resilient mesh.
- **Refactor-friendly structure**: Organize content so moving or renaming
  folders cascades logically; keep filenames short and let directories express
  the hierarchy.
- **Consistency checks**: After major edits, run `rg "\[\[.*\]\]"` or use link
  checkers to catch broken references.

## Maintenance Rituals

- **Always**: Use markdownlint utility to check for errors and warnings in markdown
  files.
- **Check Consistency**: If the user asks to check a note’s consistency, that means the relevant instructions from this document must be applied, including topic-specific instructions like [For Web Resources](#For-Web-Resources) or [For articles](#For-articles).
- **Weekly**: Sweep for orphans, resolve TODO stubs that should now be full
  notes, and confirm new backlinks appear.
- **Monthly**: Refresh `See also` sections on high-traffic notes, prune obsolete
  tags, and regenerate tag hub listings.
- **Quarterly**: Revisit domain names, templates, and index layouts; refactor
  branches that grew unwieldy; ensure `index.md` still routes cleanly.
- **After any refactor**: Verify backlinks updated correctly, spot-check links,
  and ensure tag hubs still collect the right notes.

Consistently applying these patterns keeps the knowledge base local-first,
git-friendly, and explorable from any starting point.
