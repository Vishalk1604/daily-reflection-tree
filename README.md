# The Daily Reflection Tree

A deterministic end-of-day reflection tool for employees — built entirely without AI at runtime.

Every question has fixed options. Every option leads to a known next node. The same answers always produce the same conversation. No LLM. No free text. Just a well-designed tree.

---

## What It Does

The tool walks an employee through three psychological axes at the end of their workday:

| Axis | Name | Poles | Grounded In |
|------|------|-------|-------------|
| I | **Locus** | Internal ↔ External | Rotter (1954), Dweck (2006) |
| II | **Orientation** | Contribution ↔ Entitlement | Organ (1988), Campbell et al. (2004) |
| III | **Radius** | Self ↔ Team ↔ Other ↔ Beyond | Maslow (1969), Batson (2011) |

Each axis has two layers of questions, an adaptive bridge, and a reflection node. At the end, the tree synthesizes signals from all three axes into a personalized summary — built entirely through string interpolation, not generation.

The tree has **40+ nodes** across 7 node types: `start`, `question`, `decision`, `reflection`, `bridge`, `loopback`, `summary`, and `end`.

---

## Repository Structure

```
daily-reflection-tree/
│
├── README.md
│
├── tree/
│   └── reflection-tree.json     # The full decision tree
│
├── diagram/
│   └── tree-diagram.mmd         # Mermaid diagram of all nodes and edges
│
├── agent/
│   └── index.html               # Part B: working single-file web UI
│
├── transcripts/
│   ├── transcript-victor.md     # Sample path: Internal 
│   └── transcript-victim.md     # Sample path: External 
│
└── writeup/
    └── design-rationale.md      # 2-page design rationale and citations
```

---

## How to Run (Part B)

No server required. No dependencies. No build step.

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/daily-reflection-tree.git

# Open directly in your browser
open agent/index.html
```

Or just double-click `agent/index.html`. It runs entirely in the browser.

---

## How the Tree Works

### Node Types

| Type | Role |
|------|------|
| `start` | Opening screen, routes to first question |
| `question` | Presents 3–5 fixed options; each option carries a `signal` and a `next` pointer |
| `decision` | Invisible routing node — evaluates accumulated signals and forwards to the correct next node |
| `reflection` | Displays a written reflection; records a final signal for the axis |
| `bridge` | Adaptive transition between axes — text changes based on the dominant signal of the previous axis |
| `loopback` | Revisits the opening answer after all three axes; asks if the word still fits |
| `end` | Final screen |

### Signal Accumulation

Every option a user selects carries an optional `signal` field, e.g. `"axis1:internal"`. The engine counts these signals per axis. When a `decision` node evaluates `signal_dominant`, it looks at which pole has the higher count and routes accordingly.

This means the tree is **not** a simple flowchart. Even if a user selects one "external" answer early on, a subsequent "internal" answer can shift the dominant signal and change the branch they land on.

### Cross-Axis Insights

After Axis III, a `CROSS_AXIS_D1` decision node evaluates the *combination* of the Axis I and Axis II dominant signals. Three specific combinations produce unique reflection nodes not reachable any other way:

- `internal + contribution` → `RARE_COMBO_INTERNAL_CONTRIB`
- `external + entitlement` → `EDGE_CASE_EXTERNAL_ENTITLE`
- `internal + entitlement` → `TENSION_POINT_INTERNAL_ENTITLE`

### Summary Generation

The final summary is built by interpolating from `summaryTemplates` in the JSON using the three dominant signals. It references the user's actual opening word (`{A1_OPEN}`), the Axis I signal, the Axis II signal, and the Axis III signal — producing a closing that reads differently for every distinct path through the tree.

---

## Design Decisions

**Why no free text?**
The brief required full determinism. Every response must be reproducible. Free text would require an LLM to interpret — which defeats the point.

**Why not moralize?**
"External locus" and "entitlement" are not failure states. The reflection nodes on those paths are written to surface insight, not shame. A user who feels like today happened *to* them deserves a different kind of honesty than one who feels on top of their agency — not a lecture.

**Why three axes and not one?**
A single axis (e.g. just locus of control) misses the interaction effects. Someone can have high internal locus *and* still be entitlement-oriented. The cross-axis nodes exist specifically to name those contradictions — because that's where the most useful psychological information lives.

---

## Psychology References

- Rotter, J.B. (1966). Generalized expectancies for internal versus external control of reinforcement. *Psychological Monographs*, 80(1).
- Dweck, C.S. (2006). *Mindset: The New Psychology of Success*. Random House.
- Organ, D.W. (1988). *Organizational Citizenship Behavior: The Good Soldier Syndrome*. Lexington Books.
- Campbell, W.K., Bonacci, A.M., Shelton, J., Exline, J.J., & Bushman, B.J. (2004). Psychological entitlement: Interpersonal consequences and validation of a self-report measure. *Journal of Personality Assessment*, 83(1), 29–45.
- Maslow, A.H. (1969). The farther reaches of human nature. *Journal of Transpersonal Psychology*, 1(1), 1–9.
- Batson, C.D. (2011). *Altruism in Humans*. Oxford University Press.

---

## Author

Built as a submission for the Daily Reflection Tree design challenge.