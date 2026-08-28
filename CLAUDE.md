# Working in this repo

nbdev. The notebooks under `nbs/` are the source; `vruksha/*.py` is generated. Edit the notebook,
run `nbdev_export`, never edit the `.py`. CI runs `nbdev_export` and fails on a diff.

## The line with litesearch

litesearch owns the tables. `get_graph` creates entities, mentions and edges, and it lives in
`litesearch.topics` because `topic_nodes` writes to them too. vruksha owns the algorithms over
those tables.

vruksha depends on litesearch, never the reverse. `Database.context(graph=True)` is litesearch's
seam and raises `ImportError` naming this package when it is not installed.

## Three modules, in dependency order

`entities` extracts and normalises. `build` builds and resolves. `search` walks and fuses. Keep
that order: `build` imports from `entities`, `search` imports from `build`.

## The graph leg is measured, and it loses on some corpora

`graph_w` defaults to 0.5 and `graph=` is off. The numbers are in the README and in litesearch's
`evals/RESULTS.md`. Do not turn it on by default without a new measurement.

## What must not be re-added

`hash_embed` is `litesearch.utils`. `rrf_all` is `litesearch.core`. `topic_nodes`, `clusters` and
`peers` are `litesearch.topics`. Copying any of them here restarts the fork this split ended.

## Prose in notebooks

Short. Lead with what the code does. Numbers instead of adjectives. No em dashes, no bold inside
a paragraph, no rhetorical questions. A rationale longer than three sentences belongs in a
docstring.

## Docstrings and comments

One line. A second sentence only for a measured number or a footgun. Inline comments in a `def`
signature are nbdev docments and become the API parameter table.
