# Working in this repo

nbdev. The notebooks under `nbs/` are the source; `vruksha/*.py` is generated. Edit the notebook,
run `nbdev_export`, never edit the `.py`. `README.md` comes from `nbs/index.ipynb` through
`nbdev_readme`. CI runs `nbdev_export` and fails on a diff.

## Dependency direction

vruksha imports litesearch. Never the reverse: a vruksha import inside litesearch is a cycle.
litesearch owns the entity, mention and edge tables; vruksha owns the algorithms over them.

## Three modules, in order

`entities` extracts and normalises. `build` builds and resolves. `search` walks and fuses.
`build` imports from `entities`, `search` imports from `build`. Keep that order.

## The graph leg loses on some corpora

`graph_w` defaults to 0.5 and `graph_search` is opt-in by name. The numbers are in the README.
Do not turn it on by default without a new measurement.

## Prose in notebooks

Short. Lead with what the code does. Numbers instead of adjectives. No em dashes, no bold inside
a paragraph, no rhetorical questions. A rationale longer than three sentences belongs in a
docstring.

## Docstrings and comments

One line. A second sentence only for a measured number or a footgun. Inline comments in a `def`
signature are nbdev docments and become the API parameter table.
