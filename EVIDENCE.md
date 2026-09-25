# Evidence behind the proposal

This is the research that [PROPOSAL.md](PROPOSAL.md) rests on. Each entry gives the finding, the source, and what it changes in the design.

**Independent check (September 2026).** GPT-6 Astra, with web access, checked the ten claims the design leans on most against the primary sources. Rows marked ✅ were confirmed. Rows marked ⚠️ were corrected: several claims had been overstated, and one was wrong. Other rows have not been independently checked.

**How much to trust the numbers.** The research was done in September 2026 with web search. The sandbox blocked direct access to arXiv and most publishers. Every paper here was confirmed to exist (title, authors, venue and ID) across several search indexes. Most numbers, though, come from abstracts and search snippets, not the full PDFs. Entries marked *(snippet)* are the least certain. Many 2026 items are preprints that have not been peer reviewed. Check the source before quoting a number to anyone.

**How current it is.** Most studies test models one or two generations behind the frontier. Treat each failure mode below as something to *check for* on the models you actually use (see the calibration checks in the proposal), not as a constant.

---

## 1. Generating ideas

### LLMs generate good ideas cheaply, but homogeneous ones

| Finding | Source | Design consequence |
|---|---|---|
| GPT-4 product ideas out-scored Wharton MBA ideas on purchase intent. The 2026 journal version reports LLM ideas ~7x more likely to land in the top 10%, but less novel and less diverse. | Girotra, Meincke, Terwiesch, Ulrich 2023 (SSRN 4526071); Terwiesch et al. 2026, *Production & Operations Mgmt*, doi:10.1177/10591478261474243 | Generating ideas is cheap. The scarce steps are diversity and selection. |
| Models repeat themselves, and different models give strikingly similar answers to open-ended prompts ("Artificial Hivemind"). | Jiang et al. 2025, NeurIPS D&B best paper, arXiv:2510.22954 | Mixing vendors does not guarantee diversity. Measure it. |
| Homogeneity is learned in pretraining. Alignment amplifies it but does not cause it. | Fortier, Chen, West 2026, arXiv:2608.11426 | Don't expect a model upgrade to fix it. Fix it in how you prompt. |
| All frontier LLMs tested produced more "crowded" ideas than humans on standard creativity tasks, but changing the generation strategy reduced the crowding. | Azad & Baten 2026, arXiv:2605.06540 | Same as above. |
| Creativity in science ideation barely tracks general-intelligence benchmark rank. | LiveIdeaBench, *Nature Communications* 2026, arXiv:2412.17596 | Don't choose the generator by leaderboard rank. |

### Diversity can be engineered at generation time

| Finding | Source | Design consequence |
|---|---|---|
| Two causes of sameness: LLMs fixate on their own early outputs, and they blend knowledge into one "average" voice. Chain-of-thought prompting plus *ordinary* personas produced more unique ideas than humans (248 vs 197 unique combinations, +26%), though humans matched it on number of categories (28 vs 27). The winning method included generating **and revising** ideas. Tested on one task (fitness products). ✅ *Astra-checked* | Deng, Brucks, Toubia 2026, arXiv:2602.20408 | Generate from many mundane, specific personas, and allow a revision step. The paper didn't test the proposal's full combination of prompting moves. |
| One planning call assigns different semantic directions to parallel generation calls. This gave the best diversity–quality–cost trade-off. Simply saying "differ from what others would say" was a strong cheap baseline. | Ibrahim, Azad, Baten 2026, arXiv:2605.30150 | Plan directions first, then generate within each. |
| Asking for *k* answers with their probabilities ("verbalized sampling") raised diversity 1.6–2.1x with no quality loss. | Zhang et al. 2025, arXiv:2510.01171 (ICML 2026) | A one-line prompt change. |
| 10 diverse AI personas removed the homogenization effect of human–AI co-writing. | Wan & Kalman 2025/2026, arXiv:2504.13868 | The earlier finding that "AI flattens ideas" (Doshi & Hauser 2024, *Sci. Adv.*) comes from how AI was deployed, not from AI itself. |
| Showing startups concrete examples of how other firms use AI produced 44% more use cases and 1.9x revenue in an RCT with 515 startups. | Kim, Kim, Koning 2026, SSRN 6513481 | Feed the generator real analogues (business models that work elsewhere), not just abstract prompts. |
| Only ~5% of 4,000 LLM-generated seed ideas were non-duplicates. | Si, Yang, Hashimoto, ICLR 2025, arXiv:2409.04109 | Deduplicate by embedding, and stop generating when the pool stops growing. |

