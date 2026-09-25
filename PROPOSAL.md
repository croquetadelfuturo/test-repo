# A business-idea generator and evaluator that stays small

*Proposal, v2, September 2026. The research behind it is in [EVIDENCE.md](EVIDENCE.md). This version incorporates independent reviews by Claude Fable 5.1 and GPT-6 Astra (see the end).*

## TL;DR

- **This is an experiment loop, not an idea pipeline.** Each week you pick one or two ideas, find the assumption most likely to kill each one, and run one real-world test whose evidence matches the claim. Generation runs alongside and feeds the loop. It never delays a test.
- **The LLM does breadth, research and devil's advocacy. The three of you decide. Customers supply the evidence.** The system never gives a score or a verdict, and its critic is allowed to answer "insufficient evidence".
- **Week 1 needs no code.** It's four prompts, a folder of markdown files, and one agreement between the three of you. You add tooling only when a real failure shows you need it.

---

## 1. Why the last one turned into process slop

My diagnosis has four parts:

1. **Models are trained toward "more".** RLHF reward gains come largely from length. In one 2026 study, training a model to score well against a rubric made its output look more complete while getting less correct. Ask an LLM to make a process "more rigorous" and you get more stages.
2. **LLM-graded gates pass themselves.** Most of a model's self-checks just confirm what it already said. Judges also go easier on their own output. So a gate the LLM grades almost never fails, and a gate that never fails never gets removed.
3. **Every disappointment gets patched with a rule.** The largest share of multi-agent failures (about 42%) comes from specification and design, not from the model. Each patch adds a stage, and each stage adds a handoff.
4. **Process stands in for evidence that only exists outside the system.** You can't know if people will pay without asking them to pay. Gates promise certainty without that contact, so they keep multiplying.

The fix is to point the system at the outside world and keep the internal machinery small enough to throw away.

## 2. Step 0: a working agreement (one page, about an hour)

Both reviewers said independently that the biggest gap in v1 was the three of you. "Viable" means nothing until you agree on what you're building toward. This isn't equity negotiation or a gate. It's a short, revisable page answering:

- **What kind of business?** Replacing your incomes, or aiming for venture scale? The two lead to very different ideas.
- **Runway:** the minimum income each of you needs, and how many months and how much money you'll put in before deciding.
- **Commitment for the next 4–8 weeks:** hours per week each, who does what, and whose money pays for tests.
- **How you decide:** by agreement where possible. A 2–1 vote can pick which test to run next. **No vote can commit another founder's money or time without their consent.**
- **Constraints:** non-compete and IP-assignment clauses with your current employers, and any licensing or regulatory limits.

Revisit the page after the first real signal from customers. Leave permanent equity until later.

Each of you also writes a short profile: skills, industries you know, people you can reach, unfair advantages, and things you'd hate doing for five years. Generation uses it, because it's the input a generic LLM can't invent.

## 3. The weekly loop

```
    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
    │ pick 1–2    │ →  │ skeptic:    │ →  │ run one     │ →  │ log + decide│
    │ ideas       │    │ riskiest    │    │ 7-day test  │    │ (incl. "not │
    │             │    │ assumption  │    │             │    │ yet tested")│
    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
          ↑                                                         │
          └──── shortlist (≤10) ← generation runs alongside ←───────┘
```

**Week 1 starts with an idea you already have.** Don't build anything first.

### 3a. Skeptic

One call, using a model from a different vendor than the generator for another perspective. The idea is presented as a third party's proposal, and the prompt is in the appendix. It returns:
- the single assumption most likely to make this business unattractive *given your working agreement*;
- sourced facts kept separate from hypotheses;
- one test that fits your time and cash, naming who to recruit, how to reach them, and what behaviour to observe;
- a threshold tied to your economics;
- what each possible result would and would *not* establish.

Base rates the model can't source stay "unknown" and aren't made up.

### 3b. Match the evidence to the claim

This is where v1 got it wrong: its own example "tested" willingness to pay by counting people who described a pain. Each kind of claim needs the evidence that fits it:

| Claim | Evidence that counts | Evidence that doesn't |
|---|---|---|
| The pain exists | Specific recent stories of time or money spent on it ("last month we…") | "Yeah, that sounds useful" |
| They'll pay | A deposit, pre-order, signed letter of intent at a stated price, or a paid pilot | Saying they'd pay, and interview enthusiasm |
| We can reach them | Response rate from a *cold* channel you could scale | Replies from friends and warm intros |
| We can deliver | Delivering it once by hand (a concierge test) at a known cost | A plan |
| The buyer can buy | The person saying yes controls the budget | A user who loves it but can't sign |

### 3c. Log it, including what the test couldn't tell you

One markdown file per idea. Add a new entry for each test and never edit old ones. Record:
- who you approached and whether they were warm or cold contacts;
- who declined;
- who had purchasing authority;
- what they actually committed to.

Friendly contacts are polite. They report pain and agree to pilots they'll never pay for.

