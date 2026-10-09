# B. Information designer: Round 1 concepts

---

## B1. The Skerry Forecast

**1. One sentence:** You are the only weather forecaster for a small fictional archipelago. Each morning you post percentage chances from sparse station reports, and at dusk the islands judge whether your percentages were honest.

**2. 30 seconds of play**
- A map shows five islands and four stations. Tap the lighthouse card and it flips: "1011 hPa, falling 3 in 3 hours · wind SW 4 · cirrus, thickening."
- Drag along the western horizon strip to see the cloud layers: cirrus, then a grey sheet rising at the edge.
- The four wind arrows curl around something. Drag an "L" marker onto the sea where you think the low sits.
- The forecast sheet slides up: "Rain at Kell harbour before 18:00?", "Gale at North Pier?", "Ferry calm at 14:00?" Set the dials to 70 / 20 / 60 and tap **Pin to noticeboard**. A fisherman leaves his boat tied up, and the baker orders less flour.

**3. Central decision and how the game responds**
The decision is how sure to say you are, not what will happen. Scoring uses the Brier score: at 70%, rain costs you 0.09 and a dry day 0.49. Saying 50% always costs 0.25, which is safe but useless: villagers only act on forecasts of 20% or less, or 80% or more, so the village economy rewards bold calls that turn out honest. At dusk a dot lands on your reliability chart: "Your 70% days: 9. It rained on 5."

**4. Interesting interaction**
At 11:00 the westernmost croft reports: "Barometer bottomed at 1006, now rising. Wind veered S to NW. Sheep lying down." The low has swung north early, so rain will only clip North Pier, but with stronger wind. One fact moves two dials in opposite directions: Kell rain down, North Pier gale up. You can revise, but the noticeboard keeps every timestamped version, and your score averages your forecast over the open hours. A revision at 11:00 covers 7 of the 10 scored hours; one at 17:00 covers one hour. "Sheep lying down" is folklore. Each islander's sayings have a hidden reliability (inland gulls mean a little, the sheep mean nothing), which you learn only over weeks.

**5. What changes between rounds / reason to return**
Each day brings a new weather situation from a simplified model of fronts and lows (assume it produces believable sequences). The seasons shift the patterns, and new instruments arrive: a buoy, a barograph. You post in the morning and check at dusk. A wedding asks one high-stakes question a week ahead. Your long-run calibration curve is your reputation.

**6. Borrowed logic:** Weather forecasting: probability-of-precipitation forecasts checked with Brier scores and reliability diagrams, observations at fixed hours, and reading weather fronts from cloud sequences and wind shifts.

---

## B2. Stemma

**1. One sentence:** You get five to seven hand-copied versions of a short lost text, each damaged by scribes with recognisable habits, and you rebuild both the family tree of the copies and the original wording.

**2. 30 seconds of play**
- Six parchment strips, labelled A–F, hold the same three-line verse. The words that differ glow faintly.
- Tap the third glowing word. A column pops up: A "heron", B "heron", C "hero", D "heron", E "hero", F "herne".
- Drag "hero" into the **Shared Errors** tray. A thread links C and E in the tree pane below.
- In line two, copy D skips from "by the mill" to a later "by the mill" and loses the words between, an eye-skip. F has the same gap. Drag in an empty node, β (a lost copy), and hang D and F under it.
- On the **Reconstruction** line, tap the reading you believe is original at each difference.

**3. Central decision and how the game responds**
You decide which differences are errors. Only shared errors prove that two copies are related; shared correct readings prove nothing. The majority can be wrong, because scribes swap rare words for common ones, so "herne" (an old word for heron) may be original. When you mark an error, copies that share it must have a common ancestor in your tree, or their thread turns red: "C and E both read 'hero' but your tree separates them. Coincidence?" Some slips happen independently, so you also judge which errors are distinctive enough to count.

**4. Interesting interaction**
Partway through, copy G appears, a fragment from a book binding in older handwriting than any other: "…the herne at the mill…". Now "heron" in A, B and D is a shared modernisation, a shared error, so they become one family. Your tree flips: A drops two levels, and obscure F becomes the best surviving copy. Elsewhere, the Abbot of Wennick plants a nonsense word, "quillet", in every copy he makes; spot it once and all his descendants are exposed.

**5. What changes between rounds / reason to return**
A new text arrives daily (proverbs, recipes, verses), with new kinds of damage: doubled words, misread abbreviations, a lost page, or a scribe who copied from two sources, which puts a diamond in the tree. Scribes recur: Brother Aelric drops more letters near the bottom of a page, and Sister Maud "fixes" grammar. Restored texts build into the abbey's chronicle, which hides its own mystery. In **Scribe Mode** you copy a line from a two-second glimpse, and your real slips become a chain for other players to reconstruct.