### Humans exposed to AI ideas converge

| Finding | Source | Design consequence |
|---|---|---|
| ChatGPT reduced brainstorming diversity in 37 of 45 comparisons. | Meincke, Nave, Terwiesch 2025, *Nature Human Behaviour* 9:1107 | If founders add ideas of their own, they jot them down before browsing the AI's seeds. |
| LLM help improved performance only while it was available. Ideas stayed homogenized even after it was removed. | Kumar et al., CHI 2025, arXiv:2410.03703 | Same as above. |
| "Cognitive surrender": on trials where people consulted the AI and it was wrong, they followed it 79.8% of the time. Accuracy was 31.5% vs 45.8% without AI, a 14.3-point drop, and confidence rose 11.7 points. Tested on reasoning-test questions, not venture decisions. ✅ *Astra-checked* | Shaw & Nave 2026, SSRN 6097646 | Record your own judgment before you see the AI's. |
| AI users scored 17% lower on a later skills quiz, with no significant speedup. Usage patterns that kept people engaged preserved learning. | Shen & Tamkin (Anthropic) 2026, arXiv:2601.20245 | The three of you should stay the ones doing the thinking. |

### Humans plus AI, in the field

| Finding | Source | Design consequence |
|---|---|---|
| 776 P&G professionals: AI raised quality by ~0.37–0.39 SD, and one person with AI matched a two-person team without it. Teams with AI produced the most top-10% solutions. | Dell'Acqua et al. 2025/2026, *Organization Science*, NBER w33641 | AI helps a team. The "one person matched a two-person team" part says AI can substitute for a teammate, so it is *not* evidence that three founders need AI less. |
| Kenyan entrepreneurs with a GPT-4 mentor: zero average effect. High performers gained ~15% and low performers lost ~10%, depending on which advice they acted on. | Otis, Clarke, Delecourt, Holtz, Koning 2026, *Management Science*, doi:10.1287/mnsc.2024.06909 | The value is in filtering the advice. Check every AI claim against your own facts. |
| After ChatGPT, solo-founder launches surged on Product Hunt, but teams increasingly dominated the top tier. | arXiv:2605.10291 (2026) | Keep deliberation between the three of you at the center. |
| Human crowds produced more novel circular-economy business ideas. Human–AI solutions had higher value and viability. | Boussioux et al. 2024, *Organization Science* 35(5) | AI refines for viability. Humans supply the novelty. |

---

## 2. Evaluating ideas

### An idea that pitches well is not a venture that works

| Finding | Source | Design consequence |
|---|---|---|
| 43 experts each executed a randomly assigned idea (~100 hours each). AI ideas lost 1.05–1.98 points on a 10-point scale after execution, including overall quality. Human ideas also fell on overall quality (by 0.63). The AI decline was significantly larger, but final human-vs-AI differences were not significant when counted per project. ⚠️ *Corrected by Astra review* | Si, Hashimoto, Yang, ICLR 2026, arXiv:2506.20803 | Scores at the idea stage are cheap talk for everyone, AI and human. Test in the real world. |
| Execution-guided evolutionary search found better methods. RL trained on execution rewards collapsed toward simple ideas. | Si et al., ICML 2026, arXiv:2601.14525 | Use evolve-and-select against real signals. Don't optimize the generator directly on a score. |

### LLM judges: surprisingly good at some things, easily fooled on others