```
## Crux: Independent dental offices will pay ~$200/mo for X
Kill if: fewer than 2 of 20 office managers (cold outreach) sign a paid pilot at $200
Approached: 20 cold, 4 warm | Replied: 9 | Declined: 5 | Had budget authority: 6
Result: 1 paid pilot, 2 "maybe next quarter"
Was this a real test? Partly: 3 of the 6 with budget authority said the offer was unclear
Decision: retest with a clearer offer to budget-holders only (all three agreed)
```

**Before you kill an idea, ask whether the test actually tested it.** A failure can mean the wrong buyer, a weak offer or a channel that didn't reach anyone, rather than a bad business. Record "not yet tested" as its own outcome. That keeps you from killing good ideas, and also from generating replacements for ideas nobody actually tested.

### 3d. Cap your attention

- At most **two ideas under test** at once, and at most **10 on the shortlist**.
- Generation pauses whenever you have no free capacity to test. A pile of untested ideas is the start of the slop.

## 4. Generation (runs alongside the loop from week 1)

You wanted new ideas as well as tests of your existing ones, and both reviewers agreed that a competing set makes the existing ideas easier to judge. So a small batch is generated in week 1, without delaying the first test.

**Where seeds come from, in this order:**
1. **The AI, from your profiles.** It generates *before* seeing your existing ideas, so it isn't anchored on them.
2. **The AI, from the world.** A web-research pass looks for pain people already spend money or time on. Sources include low-star reviews, forums describing workarounds, job postings for repetitive manual work, regulation changes, business models proven elsewhere, and things that recently became cheap. Every finding needs a **verbatim quote and a URL**. A link that loads isn't enough, because the quote has to support the claim.
3. **Your ideas, plus deliberate remixes of them:** other customers, other business models, other channels.

**How to keep it diverse without machinery.** Do it all in the prompt:
- plan several distinct directions first;
- write from ordinary, specific personas, not "visionary" ones;
- think step by step;
- ask for some ideas that few founders would propose;
- allow one revision pass.

Then have one call merge duplicates and say how many genuinely distinct ideas are left. There are no embeddings and no diversity metrics. If the pool keeps coming back samey, *then* consider more.

**Cards.** Every idea goes onto the same short card:
- Who is the customer?
- What's the pain, and how do they solve it today?
- What would we sell?
- Why us?
- Why now?
- Who pays, and how much?
- How do we find customer #1?
- How long until the first revenue?

Any field the source doesn't support says **"unknown"**. Any pain that came from a persona, not from evidence, is labelled **"hypothesis"**.

**Shortlisting.**
1. Each of you privately marks the cards keep, drop, or "can't tell".
2. The AI does the same, with one sentence of reasoning per card. It may answer "insufficient evidence".
3. Your own ideas always make the shortlist.
4. Once your picks are recorded, compare them with the AI's and discuss the disagreements.

The AI's view is meant to influence you, and seeing it after you've committed to your own is the point. The one thing protected is your *first* judgment.

## 5. Choosing among survivors

When more than one idea survives its tests, choose by talking it through against your working agreement. Don't compute anything. The questions:
- Which gets to first revenue fastest, and for the least cash?
- Which do all three of you want to work on for years?
- Which fits the kind of business you said you want?
- What evidence would make you regret the choice, and can you get it cheaply first?

The decision goes in the ledger with everyone's reasoning.

## 6. What's left out, and why

| Left out | Why |
|---|---|
| Scores, weighted rubrics, "probability of success" | Even experts don't agree on fine-grained ratings. LLMs over-predict startup success. Rubrics reward output that looks complete. |
| Gates the LLM grades | They almost always pass, so they tell you nothing. |
| Pairwise AI tournaments | Expensive: exhaustive comparisons over 50–150 cards take thousands of calls. Head-to-head comparisons are also easily swayed by surface features. |
| Multi-agent debate | Debate rarely beats one model reasoning step by step. |
| Simulated customer panels | They're weakest for new categories and pick the wrong segment about half the time. A persona may *suggest* a pain as a hypothesis, labelled as one. |
| Embeddings, diversity metrics, calibration scripts | Nothing has failed yet that they would fix. |
| An agent framework | It hides the prompts, and the prompts are the system. |

One thing v1 banned is back: **a revision pass is allowed** where it demonstrably helps, as it did in the idea-diversity research. What's still out is using a model's check of its own work as a gate.

## 7. Keeping it small

1. **Four prompts:** generate, research, skeptic, compare. Each opens with one line saying why it exists.
2. **A new component needs two things:** a real failure that plausibly came from *its absence*, and good reason to think it would improve a decision that matters. A disappointment alone isn't enough.
3. **Never ask an LLM how to make the process more rigorous.** Ask what can be deleted.
4. **Output caps:** 8-line cards, and skeptic replies of 200 words or fewer.
5. **When you switch models, check sycophancy once by hand.** Give the skeptic the same idea twice, once framed as "ours, we're excited" and once as a stranger's, and compare what comes back. Five minutes, no scripts.

