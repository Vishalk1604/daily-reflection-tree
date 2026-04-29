# Design Rationale: The Daily Reflection Tree

**Submission Type:** Part A — Written Rationale  
**Word Count:** ~900 words (approx. 2 pages)

---

## 1. The Core Design Problem

Building a reflection tool is easy. Building one that doesn't moralize, doesn't feel clinical, and produces genuinely different outputs for genuinely different people — without any AI at runtime — is harder than it looks.

The challenge is that every psychological framework carries implicit judgment. "Locus of control" sounds neutral until you notice that "internal" is always the good answer in management literature. "Contribution vs. entitlement" sounds balanced until you notice that one of those words is almost universally used as an insult. The first design problem, then, is not structural — it's tonal. The tree cannot be a quiz that grades employees. It has to be a mirror that reflects without distorting.

The second design problem is determinism itself. A conversation that branches deterministically risks feeling mechanical. Every path must feel like it was written specifically for the person walking it — even though it was written months in advance, for anyone who might walk it.

These two constraints shaped every decision in the tree.

---

## 2. Why Three Axes

The three axes — Locus, Orientation, and Radius — were selected because they are orthogonal. They measure genuinely different things, which means their intersections produce information that neither axis could produce alone.

**Locus of Control** (Rotter, 1966; Dweck, 2006) captures how an employee attributes outcomes: to their own actions, or to external forces. This is the most established axis in organizational psychology, but it is often misread as a character trait. It is not — it is a momentary attribution style that shifts with context, fatigue, and stakes. The tree treats it accordingly: it asks about today, not about personality.

**Orientation** (Organ, 1988; Campbell et al., 2004) captures the direction of attention during social interaction: toward what was given, or toward what was owed. Organizational citizenship behavior research consistently shows that contribution-oriented employees create disproportionate value in teams — but entitlement-awareness is not the same as selfishness. The experience of feeling underrecognized is nearly universal. The tree names it without pathologizing it.

**Radius of Concern** (Maslow, 1969; Batson, 2011) captures the breadth of perspective the employee held during the day: self only, immediate team, a specific other, or the broader population their work serves. Maslow's late work on self-transcendence identified outward perspective as the highest tier of human motivation — and Batson's research on empathy showed it to be trainable, not fixed. The tree surfaces where the employee's natural radius landed, and gently questions its edges.

The three axes matter together because of cross-axis effects. An employee with high internal locus *and* contribution orientation is a rare psychological profile — and a meaningfully different person to speak to than one with high internal locus *and* entitlement orientation. The tree contains dedicated nodes for these intersections (`RARE_COMBO_INTERNAL_CONTRIB`, `EDGE_CASE_EXTERNAL_ENTITLE`, `TENSION_POINT_INTERNAL_ENTITLE`) that are only reachable by accumulating the right signal combination across axes. This is what makes the final summary feel personal — not just template-filled, but path-earned.

---

## 3. How the Tree Avoids Moralizing

Every question in the tree has at least one option on the "worse" psychological path that is written with dignity.

For example, on the Locus axis, the option "I felt stuck, waiting for something outside to change" is not framed as a failure. It is framed as a recognizable human experience — because it is one. The reflection node that follows (`A1_R_EXTERNAL`) does not say "you should have done better." It says: *"somewhere in it, even quietly, you made a call. Even waiting is a decision."* This is not reassurance. It is a reframe that is genuinely true — and it opens a door without pushing the employee through it.

The hardest node to write was `EDGE_CASE_EXTERNAL_ENTITLE` — the path where an employee felt both that the outcomes were outside their control *and* that they were not fairly recognized. This combination is the closest the tree gets to a "low" result. The reflection names the psychological risk ("learned helplessness dressed up as fairness consciousness") but frames it as a risk to watch, not a diagnosis. The only ask is a micro-experiment for tomorrow: *"what tiny thing can I still choose?"* Not a motivational speech. One small question.

The principle behind this: reflection is not feedback. Feedback tells people what to do differently. Reflection shows people what was already there. The tree's job is the latter.

---

## 4. Structural Decisions

**Signal accumulation over binary branching.** Rather than routing the entire tree on a single answer, every question contributes a signal to a running count per axis. The dominant signal at the end of each axis determines which reflection the employee receives. This means two employees can answer question one differently and still arrive at the same reflection — or answer question one the same way and diverge later. The path is earned across multiple answers, not decided by the first click.

**Adaptive bridges.** The transition nodes between axes (`BRIDGE_1_2`, `BRIDGE_2_3`) change their text based on the dominant signal of the axis just completed. This creates continuity — the conversation remembers what was said — without requiring any runtime AI.

**The loopback.** After all three axes, the tree returns to the employee's opening word and asks if it still fits. This is the structural acknowledgment that reflection changes things. Some employees will say the word still fits. Some will say it doesn't. Both are valid. The loopback's answer feeds into the cross-axis routing decision that follows.

**The summary as synthesis, not generation.** The final summary uses six template slots — one per axis signal (two states each) and one cross-axis insight — and interpolates the employee's actual opening word into the text. The result reads as personal because it is assembled from the specific path taken, not generated from scratch. Every word in the summary was written in advance. No LLM is needed because the structure does the work.

---

## 5. References

- Rotter, J.B. (1966). Generalized expectancies for internal versus external control of reinforcement. *Psychological Monographs*, 80(1), 1–28.
- Dweck, C.S. (2006). *Mindset: The New Psychology of Success*. Random House.
- Organ, D.W. (1988). *Organizational Citizenship Behavior: The Good Soldier Syndrome*. Lexington Books.
- Campbell, W.K., Bonacci, A.M., Shelton, J., Exline, J.J., & Bushman, B.J. (2004). Psychological entitlement: Interpersonal consequences and validation of a self-report measure. *Journal of Personality Assessment*, 83(1), 29–45.
- Maslow, A.H. (1969). The farther reaches of human nature. *Journal of Transpersonal Psychology*, 1(1), 1–9.
- Batson, C.D. (2011). *Altruism in Humans*. Oxford University Press.