| Finding | Source | Design consequence |
|---|---|---|
| On 30 live Kickstarter ventures launched after the models' training cutoff, frontier LLMs predicted crowdfunding results better than people. Gemini 2.5 Pro reached a rank correlation of 0.74; 346 managers and 3 investors scored 0.04–0.45. Combining the best human with the best model gave 0.67. The "augmentation trap" is about *mathematically averaging* independent predictions. Founders discussing ideas with an AI was not tested. A 26-project LLM-only replication is included. ⚠️ *Corrected by Astra review* | Csaszar, Peterson, Wilde 2026, arXiv:2602.01684 | The AI's view is worth hearing on consumer products. Record yours first so the comparison means something, but this is one study predicting crowdfunding totals, not a rule about how to make decisions. |
| LLM ratings of 171 pitches tracked real fundraising outcomes. Apparent skill at predicting survival came mostly from memorized online footprints of the ventures. | Kleinert & Urbig 2026, *Entrepreneurship Theory & Practice*, doi:10.1177/10422587261430320 | Judge ideas the model can't have seen before. Beware of it "knowing" the answer. |
| Blind comparisons by 105 researchers of ideas from 14 LLMs and 5 scaffolds: scaffolds helped or hurt almost at random (+177 to −155 Elo). LLM judges agreed with experts 60–73% of the time, where 50% is chance. | "Ideation Arena" 2026, arXiv:2608.29696 | Elaborate scaffolding is not a reliable improvement. |
| Direct LLM judges recognized substance at chance level (0.50) and were swayed by writing style. A *trained style-detection module* feeding signals to the judge raised this to 0.76. The paper does not test rewriting ideas into a template. ⚠️ *Corrected by Astra review* | "Style Wins, Substance Loses" 2026, arXiv:2608.01666 | Judges are swayed by style. A plain card template helps you read ideas side by side, but there's no evidence it debiases an AI judge. |
| Experts disagreed widely on business-idea ratings (Krippendorff's α from −0.40 to 0.48). Overlap of each expert's above-median set was 0.33–0.89, but those sets were derived from numeric ratings, so this is not an experiment showing keep/drop decisions work better. ⚠️ *Corrected by Astra review* | ACL 2026 Industry, arXiv:2604.22517 | Fine-grained scores are noise, even from experts. Coarse decisions are a pragmatic choice, not an evidence-backed one. |
| Swapping A/B order reversed 55% of verdicts. Judges favored their own model family by 3–8 points. | Awuni et al. 2026, arXiv:2609.17857 (open-weight models only) | Run every comparison in both orders, and don't let the generator judge its own ideas. |
| Self-preference didn't shrink in more capable models. Scoring each dimension separately cut it by 31.5%. A separate study found much apparent self-preference is really judge error (only 51% of earlier findings survive). | arXiv:2604.22891; "Are LLM Evaluators Really Narcissists?" ICML 2026, arXiv:2601.22548 | Use a critic from a different model family. The self-preference risk is real but smaller than first thought. |
| Reasoning ("thinking") models are still swayed by position, authority and bandwagon cues on subjective questions. | Wang et al., COLM 2025, arXiv:2504.09946 | Extended thinking doesn't remove judging bias. |
| Pairwise preferences flipped 35% of the time when a distractor was added; absolute scores flipped only 9%. | Tripathi et al., COLM 2025, arXiv:2504.14716 | Head-to-head comparisons are easy to game with surface features. |

### Sycophancy: telling you what you want to hear

| Finding | Source | Design consequence |
|---|---|---|
| Across 11 models, AI endorsed users' actions 49% more often than humans did. Users rated the flattering AI as *more* trustworthy. | Cheng et al. 2026, *Science* 391, eaec8352 | Your satisfaction with the tool is the wrong metric. |
| Under up to 25 turns of pushback, every model gave up correct positions more often as the conversation went on, often while its own reasoning still held the right answer. | SPINE 2026, arXiv:2609.09090 | The critic never argues with the idea's author. Rerun only with new evidence. |
| GPT-5 tried to "prove" false statements supplied by users 29% of the time. | BrokenMath, NeurIPS 2025, arXiv:2510.04721 | Present ideas neutrally, never "here's our great idea". |
| Feedback on the same argument became more positive when the user said they liked it. | Sharma et al., ICLR 2024, arXiv:2310.13548 | Hide who wrote an idea and how excited anyone is about it. |

### Self-critique is confirmation, not verification

| Finding | Source | Design consequence |
|---|---|---|
| 85–95% of reasoning models' *local* rechecks in math just confirmed the previous step. Suppressing them selectively kept accuracy better than suppressing all of them, and strategic rethinking was explicitly protected. ⚠️ *Corrected by Astra review* | "Self-Verification Dilemma" 2026, arXiv:2602.03485 | "Check your own answer" steps are mostly wasted, but real revision can help: the diversity paper's winning method included one. Don't ban revision. Don't use self-checks as gates. |
| Better generation does not bring better self-verification ("persistent capability asymmetry"). | arXiv:2602.07594 (2026) | Same as above. |
| Self-critique collapses performance. Sound *external* verification gives large gains. | Stechly, Valmeekam, Kambhampati, ICLR 2025, arXiv:2402.08115; Huang et al., ICLR 2024 | Checks must come from outside the model: customers, prices, base rates, the three of you. |
| Multi-agent debate beat plain chain-of-thought in ≤20% of 36 setups. Mixing model families was the one reliable help. | arXiv:2502.08788 (2025); Smit et al., ICML 2024 | No bull-vs-bear debate machinery. |

### Synthetic customers: weakest exactly where startups live

| Finding | Source | Design consequence |
|---|---|---|
| For incremental Colgate products in a mature category, synthetic ratings reached 90% of human test–retest reliability. | Maier et al. 2025, arXiv:2510.08338 | Fine for mature categories, which is probably not where you'll be. |
| Across 19 preregistered studies, AI "digital twins" correlated with their real humans at only ~0.2. Generic demographic personas did as well as the twins. | Toubia, Peng et al. 2025, arXiv:2509.19088 | Don't use them to predict individuals. |
| No LLM beat simple non-LLM baselines at the individual level. Models inflate gaps between segments 2–4x and point to the wrong segment in about half of US cases. Bigger models didn't fix it. | arXiv:2607.26348 (2026) | Never use synthetic customers to choose a segment. |
| Pricing: totals were usable, but subgroup errors ran 10–30 points. A real calibration sample of 50–300 people cut the bias 83–94%. | Tigre & Souto 2026, arXiv:2609.13148 | If you ever use them, calibrate against real people. |
| Fine-tuning on past surveys didn't help for new product categories. | Brand, Israeli, Ngwe, HBS WP 23-062 | Same as above. |

### Forecasting and predicting startup success

| Finding | Source | Design consequence |
|---|---|---|
| By mid-2026, the best retrieval-backed AI forecasting systems were statistically indistinguishable from superforecasters on ForecastBench *(snippet; the human baseline is from 2024)*. | Forecasting Research Institute 2026; Karger et al., ICLR 2025 | AI is strong on questions with clear, checkable outcomes. "Will this startup work?" isn't one. |
| Zero-shot Gemini models did worse than logistic regression at predicting Series A funding from Product Hunt launches. | PHBench 2026, arXiv:2605.02974 | Don't ask a model for a "probability of success". |
| Plain LLMs over-predict startup success, largely by taking founders' claims at face value. | SSFF, arXiv:2405.19456 | Anchor on base rates (most ventures fail) and discount self-reported claims. |

### Web research: grounded but check the links

| Finding | Source | Design consequence |
|---|---|---|
| 3–13% of URLs cited by LLMs and deep-research agents were fabricated, and deep-research agents fabricated more than search-augmented chat. | Rao, Wong, Callison-Burch 2026, arXiv:2604.03173 | Check automatically that every cited link resolves. |
| Only 13–16% of consulting-style deliverables from top deep-research agents met the acceptance bar. | Asthana et al. 2026, arXiv:2605.17554 | Research output is raw material, not a finished analysis. |
| Sakana's AI Scientist labelled established techniques as novel. | Beel, Kan, Baumgart 2025, arXiv:2502.14297 | "No competitors found" means *low confidence*, not a green light. |

---

## 3. Why LLM-built systems bloat into process

| Finding | Source | Design consequence |
|---|---|---|
| "Every component in a harness encodes an assumption about what the model can't do on its own." On newer models, several of Anthropic's own components became "dead weight". Agents "praise their own work regardless of quality", so **one** separate, skeptical evaluator helped. | Anthropic Engineering, "Harness design for long-running application development", Mar 2026 | Write down the assumption behind each component, retest it on each model upgrade, and use one separate skeptic, not a chain of gates. |
| Add complexity "only when it demonstrably improves outcomes". | Anthropic, "Building Effective Agents", Dec 2024 | The default is a single well-prompted call. |
| Rules-based feedback is best. LLM-as-judge is "generally not a very robust method". | Anthropic, "Building agents with the Claude Agent SDK", Sep 2025 | Prefer deterministic or real-world checks. |
| 14 failure modes across 1,600 traces. ~42% of failures come from specification and system design, ~37% from misalignment between agents. | Cemri et al. (MAST), NeurIPS 2025, arXiv:2503.13657 | More stages means more handoffs that can break. |
| On sequential tasks, every multi-agent variant scored 39–70% worse than a single agent. | Kim et al. (Google), arXiv:2512.08296, Dec 2025 | Evaluating an idea is sequential reasoning, so use one agent. |
| A ~100-line agent scores >74% on SWE-bench Verified. Vercel cut 80% of an agent's tools and success went from 80% to 100%. | mini-SWE-agent 2025; Vercel, Dec 2025 | Minimal scaffolds win. |
| In RL training on medical responses, training against a rubric made outputs more "complete" but less concise, less correct and lower quality overall. A rubric verifier preferred the trained model 85.8% of the time; rubric-free judges preferred the base model 78.4% of the time. ✅ *Astra-checked* | arXiv:2605.12474 (2026) | A rubric graded by an LLM rewards output that *looks* thorough (Goodhart's law). This is about training models, so treat it as an analogy for LLM-graded gates, not proof that checklists hurt founders. |
| Judges were >50% more likely to wrongly mark a rubric item as satisfied when the output was their own. | Pombal, Rei, Martins 2026, arXiv:2604.06996 | LLM-graded gates pass themselves. |
| Agent-written PRs add redundancy and technical debt, yet reviewers react to them *more* positively. | "More Code, Less Reuse", MSR 2026, arXiv:2601.21276 | Humans also mistake more structure for more quality. |
| RLHF reward gains are mostly driven by longer responses. | Singhal et al. 2023, arXiv:2310.03716 | Models are trained toward "more". |

## 4. Classic startup practice (non-LLM)

- **Premortem** (Klein, HBR 2007). Assume the idea has already failed and explain why. Imagining it as already happened improves identification of reasons by ~30%.
- **Reference-class forecasting / outside view** (Lovallo & Kahneman, HBR 2003). Start from the base rate for similar ventures, then adjust.
- **The Mom Test** (Fitzpatrick 2013). Ask about their life and specific past behavior, not about your idea or their opinions of the future.
- **Pretotyping and XYZ hypotheses** (Savoia, *The Right It*, 2019). Most new ideas fail even when well executed. Frame each idea as "at least X% of Y will Z" and test it before building.

## 5. What's known about the current frontier models (as of 2026-09-25)

The published studies above mostly test older models. For example, the 2026 *Science* sycophancy paper tested GPT-5, GPT-4o and Claude Sonnet 3.7, and "Artificial Hivemind" tested GPT-4o and the Claude 3 family. No study found has re-run these on GPT-6 Astra (released Sep 3, 2026), Claude Fable 5.1 (Sep 1) or Claude Opus 5.5 (Sep 22). Everything below comes from search summaries of system cards and third-party evals. None of it was read in the primary documents, so treat it as *reported*, not verified.

| Failure mode | Status on current models | What's reported |
|---|---|---|
| Hallucination | **Improved on benchmarks** | GPT-6 Astra: 4.2% on OpenAI's internal eval, down from 12.2% for GPT-5.6 Sol. Its hallucination rate on the Artificial Analysis Omniscience benchmark fell from 92% to 51% at max effort. The system card warns against reading these as production error rates, so they say nothing direct about accuracy on business research. ✅ *Astra-checked* |
| Sycophancy / caving under pushback | **Persists; varies by vendor** | Fable 5.1 scores 85.3% on the MASK honesty test, against 94.8% for Opus 5, and its card calls it "less honest under pressure than recent Claude models". **Opus 5.5's own card reports 87.4%, also below Opus 5**, so the earlier claim that it is "the strongest Claude on honesty" was wrong. On a 25-turn pushback test of the previous generation (SPINE), models abandoned correct positions 20–62% of the time. ⚠️ *Corrected by Astra review* |
| Self-preference as a judge | **Persists** | The Fable 5.1 system card says it "gives better grades to Claude models". |
| Correlated errors across models | **Persists** | Frontier LLM forecasts are highly correlated with each other. Adding a different model family adds the most value (arXiv:2606.29661). |
| Homogenization / low diversity | **Unknown for current models** | No 2026 diversity benchmark covers Astra, Fable 5.1 or Opus 5.5. Earlier models show it. Verbalized Sampling reports *more capable* models benefit more from diversity prompting. |
| Over-optimism | **Persists (indirect)** | Six frontier models were "substantially overconfident" in scientific forecasting (arXiv:2605.22681). |
| Verbosity | **Unknown for current models** | YapBench (arXiv:2601.00624) found verbosity does not improve with newer models. |

**What this means for the design:** hallucination has improved on benchmarks, but that doesn't establish accuracy on business research, so research output still needs a verbatim quote and a working link for every claim. Sycophancy, self-preference and correlated errors have not gone away. A second vendor for critique adds another perspective, but it doesn't guarantee the two models make independent errors.
