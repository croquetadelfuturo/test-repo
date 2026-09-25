# A business-idea generator and evaluator that stays small

*Proposal, September 2026. The research behind every claim is in [EVIDENCE.md](EVIDENCE.md).*

## TL;DR

- **The LLM handles breadth. The three of you handle judgment. Real customers give the verdict.** The system never produces a score or a go/no-go. It produces varied ideas, a short list of the assumptions each idea depends on, and the cheapest real-world test for each one.
- **It has four steps and one log:** Seed → Spray → Sort → Test, plus an evidence ledger. That's about seven prompts, a folder of markdown files, and a few hundred lines of script.
- **Your judgment and the AI's stay separate until the end.** You form yours before seeing its. The AI ranks independently, and you compare the two instead of blending them. This is the single most evidence-backed rule here.
- **Anything added to the system has to earn its place.** Each component states the model weakness it covers and a check that shows whether that weakness still exists. When the check stops finding problems, the component is deleted.

---

## 1. Why the last one turned into process slop

Hard gates, oracles and stage checklists aren't bad luck. They're what you get when you combine how LLMs are trained with a problem that has no internal answer. There are four mechanisms:

1. **Models are trained toward "more".** Most of the reward gain from RLHF (human-feedback fine-tuning) comes from length. Training a model to score well against a checklist makes its output more "complete" but less concise, less correct and worse overall. Ask an LLM to make a process "more rigorous" and you reliably get more stages and more rubrics. Humans reward this too: reviewers react *more* positively to agent-written code that adds redundancy.
2. **An LLM checking its own work is confirmation, not verification.** Up to 95% of reasoning models' self-checks just confirm the first answer. LLM judges are over 50% more likely to wrongly mark a rubric item as satisfied when the output is their own. So LLM-graded gates almost always pass. They feel safe, they generate no new information, and nobody ever deletes them.
3. **Every failure gets patched with a rule.** About 42% of multi-agent failures trace back to specification and design, not to the model. Each fix adds a stage and each stage adds a handoff. Anthropic's own harness-engineering post puts it this way: every component "encodes an assumption about what the model can't do", and those assumptions go stale with each model release.
4. **Process stands in for evidence nobody can get from inside the system.** You can't know whether people will pay without asking them to pay. Gates promise certainty without that contact with the world, which is why they keep growing: no amount of internal process ever actually settles the question.

The fix isn't better gates. Point the system at the outside world: customers, prices, base rates, and the three of you. Keep the internal machinery small and easy to throw away.

## 2. What LLMs are actually good and bad at here

| Use it for | Don't use it for |
|---|---|
| Producing lots of ideas, **if** you make it diverse (structured prompts, many ordinary personas, planned directions). | Asking for ideas in one plain prompt. You'll get the same 20 ideas everyone else gets. |
| Web research on competitors, substitutes, pricing and regulation. Hallucination has improved a lot in the current models. | Treating "no competitors found" as a green light. Deep-research agents still fabricate 3–13% of their links. |
| Writing the strongest case *against* an idea (premortem, base rates), when it doesn't know whose idea it is. | Asking it whether *your* idea is good. Sycophancy persists in current models, and the Fable 5.1 system card reports it's *less* honest under pressure. |
| Ranking plain-template ideas independently of you. On live Kickstarter ventures, frontier models out-ranked 346 managers. | Blending its ranking with yours. People follow wrong AI answers about 4 in 5 times, and a blended ranking did worse than the AI alone. |
| Drafting customer-interview scripts, landing pages and pre-sale offers. | Simulated customers as a demand signal. They're weakest in new categories, and no LLM beats simple baselines at predicting individuals. |
| Arguing the other side. | Multi-agent debate, self-critique loops, or an LLM grading its own output. The evidence says these add nothing. |

## 3. The design

```
  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
  │  SEED    │ →  │  SPRAY   │ →  │  SORT    │ →  │  TEST    │
  │ (humans, │    │  (LLM)   │    │ (humans  │    │ (world)  │
  │  LLM and │    │          │    │  + LLM,  │    │          │
  │   web)   │    │          │    │ separate)│    │          │
  └──────────┘    └──────────┘    └──────────┘    └──────────┘
        ↑                                              │
        └──────────────  evidence ledger  ←────────────┘
```

### Step 0: Founder profile (write once, update occasionally)

One page per person, plus one shared page:
- Skills, industries you've worked in, and people you can reach.
- Unfair advantages, meaning things you know or have access to that most people don't.
- Constraints: money, time, location, risk tolerance.
- Things you'd hate doing for five years.

