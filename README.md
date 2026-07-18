# AI Concept Map

An interactive map of the 80 concepts you need to actually understand how modern large language models work — from byte-pair encoding to mesa-optimization — laid out as a graph of prerequisites rather than a flat glossary.

One HTML file. No build, no dependencies, no server.

## Why

Most AI explainers are either a glossary (definitions with no structure) or a course (structure you can't skim). Neither tells you *what you have to understand first*.

This is a dependency graph. Every concept links to the ones underneath it, grouped into 11 tiers where each tier assumes the one above. You can read it top to bottom as a curriculum, or drop into any node and walk backwards until you hit something you already know.

Each concept gets a one-line summary, a few paragraphs on why it earns its place, and links to its prerequisites, siblings, and the failure modes it explains. Nine of them have hand-drawn diagrams.

## Quick start

```bash
git clone https://github.com/MattModeCode/ai-concept-map.git
cd ai-concept-map
open index.html
```

Or just open [`index.html`](index.html) in any modern browser.

> [!TIP]
> Hosting it is equally simple — drop `index.html` on GitHub Pages, Netlify, or any static host. There is nothing to build.

## Using it

The map opens in graph view. Nodes are concepts, edges are conceptual dependencies, and colour clusters are categories.

| Action | Result |
| --- | --- |
| Drag | Pan the canvas |
| Scroll | Zoom |
| Click a node | Open its detail panel |
| `map` / `list` | Switch between the graph and a linear reading path |
| Search | Filter concepts by name |

**List view** is the same 80 concepts flattened into tier order — the recommended reading path if you're starting from scratch. Start at the top even if the later tiers are what you came for; a good half of the alignment material stops making sense without the training and interpretability tiers under it.

Filled nodes are the original core list; ringed nodes were added afterwards to fill in a missing prerequisite, a sibling worth knowing, or an outright gap.

## What's covered

Eleven tiers, roughly in dependency order:

1. **Foundations** — tokenization, cross-entropy, attention, the residual stream, logits, sampling, scaling laws
2. **Modern architecture** — RoPE, RMSNorm, SwiGLU, GQA/MQA, FlashAttention, MoE, state-space models
3. **Training dynamics** — AdamW, Shampoo, Muon, LR schedules and muP, double descent, grokking
4. **Adaptation** — taking a trained model and making it yours, cheaply
5. **Post-training & RL** — where a text predictor becomes an assistant, and where most alignment failures are born
6. **Inference & serving** — the half of the field that is about memory bandwidth, not intelligence
7. **Interpretability** — reading the model rather than testing it
8. **Alignment theory** — arguments mostly older than LLMs, now getting empirical results attached
9. **Evaluation** — how you know anything, and the reasons you might not
10. **Adversarial & security** — the failure modes with an attacker attached
11. **Agent patterns** — what happens when the loop closes and the model can act

Cross-cutting categories: architecture, training dynamics, post-training, inference & serving, interpretability, alignment theory, evaluation, adversarial, agent patterns.

## How it works

Everything lives in [`index.html`](index.html) — content, force-directed layout, canvas renderer, and styles.

- **Data layer** — a single `C` array of concept objects (`id`, `name`, `cat`, `tier`, `one`, `body`, `links`, `diagram`), plus `CATS` and `TIERS` lookup tables.
- **Layout** — a small force simulation seeds nodes near their category anchor, then relaxes edge springs and node repulsion into a stable arrangement.
- **Rendering** — plain Canvas 2D with manual pan/zoom transforms. No graph library.

### Adding a concept

Append an object to the `C` array:

```js
{ id:'speculative', name:'Speculative decoding', cat:'infer', tier:6, yours:false,
  one:'a small model drafts, a big model checks',
  body:`...`,
  links:['kvcache','sampling'], diagram:null },
```

Links are bidirectional — listing `kvcache` here draws the edge from both sides. The node count in the header updates itself.

## Accessibility

Keyboard-navigable controls with visible focus rings, ARIA labelling on the view toggle, search, and detail panel, and a list view that works as a full text alternative to the canvas.
