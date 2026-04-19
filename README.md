# git-lex-kit-autoknow

AutoKnow: automated knowledge organization from unstructured sources.

## What it does

AutoKnow extracts structured entities and relationships from raw documents
(conversations, PDFs, notes, etc.) using subagent-driven extraction. The
output is a typed knowledge graph of interlinked entity pages.

## Document types

| Type | Description |
|------|-------------|
| **Source** | A raw input document wrapped as markdown. Frozen after creation. |
| **Entity** | An auto-generated page for a discovered entity (person, project, tool, etc.). |

## Installation

```bash
git lex init --kit autoknow
```

## Pipeline

1. Drop source documents into `AutoKnow/Source/`
2. Subagents extract typed SPO triples with line-number provenance
3. Pipeline aggregates triples, generates entity pages with descriptions
4. `git lex save` commits and loads triples into the store
5. Query with `git lex query` or browse via viz

## Key design principles

- **Sources are frozen** — never edited after creation, providing stable provenance
- **Classes emerge from extraction** — not predefined. Person, Project, Tool, etc. are discovered by the extractor, not hardcoded in the ontology
- **Deterministic scaffolding + LLM descriptions** — structural data (frontmatter, relationships) is pure data transformation; only the description paragraph requires an LLM
- **Provenance per triple** — every fact traces back to a Source file and line number
- **Trust level: best-effort** — AutoKnow output is subagent-generated and may contain errors

## Kit contents

```
ontology/autoknow/autoknow.ttl   — OWL ontology (Source + Entity classes)
content/AGENTS.md                — Agent instructions
content/AutoKnow/Source/         — Where source documents go
content/AutoKnow/Entity/         — Where generated entity pages go
```
