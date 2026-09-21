# Release notes

<!-- do not remove -->

## 0.1.0
Reshaped from PMI co-occurrence to an LLM typed graph. `build_graph` primes, extracts (`extract_typed`), writes (`graph_write`, `embed_entities`) and merges duplicates (`resolve_entities`).
`cites` and `norm_cite` seed citations. `graph_search` is patched onto litesearch's `Database`. Needs litesearch>=0.1.33.

## 0.0.2
vruksha release