**6. Borrowed logic:** Textual criticism (Lachmann's method): copies are linked only by shared errors, "prefer the harder reading", and named types of scribal error such as eye-skip.

---

## B3. Segregable

**1. One sentence:** You are a declassification reviewer. You swipe black bars across old documents so a requester with their own dossier can't piece together a protected secret, while releasing as much text as the rules require.

**2. 30 seconds of play**
- A typewritten 14-line memo from a fictional 1960s port town. A red tab reads: **PROTECT: identity of source PELICAN.** On the left, the requester's dossier lists what they already have: port records, a newspaper clipping, a ferry timetable.
- Drag a finger across "at the lighthouse". A black bar appears, and a wheel asks for an exemption code: E1 Security, E6 Privacy or E7 Investigation.
- The **Release** meter drops from 100% to 91%. It must finish above 70%.
- Tap **Run Requester** (3 runs per document). A pen strings phrases together on a corkboard overlay: "Thursday" + the timetable's single Thursday crossing + "arrived on the last boat". The string pulls toward PELICAN's empty photo slot and stops one link short.

**3. Central decision and how the game responds**
You decide which few words to remove. Cutting the name isn't enough, because harmless fragments combine with what the requester already knows. Cutting too much breaks the release minimum, and every bar needs a code that fits. An appeals clerk restores bars whose code doesn't apply, such as E6 Privacy on a date that identifies no one. The corkboard shows which chains of inference are still alive and how close each gets.

**4. Interesting interaction**
Line 9: "Agent met ████ at the pier after the 19:40 ferry." Every letter in the typewriter font is the same width, so the bar is exactly four characters wide. The dossier names three people seen at the pier that week: Ilse, Margit and Teodor. The pen measures the bar, writes "4 letters" and pins Ilse. To stop it, you widen the bar over "Ilse at the". Eleven characters could hide almost anything, but the Release meter drops 4%. Later levels add a ghost overlay: another agency's release of the same memo last year, with different bars. That version is already public, so your bars must cover whatever it left visible.

**5. What changes between rounds / reason to return**
Each round brings new documents (ship logs, telegrams, a diary, a restaurant bill with handwriting), dossiers, secrets and exemption rules. A campaign builds the town's public archive one release at a time. In a reverse mode, your finished redaction becomes another player's puzzle as the requester. You score if your redaction survives 24 hours uncracked, and they score by breaking it.

**6. Borrowed logic:** Government declassification and freedom-of-information review: line-by-line redaction tagged with exemption codes, the duty to release every "reasonably segregable" portion, and the mosaic theory that harmless facts can combine into protected ones.

---

## B4. Assumes Facts (strange one)

**1. One sentence:** You see a scene once, for four seconds, then testify while a lawyer slips false details into the questions; keep your memory clean, object at the right moments, and survive cross-examination built from your own answers.

**2. 30 seconds of play**
- A silent illustrated scene plays for four seconds: a corner shop at dusk, three people at a bus stop, a man in a green coat leaving with a paper bag, a dog tied to a post, a parked white van. Then black.
- A courtroom. "Of the four people at the bus stop, how many held umbrellas?" Buttons: 0 / 1 / 2 / 3+, a red **OBJECTION** (3 per trial) and a grey **I don't recall**.
- Four? You remember three… maybe. Tap OBJECTION → "Assumes facts not in evidence." **SUSTAINED**, and your credibility meter rises.
- "Was the dog's leash red or blue?" This one is honest. Tap "blue", and the transcript adds: *WITNESS: Blue.*
- "How fast was the van going when it smashed into the bin?" A speed slider appears. The van was parked… wasn't it?

**3. Central decision and how the game responds**
Trust your memory or the confident question. The lawyer knows the true scene and mixes honest assumptions (bait for wasted objections) with planted ones. An overruled objection costs more credibility than "I don't recall". If you answer a planted question without objecting, the false detail goes into the transcript under your name.

**4. Interesting interaction**
In phase two, the other side cross-examines you from your transcript. Had you answered the bus-stop question instead of objecting, you'd hear: "You said the van was doing 15 miles per hour, and that four people waited at the bus stop. Describe the fourth." Defend each plant you accepted by inventing details, or admit the contradiction and lose credibility. After the verdict, a slow-motion replay shows every accepted detail, a fourth commuter and a moving van, as ghosts that fade one by one. The man in the green coat was innocent; the bag was his. Whether the jury believed that depended on you.

**5. What changes between rounds / reason to return**
Scenes are assembled from a large library of parts (assume they don't repeat). Lawyers have styles: one plants in the first question, one waits until you're tired, one asks true questions in an accusing tone. Early scenes are silent, so any question about a sound is a plant, until chapter two adds sound. Appeal hearings three days later revisit Monday's scene with Monday's transcript, where an accepted plant may now feel like a memory. A profile tracks your weak spots: "You accept planted colours 40% of the time, planted numbers 10%." In two-player mode, one player sees the true scene and writes the questions.

**6. Borrowed logic:** Trial examination (objections to leading questions and assumed facts, impeachment by prior inconsistent statements) combined with the eyewitness misinformation experiment's sequence: scene, misleading questions, later test.
