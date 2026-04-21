# AutoKnow — Agent Instructions

AutoKnow is an automated knowledge organization kit. It extracts structured
entities and relationships from unstructured source documents using subagents.

## How it works

1. **Sources** — Drop raw documents into `AutoKnow/Source/`. Each Source is a
   markdown file wrapping the original content (conversation JSON, document
   text, etc.). Sources are frozen after creation — never edit them.

2. **Extraction** — A subagent reads each Source and emits SPO (subject-
   predicate-object) triples as a `.spo` sidecar file alongside the Source.
   Triples use typed-path format: `Class/instance | predicate | value`.

3. **Entity pages** — The pipeline aggregates triples across all Sources and
   generates one page per discovered entity at `AutoKnow/Entity/{name}.md`.
   Each page has typed frontmatter, a description, and cross-links.

## Using git-lex with AutoKnow

- **Add a source:** Place a `.md` file in `AutoKnow/Source/`
- **Run extraction:** The harness triggers subagent extraction automatically
  (or manually via the pipeline scripts)
- **Save:** `git lex save "message"` — commits, extracts frontmatter, syncs
- **Query:** `git lex query "SPARQL..."` — query entities and relationships

## Frontmatter format

Source files:
```yaml
autoknow.Source.sourceId: "uuid-or-identifier"
autoknow.Source.sourceType: "conversation"
autoknow.Source.sourceName: "Human-readable title"
autoknow.Source.dateCaptured: "2024-12-09T05:56:11"
autoknow.Source.messageCount: 28
```

Valid `sourceType` values: `conversation`, `document`, `webpage`, `chat`, `email`, `note`.

Entity files:
```yaml
autoknow.Entity.entityId: "rob"
autoknow.Entity.entityType: "Person"
autoknow.Entity.entityDescription: "A 2-4 sentence factual description."
autoknow.Entity.mentionCount: 117
```

## SPO sidecar format

One triple per line, pipe-delimited:
```
Class/subject | predicate | Class/object | source_file.md:line
Class/subject | predicate | plain-string-value | source_file.md:line
```

- Subjects and relationship objects use typed-path: `Person/rob`, `Project/pixeltable`
- Property values are plain strings (no `Class/` prefix)
- Predicates are camelCase: `worksAt`, `hasName`, `createdBy`
- Provenance column references the Source file and line number

## Trust level

AutoKnow-generated content is **best-effort, subagent-produced**. It may
contain errors, type drift, or duplicate entities. A Soul may reference
AutoKnow entities in its own ontology but should not treat them as
authoritative without review.