## 8. Build

```
founders/agreement.md   ← section 2
founders/<name>.md      ← profiles
prompts/                ← generate.md, research.md, skeptic.md, compare.md
ideas/pool.md           ← cards
ideas/<slug>.md         ← one ledger per idea under test
```

Weeks 1–2 can run entirely in a chat window with these prompts. Write a script only if copy-pasting becomes the bottleneck. Use two vendors, for example GPT-6 Astra and a current Claude model, one to generate and one to critique. That gives another perspective, though not a guarantee of independent errors.

## 9. The first month

| Week | What happens |
|---|---|
| 1 | Write the working agreement and profiles. Take your strongest existing idea through the skeptic and start its test. Generate the first small batch in parallel. |
| 2 | Finish test 1 and log it. Shortlist to 10 or fewer. Start a test on the second idea, existing or generated. |
| 3 | Decide on idea 1: continue, retest, kill, or "not yet tested". Test the next one. |
| 4 | Review the ledger against the working agreement. Choose what gets your next month. |

If you've spent more time on the tool than on customers by the end of the month, something has gone wrong.

## 10. What we don't know

- **How good the AI's judgment is on your ideas.** The best evidence (LLMs predicting Kickstarter results better than managers) is one study of consumer crowdfunding. Treat the AI's view as a second opinion.
- **Whether the diversity tricks transfer.** They were tested on one product task with older models.
- **Most numbers in EVIDENCE.md haven't been independently checked.** The ten that matter most were checked by Astra, and several were corrected.

---

## Review history

- **v1 → v2.** Claude Fable 5.1 and GPT-6 Astra reviewed v1 independently, without knowing who wrote it. Astra also checked the key claims against the primary sources.
  - **Both reviewers:** test existing ideas in week 1; add a working agreement; cap attention; cut the measurement layer and pairwise ranking (v1's cost estimate was off by 10–100x); fix the skeptic and ranking prompts.
  - **Astra:** evidence must match the claim (v1's payment test was invalid); a loading link doesn't prove the source supports the claim; don't ban revision; a failed test may not have tested anything; several evidence rows were overstated, and one ("Opus 5.5 strongest on honesty") was wrong.
  - **Fable:** add a customer #1 channel to the card; label persona-derived pain as a hypothesis; require quotes for research findings; legal constraints.
  - **Where the reviews disagreed:** Fable wanted generation only after existing ideas fail. Astra and I disagreed, because that favours your first guesses and ignores that you asked for new ideas.

---

## Appendix: the prompts

**generate.md**
> *Exists because: generating ideas in bulk is cheap for an LLM, and a single plain prompt produces homogeneous ideas.*
> Founders: {profiles}. Working agreement: {agreement}. First, propose 8 search directions that differ from each other as much as possible: specific customer groups these founders can reach, painful workflows in industries they know, recent changes, business models proven elsewhere. For each direction, think as one ordinary, specific person affected by it, list the problems they deal with in a normal week, and propose 3 business ideas, at least one of which fewer than 1 in 10 founders would suggest. Then review the whole list once: replace the weakest third with better ideas. Output cards with the fields: who / pain + how solved today / offer / why us / why now / who pays and how much / how we find customer #1 / time to first revenue. Write "unknown" where you have no basis, and mark persona-derived pain as "hypothesis".

**research.md**
> *Exists because: finding evidence across hundreds of pages is what LLMs with web search are good at, and they still fabricate sources.*
> Mode A (seeding): find evidence of problems people or businesses already spend money or time working around, in areas these founders could reach. Mode B (per idea): find existing companies, substitutes, pricing and failed prior attempts for {idea}. For every finding, give a verbatim quote, the URL, and one sentence on what it shows. If you can't find something, say "not found". Absence of evidence is not evidence of opportunity. At most 20 findings.

**skeptic.md** *(adapted from Astra's rewrite)*
> *Exists because: LLMs are good at the strongest case against an idea when they don't know whose idea it is.*
> A third party proposes the business below. Our goals and limits: {agreement}. Identify the single assumption most likely to make this business unattractive under those goals. Separate sourced facts from hypotheses; leave base rates you can't source as "unknown". Propose one test that fits our time and cash limits: whom to recruit, how to reach them, and what behaviour to observe. The behaviour must match the claim; for willingness to pay, that means payment or a signed commitment. Propose a threshold tied to our economics, marking your assumptions. State what each possible result would, and would not, establish. Add the one interview question most likely to embarrass this idea. 200 words maximum.

**compare.md** *(adapted from Astra's rewrite)*
> *Exists because: an independent second opinion on which idea to test next.*
> Given these ideas, our working agreement and the sourced evidence, which deserves our next seven-day experiment? For each idea, give its strongest *supported* advantage and its biggest unknown. Recommend one, or "insufficient evidence". Name the observation that would reverse your recommendation. Don't infer missing facts or predict long-term viability. 120 words maximum.
