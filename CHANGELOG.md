# Release notes

<!-- do not remove -->

## 0.1.0
vruksha is an LLM typed-graph package, no longer PMI co-occurrence. `build_graph` primes the model with the entities already there, extracts typed entities and relations per chunk (`extract_typed`), writes them (`graph_write`, `embed_entities`), and merges the duplicates that slip through (`resolve_entities`). `cites` and `norm_cite` seed structured citations. `graph_search` is patched onto litesearch's `Database`. Needs litesearch 0.1.33.

## 0.0.2
vruksha release
