# ALGORITHMS DATA STRUCTURES GRAPH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 çizge (⟷ graph ⟷ tr:graf ⟷ çizit)

- Any `node` may have a connection to any `node` (even circular).
- For example: `A` `node` can be connected to 10 other `nodes`. In the same `graph` another `node` may connect only 1 `node`.

Some terms:

- `vertex (plural vertices) ⟷ node (⟷ düğüm)`: `vertex`; `tepe noktası` anlamına gelir.
- `edge`: Each connection of a specific `node`.
- `Order`: The number of `vertices` in the `graph`.
- `Size`: The number of `edges` or `node` in the `graph`. There is no standard of this.
- `vertex degree (⟷ düğümün derecesi)`: number of `edges` of a `vertex`.
- `isolated vertex`: a `vertex` which does not have any connections(`edges`) to other `vertices`.
- `self loop`: an `edge` from `vertex` to itself.
- `undirected graph`: In this type of graph there is no direction information for `edges`. In another word; if `X` `node` have connection to `Y` `node`, `Y` `node` also must have connection to `X`. The opposite `graph` type is `directed graph` which has direction information.

## 📌 komşuluk matrisi (⟷ adjacency matrix)

This takes more memory. But it is fast to find the connections.

```text
   A B C

A  0 1 0

B  1 1 1

C  0 1 1
```

## 📌 adjacency list (⟷ adjacency list)

Requires low memory. But it takes long time to find the relations between `node`s.

```text
A -> {B}

B -> {C, D}
```

The above `adjacency list` means:

- There is a connection from `A` to `B`.
- There is a connection from `B` to `C` and `D`.

## 📌 real use case examples of graph

- storing the physical border connections between countries and cities.
- Storing the list of friend connections on social media web sites
- connections of between atoms or molecules (for simulation)
- connections between devices for electronic circuit (for simulation or on engineering design tools)
- connections of each part on puzzle game
- `Google`'s `PageRank` algorithm. In this algorithm, `Google` stores which web page has linked by which web pages. If a web page has reference (link) inside other web pages, that means the web page reputation is high.
