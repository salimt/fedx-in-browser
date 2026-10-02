# fedx-wasm

A workspace for running federated SPARQL queries in the browser. It uses [FedX](https://rdf4j.org/documentation/programming/federation/), the federation engine from Eclipse RDF4J, compiled to WebAssembly.

You choose which SPARQL endpoints to include, write a single query, and FedX works out which endpoint gets which part of it and joins the results. Wikidata, DBpedia and UniProt are set up by default. You can add your own.

**Link:** https://salimt.github.io/fedx-wasm/


## What it does

- Lets you toggle endpoints on and off per query, and add new ones (with an access token if the endpoint needs one)
- Has a query editor with tabs and saved queries, and checks the SPARQL with FedX before you run it
- Shows the results, the raw response, the execution plan and the requests that were sent to each endpoint
- Exports results, and exports or resets the whole workspace

## Using it

1. Open the page and add the endpoints you want in the federation.
2. Write a query. It's plain SPARQL, so no `SERVICE` clauses are needed.
3. Run it with the button or `Ctrl/⌘ + Enter`.

Example, with Wikidata and DBpedia selected:

```sparql
PREFIX dbr:  <http://dbpedia.org/resource/>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?person ?label WHERE {
  dbr:Douglas_Adams owl:sameAs ?person .
  ?person rdfs:label ?label .
  FILTER(LANG(?label) = "en")
}
LIMIT 10
```

"Show plan" displays how FedX intends to split the query before anything is sent. After a run, the "Remote requests" tab lists what each endpoint actually received.

## Limitations

- It depends on public endpoints, which can be slow or rate limited. A common failure is `Source selection has run into a timeout`. FedX asks every selected endpoint whether it can answer each part of the query, and one slow endpoint is enough to hit the limit. Unticking endpoints you don't need usually fixes it.
- An endpoint has to allow cross-origin requests from a browser, or it can't be queried from here.
- Everything runs inside a browser tab, so very large intermediate results can be a problem.
