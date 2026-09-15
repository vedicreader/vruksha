# Working in this repo

nbdev. The notebooks under `nbs/` are the source; `vruksha/*.py` is generated. Edit the notebook,
run `nbdev_export`, never edit the `.py`. `README.md` comes from `nbs/index.ipynb` through
`nbdev_readme`. CI runs `nbdev_export` and fails on a diff.

## Dependency direction

vruksha imports litesearch. Never the reverse, in code or in `pyproject.toml`: litesearch
naming vruksha in any dependency group, dev included, is a cycle.
litesearch owns the entity, mention and edge tables; vruksha owns the algorithms over them.

## Three modules, in order

`entities` extracts (LLM) and guards merges. `build` writes, primes, embeds and resolves. `search` walks and fuses.
`build` imports from `entities`, `search` imports from `build`. Keep that order.

## Model-agnostic by injection

vruksha names no model. `build_graph` takes `chat` (any `.oneshot(prompt, sp=, max_tokens=)`) and
`emb_fn` from the caller; rishi/urai/an API client live there, not here. Do not add a model or
`rishi` to `pyproject.toml`.

## The graph is typed, not co-occurrence

Edges are LLM-extracted relations (`refers_to`, `cites`, `defines`, ...), not PMI. The PMI graph
measured negative for retrieval; the typed graph reaches cross-reference bridges hybrid cannot.
Numbers live in litesearch `evals/RESULTS.md`. `graph_search` is opt-in by name; do not wire it
into a default retrieval path without a new measurement.

## The lexical guard is load-bearing

`resolve_entities` merges duplicate entities, but only pairs that pass `_lex_ok`: token or acronym
overlap AND identical numbers, so `python 3.11` never eats `3.12`. Priming reduces how much it has
to do; keep the guard conservative.

## Prose in notebooks

Short. Lead with what the code does. Numbers instead of adjectives. No em dashes, no bold inside
a paragraph, no rhetorical questions. A rationale longer than three sentences belongs in a
docstring.

## Docstrings and comments

One line. A second sentence only for a measured number or a footgun. Inline comments in a `def`
signature are nbdev docments and become the API parameter table.