This is the most useful input the generator gets, because it's the part a generic LLM can't make up.

### Step 1: SEED (three independent sources)

Seeds come from three sources. They are generated in parallel, and none of them sees the others' output, so no source can anchor another:

1. **Your ideas.** The ideas you already want to stress-test, plus anything else you think of. There's no quota.
2. **The AI, from your profiles.** One call reads the founder profiles and proposes seeds that exploit the skills, access and unfair advantages you described. This covers what you're well placed to do but haven't thought of. It reuses the generate prompt with your profiles as the direction, so it adds no new prompt.
3. **The AI, from the world.** A web-research pass looks for evidence of pain that people already spend money or time on. Sources to search:
   - 1–2 star reviews of existing products;
   - forum and Reddit threads where people describe workarounds;
   - job postings that pay people to do repetitive manual work;
   - recent regulation changes;
   - business models that work in another country or industry and haven't been ported;
   - things that recently became cheap or possible.

   Every seed carries the URL it came from, and the script checks that the links resolve. This is the source humans can't easily produce, because it takes reading hundreds of pages. It also starts each idea from observed demand rather than from what sounds clever.

All three sources go into the same pool and are treated identically from here on.

The only guardrail: if any of you want to add ideas of your own, jot them down *before* browsing the AI's seeds. People who see AI ideas first converge on them, and stay converged even after the AI is removed. It's a five-minute habit, not a gate.

### Step 2: SPRAY (LLM, about 20 minutes of compute)

Goal: a large, genuinely varied pool of ideas, then deduplicated down to 50–150 cards.

1. **Plan directions.** One call reads the founder profiles and proposes 10–20 *different* directions to search. For example: a customer group you can reach, a painful workflow in an industry you know, a regulation change, a business model that works in another country or industry and could be ported.
2. **Generate within each direction.** Run one call per direction, using:
   - step-by-step prompting;
   - a specific, ordinary persona (for example "a dental practice office manager in Ohio", not "Steve Jobs");
   - "give k ideas with how typical each one is" (verbalized sampling);
   - "differ from what others would say".

   The research shows these simple moves together beat human diversity.
3. **Remix the seeds.** Run the same mechanism on every seed, from all three sources: variations, other customer groups, the same idea with a different business model.
4. **Use at least two model vendors.** It's a cheap hedge, not a guarantee. Different vendors' models are more alike than you'd expect.
5. **Deduplicate and measure.** Embed every idea, merge near-duplicates, and report two numbers: pool diversity (mean pairwise distance) and a saturation curve. **Stop when a new batch adds almost nothing new.**
6. **Normalize every idea to one plain card template.** Same fields, same length, no adjectives, and no indication of whether a founder or the AI wrote it:
   - **Who:** the specific customer.
   - **Pain:** what hurts, and how they solve it today.
   - **Offer:** what we'd sell.
   - **Why us:** the edge from the founder profiles.
   - **Why now:** what changed that makes this possible or needed.
   - **Money:** who pays, roughly how much.

   This step does double duty. LLM judges are swayed by writing style, and stripping ideas to their content raised one judge's ability to recognize substance from chance (0.50) to 0.76.

### Step 3: SORT (humans and LLM, independently, then together)

1. **Each founder privately marks every card keep or drop.** No 1–10 scores: experts disagree almost completely on fine-grained scores and much less on keep/drop. Don't look at each other's picks yet.
2. **The AI ranks the same anonymous cards separately.**
   - Use a model family different from the generator.
   - Compare cards in pairs, both orders each time, and count a flip as a tie.
   - Aggregate into a ranking and give a one-line reason per card.
   - It never sees your picks.
3. **Look at the disagreement map, not an average.** You get one table with the three founder picks and the AI rank side by side. You discuss only:
   - cards the AI ranks high that you all dropped: *what does it see?*
   - cards you like that the AI ranks low: *what do we know that it doesn't, or are we fooling ourselves?*
   - cards where the three of you split.

   You make the decision, and it's written in the ledger. Nothing is averaged.
4. **Keep 3–5 ideas.**

### Step 4: TEST (the world is the only oracle)

For each shortlisted idea, run two LLM passes and then real-world tests.

**Research pass** (a deep-research call with web search):
- What it finds: competitors, substitutes, pricing, market structure, regulation, and failed prior attempts.
- The script automatically checks that every link resolves and flags any claim without a working source.
- "Nothing found" is reported as *low confidence*, never as evidence the idea is new.

