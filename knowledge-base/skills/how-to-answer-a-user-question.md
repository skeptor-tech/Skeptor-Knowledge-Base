# How to Answer a User Question

Use URLs mentioned in the KB (APIs, RSS feeds, search endpoints) to get information. Do not make up URLs!!! only use URLs available in the KB. Provide answer with footnote references to sources. Use only URLs available in the KB.

## Some Rules

- Do not edit KB while answering a user question.

## Preparation

- **Start in the KB**: Search `knowledge-base` with `rg "<keyword>"` and
  scan `index.md`, domain `topic_index.md` files, and tagged hubs to understand
  what is already documented.
- **Sparql Queryable Memory**: Explore semantic relationships and retrieve structured data by querying SPARQL memory tool and relevant public endpoints (e.g., Wikidata https://query.wikidata.org/).

For local KB data use memory tool:
```sparql
PREFIX m: <http://example.org/memory#> # KB entities prefix 
PREFIX g: <http://example.org/ontology/> # KB graphts prefix
CONSTRUCT {
    ?person a m:Person .
    ?person m:fullName ?fullName .
}
FROM g:abox # graph for schema
FROM g:tbox # graph for data
WHERE {
    ?person a m:Person ;
            m:firstName ?firstName ;
            m:lastName ?lastName .
    BIND(CONCAT(?firstName, " ", ?lastName) AS ?fullName)
}
```
You can use remote endpoints using SERVICE keyword
```sparql
PREFIX ...

SELECT ...
WHERE {
  ...
  SERVICE <https://query.wikidata.org/sparql> {
    ...
  }
}
```
two external endpoins known for now:
https://dbpedia.org/sparql
https://query.wikidata.org/sparql

- **Collect context**: Note existing conclusions, definitions, and links so the
  reply can cite them directly (use `relative/path.md:line` references when
  available). Note available API, RSS feeds, search URLs, etc. if available (look for # API & sources section)
- **Enhance context with external information**: Utilize the KB's resources such as APIs, RSS feeds, search endpoints, and SPARQL endpoints to gather more information.
- **Finally**: only provide an answer if required info is in the KB or was found in a reliable source, otherwise deny the request.

## Use the Knowledge Base First

- Quote or paraphrase only after verifying the source note; link back to the
  precise file so readers can trace the reasoning.
- Prefer synthesizing across multiple KB notes or external sources rather than copying one entry
  verbatim; highlight agreements, contradictions, and gaps, provide references as footnotes.

## Extend with APIs, RSS, and Search

- **APIs**: Use documented service APIs (REST, GraphQL, CLI) to fetch the
  latest values—capture the endpoint, request shape, timestamp, and key
  response fields in the answer.
- **RSS/Atom feeds**: Pull recent items when tracking news releases, changelogs,
  or blog updates; surface the publication date and link.
- **Search endpoints**: Run targeted searches (web, code, issues) only after the
  KB review, and summarize the incremental insight they provide.
- **SPARQL endpoints**: Construct queries for semantic databases (e.g., Wikidata)
  to answer complex questions about relationships, categories, or properties.
- Always clarify which insights come from external calls vs. the KB, and record
  enough detail for someone else to reproduce the lookup.

## Quality Checks

- Cross-verify facts between KB and fresh data; note discrepancies explicitly.
- Keep formatting concise (short paragraphs, bullets for multi-part answers).
- Ensure the final message stands alone—no references to "the above" or hidden
  context—so it can be pasted elsewhere without confusion.

## Compose the Response

- Lead with the direct answer or recommendation.
- Attribute supporting statements to KB notes first (paths + key lines).
- Explicitly cite where each key fact came from with footnotes, using KB paths
  (with line numbers when possible) or the external source URL.
- Append external findings with source names, query/run info, and timestamps.
- Provide footnote references to external sources and KB notes with relevant
  URLs.
