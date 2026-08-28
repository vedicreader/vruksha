# vruksha

An entity graph over a litesearch store. Extraction, resolution, and a PageRank leg beside FTS
and vectors. No LLM anywhere.

```python
from litesearch import database
from vruksha import build_graph, resolve_entities

db = database('corpus.db')
build_graph(db, rows, emb_fn=enc)     # entities and co-occurrence edges
resolve_entities(db)                  # `hnsw` and `HNSW index` become one node
db.graph_search('how does resolution work', qemb, graph_w=0.5)
```

## Four problems, one of which wants a model

| problem | how it is solved here |
|----|----|
| what the entities are | AST symbols for code, yake keyphrases for prose |
| which mentions are the same thing | embeddings proposing, a lexical guard deciding |
| what connects to what | PMI over co-occurrence windows |
| what to do with the graph | personalised PageRank, fused as a third search leg |

The lexical guard is the part worth knowing about. Embedding similarity alone happily merges
`python 3.11` into `python 3.12`. `_lex_ok` requires token overlap, matching digits and a
matching acronym before a merge goes through.

## When to turn the search leg on

Measured against plain hybrid search (`evals/run.py eval_graph` in litesearch):

- **Regulation and legal text: a loss.** p_mrr 0.8170 for plain hybrid against 0.7395, 0.6859 and
  0.6463 at `graph_w` 0.25, 0.5 and 1.0, at two to four times the latency.
- **Papers and prose: a win.** Better in seven of nine paired-bootstrap comparisons, +0.0387 target
  MRR on arXiv at `graph_w=1.0`.

So it is opt-in by name, and off by default. Turn it on for a corpus whose entities carry
meaning, and raise `graph_w` towards 1.0 when you do.

## Install

```
pip install vruksha
```

`topic_nodes`, `clusters` and `peers` are not here. They run by default and need no graph walk, so
they stayed in `litesearch.topics` with the tables they write to.
