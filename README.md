# Handoff: M3A Query-Flow Telemetry

## Overview
A telemetry UI for the M3A RAG system (LangGraph on EC2). Three views of the same event stream: a live wall where each in-flight query is an "electron" travelling the graph, a per-query forensic log, and a candidate-funnel chart. `TELEMETRY_SPEC.md` defines what the backend must emit; this README defines what the UI must look like and do.

## About the design files
`M3A Query Flow.dc.html` is a **design reference built in HTML** — it shows intended look and behaviour with mock data. Do not ship it. Recreate the three views in the team's frontend stack (React or whatever the M3A UI already uses) against the read API in `TELEMETRY_SPEC.md` §6. Open the file in a browser to see it; it needs `support.js` and `ds/styles.css` beside it (included).

## Fidelity
High-fidelity for layout, type, colour and spacing. Data is mocked (query ids, users, scores); node names are the designer's assumptions and must be mapped to the real LangGraph node names.

## Design system: Modernist
Flat, architectural, single accent. Rules:
- Ground `#f3f2f2`, surface `#eae9e9`, ink `#201e1d`, accent red `#ec3013`. Neutral ramp: 300 `#d7d3d3`, 400 `#bab6b6`, 500 `#9b9797`, 600 `#7d7979`, 700 `#605d5d`, 800 `#444141`. Accent tints: 100 `#fff2ef`, 200 `#ffe0d9`, 700 `#ae1800` (use 700 for red body text).
- Font: Archivo (Google Fonts), weights 400 / 600 / 800. Monospace for payloads: `ui-monospace, Menlo`.
- **Zero border radius anywhere.** Strong 2px ink rules between major sections, 1px 40%-ink rules between rows. No shadows on these views.
- Everything flush left, including button labels. Uppercase 10px labels with `letter-spacing .08em` in neutral-700.
- Hover: accent-200 tint (`#ffe0d9` / `#fff2ef`). Focus: 2px accent outline, offset 2px.
- Icons: Lucide if any are needed.
- Accent is used only for: electrons, the selected node, latency bars, rule ids and loss captions.

## Views

### 1a — Live wall (`#1a`)
Layout: 1560px card, grid `1fr 330px` (stage | drawer). Header row 14×20px padding, 2px bottom rule: title "M3A · QUERY FLOW" (800/16px), a two-tab segmented control (Graph A · Markets / Graph B; active = ink fill, ground text; 11px/800 uppercase), right-aligned stats (in flight, today, p50 end-to-end, "time · log-scaled").

Graph stage: 1300×400px, centred. Wires are 2px ink SVG paths with right-angle elbows. Node centres (x,y):
- Ingress 50,165 · Router 190,165
- Fan-out at x=280 to four retrieval nodes at x=370, y = 40 / 120 / 200 / 280 (News BM25, News dense, Sell-side research, Tweets)
- Merge at x=515 into RRF 550,165 · Dedup 690 · Reranker 830 · Editorial 970 · Answer model 1110 · Response 1250
- Follow-up loop: dashed 1.5px neutral-600 path from Response down to y=372, back to Router. Caption "follow-up / re-query · 31% of answers".

Node: 56×36px box, 2px ink border, ground fill, 10px/800 abbreviation (IN, RX, B25, VEC, RES, TW, RRF, DD, RR, ED, LLM, OUT). Selected node: accent fill, ground text. Hover: accent-200 fill. Label under node (branch nodes: to the right, 36px from centre): 10px uppercase neutral-800, name, then a 3px accent bar whose width = `8·log10(ms+1)` px plus the latency figure in 800 weight.

Electrons: 10×10px squares (accent; every third ink) travelling the wire path. Timing: travel between consecutive points 0.45s constant; dwell at a node `0.35·log10(latency_ms+1)` s; fade out at Response. One electron per in-flight query; position = latest stage event. Tooltip on hover (ink fill, ground text, 10px): `query_id · user` / `time · in <stage> · in→out`.

Stat tiles under the stage: 4 equal columns, 2px top rule, 1px rules between: Router split · Pool→answer funnel · Editorial overrides · Answer model.

Drawer (330px, surface fill, 2px left rule): stage name (800/18px) + latency in accent; 2-col grid In / Out / Decision / Why (12px); "Payload · query_id" list in monospace 10.5px, rows `grid 22px 1fr 44px` (rank in accent, text ellipsised, score right-aligned neutral-700); pinned to bottom: the event schema block.

### 1b — Forensic swimlane (`#1b`)
1560px card. Header with title "M3A · QUERY LOG", date/count, and filter chips (1px ink border; active chip accent border + accent text). Column grid: `150 200 140 90 110 130 150 190 1fr` = Query · Router · Pool · RRF · Dedup · Rerank · Editorial · Answer · Latency (log). Rows 11px/1.4, 10×12px padding, 1px rules; hover accent-100. Router cell shows the regex rule id + pattern in monospace accent-700, then `→ ROUTE`. Latency cell: 10px-high segmented bar (neutral 400/500/700 + accent for answer model), segment widths ∝ log latency, total on the right in 800.
Expanded row (accent-100 background on the summary row, surface fill below, 2px bottom rule): 3 equal columns — Router why + pool queries sent · Rerank top-8 with `score · rank_before→rank_after` and editorial actions · Answer model (tokens, time, system/user/ctx lines, completion excerpt with 2px accent left rule, follow-up link).

### 1c — Candidate funnel (`#1c`)
1220px card, stage 1180×330. Stage columns at x = 130, 310, 490, 670, 850, 1030; 22px-wide ink bars, height = 2px per document, vertically centred; the first column is a stack of the four source lists (ink / neutral-800 / accent / neutral-500) with a right-aligned legend to its left. Bands between columns: cubic-bezier ribbons in neutral-300/`#c8c4c4`, with a thin accent-200 "loss" tail falling below each. Above each column: uppercase label + count (800/20px). Below: loss caption in accent-700 (`−13 near-duplicates merged (simhash ≥ .92)`).

## Interactions
- Node click → drawer updates (state: `selectedStage`). Electron hover → tooltip. Tab switch → graph B nodes (not designed; same vocabulary).
- 1b row click → expand/collapse; filters query the API.
- 1c is per query: reached from a 1b row or the 1a drawer.
- Time is log-scaled everywhere; label it.

## Data
See `TELEMETRY_SPEC.md`: SSE `/telemetry/live` for 1a, `/telemetry/queries` for 1b, `/telemetry/queries/{id}` for 1c and the drawer payloads via `/telemetry/payload?ref=`.

## Files
- `M3A Query Flow.dc.html` — the design (open in a browser). `support.js`, `ds/styles.css` — runtime + Modernist tokens it depends on.
- `TELEMETRY_SPEC.md` — backend/event spec.
- `ds/styles.css` — full Modernist token sheet and component classes; use it as the source of truth for colours and type.