**Skeptic pass** (one call, different model family, idea presented as "a proposal from a third party"):
- **Premortem:** "It's 18 months from now and this failed. Write the three most likely reasons."
- **Outside view:** "What's the reference class, and what's the base rate for ventures like this?"
- **Cruxes:** "List the 3 assumptions that, if false, kill this idea." Each one is written as a falsifiable claim: *at least X% of Y will do Z* (Savoia's XYZ format).
- **Cheapest test per crux:** customer interviews, a fake-door landing page, a pre-sale, a concierge MVP (doing the service manually), or a price test.

No score and no verdict. One page, maximum.

**Then you go and do the tests.**
- The LLM drafts the interview script following *The Mom Test*: ask about specific past behavior, never "would you use this?"
- Before running each test, you write down what result would make you drop the idea. That's the kill criterion, and it's set *before* you see the data.
- Twenty real conversations beat anything the system can compute.

### The evidence ledger (the only loop that matters)

One markdown file per idea, append-only:

```
## Crux: At least 30% of independent dental offices would pay $200/mo for X
Kill if: fewer than 3 of 15 interviewed describe spending time/money on this in the last 90 days
Test: 15 interviews, 2026-10-02 → 2026-10-12
Result: 7/15 described it; 2 asked for a pilot
Decision: continue (all three) — next crux: willingness to switch from spreadsheets
```

The ledger is also how the system learns. When you run Spray again, it reads the ledger, so the next round builds on what customers actually said.

## 4. What's deliberately left out, and why

| Left out | Why |
|---|---|
| Numeric scorecards and weighted rubrics | Experts don't agree on fine-grained scores. Rubrics graded by an LLM reward output that *looks* complete. |
| Gates the LLM grades | Self-verification just confirms, and judges pass their own output. |
| Multi-agent debate or "bull vs. bear" agent teams | Debate beat plain step-by-step prompting in ≤20% of setups. Multi-agent systems scored 39–70% worse on sequential tasks. |
| "Critique and revise" loops | Up to 95% of self-checks just confirm the first answer. |
| Simulated customer panels for demand | They're weakest for new categories and wrong about segments about half the time. At most, the skeptic may brainstorm objections from a customer's point of view. |
| A "probability of success" | Models over-predict success. Zero-shot predictions did worse than a logistic regression on real funding outcomes. |
| Automatic kill/continue decisions | The decision belongs to you, and the data belongs to the world. |
| An agent framework | Direct API calls and files. Frameworks hide the prompts, and the prompts are the system. |

## 5. Keeping it small

These rules are the actual defense against bloat. Put them in the repo README.

1. **Complexity budget.** At most 7 prompts. Each output has a fixed length limit (card ≤ 80 words, brief ≤ 1 page). If you add a prompt, remove one or explain in writing why you can't.
2. **Every component carries its reason.** Each prompt file opens with one line: *"Exists because: [model weakness]. Check: [how we'd know it's no longer needed]."* If you can't write that line, the component doesn't go in.
3. **Add things only after a failure you saw in real use.** Not because it "might help", and not because an LLM suggested it. A change has to show it altered a real decision.
4. **Never ask an LLM "how can we make this more rigorous?"** Ask "what can we delete?" The first question always produces more process.
5. **Recheck on every model upgrade.** Run the calibration checks below and delete any safeguard whose check no longer finds a problem.

### The calibration checks (about one afternoon, rerun when you change models)

These replace trusting papers that test older models. There are three checks, and each is a short script.

- **Sycophancy:** send the same 10 cards to the skeptic twice, once as "our idea, we're excited" and once as "a stranger's proposal". If the cruxes or tone change meaningfully, keep the anonymization strict. If they don't, relax it.
- **Order bias:** rerun the AI ranking with the cards in reverse order. If more than about 20% of pairwise verdicts flip, keep the both-orders rule. If almost none flip, drop it and halve the cost.
- **Diversity:** generate 50 ideas with a plain prompt and 50 with the Spray prompt, then compare pool diversity. If the plain prompt is just as diverse on the current model, simplify Spray.

## 6. Build sketch

```
ideas/
  README.md          ← the five rules above
  founders/          ← profile pages
  prompts/           ← seed-world.md, plan.md, generate.md, card.md, rank.md, research.md, skeptic.md
  pool/              ← generated + seed cards (one JSONL file per run)
  ideas/<slug>.md    ← brief + evidence ledger per shortlisted idea
  run.py             ← ~300 lines: spray, dedupe, rank, brief, check-links, calibrate
```

- **Models.** Use one frontier model for generating and a *different vendor's* model for ranking and critique. Current system cards give two reasons: frontier models make correlated errors, and at least one current model is documented grading its own family's output higher. Given current reports, Claude Opus 5.5 and GPT-6 Astra are a reasonable pair. Which one generates matters less than keeping them separate. Run the calibration checks before trusting either.
- **Embeddings** are used for deduplication and the diversity number. Any embedding API works.
- **Interface:** markdown files in a shared repo or folder. Add a UI only if the three of you actually find the files painful.
- **Cost:** a full Spray-and-Sort run is a few hundred model calls. It's cheap enough to rerun weekly.

## 7. The first month

| Week | What happens |
|---|---|
| 1 | Write your founder profiles and list the ideas you already have. Run the AI seeding from your profiles and from web research. Build `run.py` spray and dedupe. Run the calibration checks. |
| 2 | Spray, then Sort to 3–5 ideas. Hold one discussion meeting, run on the disagreement map. |
| 3 | Research and skeptic briefs. Write kill criteria. Book 10–20 customer conversations per idea. |
| 4 | Run the tests. Update the ledger. Decide what to kill and what gets a second round of tests. Re-spray with what you learned. |

If by week 4 you've spent more time on the tool than on customer conversations, something has gone wrong.

## 8. What we don't know

- **Diversity on today's models.** No study tests business-idea diversity on GPT-6 Astra, Fable 5.1 or Opus 5.5, which is why the diversity calibration check exists.
- **How much to trust the AI ranking.** The most encouraging result (LLMs out-ranking managers on Kickstarter) is a single study of 30 consumer ventures. Treat the AI rank as a strong second opinion, not an oracle, and see whether your ledger agrees with it over time.
- **The numbers in the evidence file.** Most were read from abstracts and search summaries, not full papers. See the caveats at the top of [EVIDENCE.md](EVIDENCE.md).
- **Your market may differ.** B2B, regulated or hardware ideas differ from the consumer products most studies use. The Test step is designed to compensate for that, because it doesn't depend on the literature.

---

## Appendix: the prompts (sketches)

These are short on purpose. If a prompt grows past about 25 lines, treat that as a warning sign.

**seed-world.md**
> Search the web for evidence of problems that people or businesses already spend money or time working around, in areas these founders could reach: {profiles}. Look at 1–2 star product reviews, forums and Reddit threads describing workarounds, job postings for repetitive manual work, recent regulation changes, business models that work in one country or industry but not yet another, and things that recently became cheap or possible. For each finding, give the pain in one sentence, who has it, the evidence URL, and one seed idea. Only report pain you found evidence for. 20 findings maximum.

**plan.md**
> Here are three founder profiles and the ideas we already have. Propose 15 search directions for new business ideas that are as *different from each other* as possible. Mix these kinds: specific customer groups these founders can reach, painful workflows in industries they know, recent changes (regulation, tech, cost curves), and business models proven elsewhere that could be ported. One line each. Avoid the directions most people would suggest.

**generate.md**
> Direction: {direction}. Think as {ordinary specific persona}. First, list the problems this person actually deals with in a normal week. Then propose 6 business ideas that would solve one of them, each with an estimated probability that a typical founder would also suggest it. Make sure the ideas differ from what others would say. Use the card format. Be concrete, and no adjectives.

**card.md**
> Rewrite this idea into the card format. Keep only the substance and make no claims beyond the original. ≤80 words. Fields: who / pain (+ how they solve it today) / offer / why us / why now / money.

**rank.md**
> Two anonymous business ideas, A and B, for these founders: {profiles}. Which is more likely to become a viable business for *these* founders within 2 years? Answer "A" or "B" and give one sentence citing the single most decisive factor.

**skeptic.md**
> A third party has proposed the business idea below. Assume it's 18 months from now and it failed. (1) Give the 3 most likely reasons. (2) Name the reference class and its approximate base rate of success. (3) List the 3 assumptions that, if false, kill it, each as "at least X% of Y will Z". (4) For each assumption, give the cheapest test that could be run in two weeks. No overall verdict, no score. One page maximum.

**research.md**
> Find existing companies, substitutes, pricing, and failed prior attempts for the idea below. Every claim needs a URL. If you can't find something, say "not found". Don't infer that absence means opportunity.
