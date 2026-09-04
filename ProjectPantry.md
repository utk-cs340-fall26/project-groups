# Project Pantry

## Project Description

Project Pantry is an AI recipe generator constrained to what a user actually has on hand, rather than one that produces plausible recipes requiring a grocery run.

Pantry contents are populated by AI-parsed grocery receipts and depleted when recipes are cooked, tracked as an append-only event ledger so stock is always derived and never overwritten. Freeform feedback on cooked meals ("too salty," "loved the crispy edges," "stop putting cilantro in things") is distilled by a small model into structured preference signals, which decay over time and weight future generations — so the system learns a household's taste without ever pasting raw feedback history into a prompt.

**The problem it solves.** Recipe apps recommend food you can't make. They assume a fully stocked kitchen, ignore the half-used bag of lentils and the chicken thighs expiring Thursday, and treat "what do I have?" as the user's problem to solve manually. The result is either a shopping trip or a discarded suggestion — and, downstream, food waste. Generic LLM recipe prompts fail differently: they hallucinate quantities, forget stated preferences within a session, and have no memory that you already said you hate fennel three meals ago. Neither approach closes the loop between *what's in the kitchen*, *what got cooked*, and *whether it was any good*.

### Major Features

- **Append-only pantry ledger** — every stock change is an immutable event row (`purchase | cook | waste | expire | manual_adjust | reconcile`) carrying a reference back to the receipt or recipe that caused it. Current stock is a derived view, and mutation is blocked at the database level by triggers rather than by convention.
- **Canonical unit normalization** — quantities normalize to grams, milliliters, or counts at write time against the ingredient's declared dimension. When a conversion needs a density or state we don't have, the system asks instead of inventing a number.
- **Tiered ingredient resolution cascade** — receipt line items resolve cheapest-first: exact match on normalized retailer aliases, then trigram similarity with abbreviation expansion, then a small-model call with top candidates as multiple choice plus an explicit "unknown" option. Successful resolutions are written back as aliases; low-confidence results go to a human confirmation queue.
- **Feedback distillation with provenance** — freeform notes become structured preference signals (subject, axis, direction, magnitude, scope, confidence), each retaining the verbatim quote that produced it, so the UI can explain *why* it thinks you dislike something and let you delete that belief. Contradictions resolve through per-axis time decay rather than special-case logic.
- **Pantry-constrained generation** — only a compact derived preference profile and the current pantry snapshot enter the generation prompt, and every output is checked by a deterministic validator that rejects any recipe calling for more than you have.

### Stack

TypeScript, Next.js (App Router), Postgres 16, Drizzle ORM, Zod validation at every boundary, and the Anthropic API called directly (no orchestration frameworks).

### Intended Users

Home cooks who shop irregularly, households trying to cut food waste, and anyone who owns more ingredients than meal ideas.

---

## Group Members

| Name | GitHub ID |
|---|---|
| Cameron Wilson | c4w1 |
| Min Jae Bae | mazinouruk |
| Vincent Guo | guo-vincent |
| Allen Xie | ScriptlessFoe |
| John Stuart Roberts | jroberts8893 |

---

## Group Status

**Full** — 5 of 5 members. We are not looking for additional members.
