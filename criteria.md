# Acceptance criteria — FitFindr

Five criteria that say what "working" means for this agent, written in unit 3
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"The agent handles errors"* is an opinion.
*"When search returns nothing, the agent stops before calling the second tool,
in 5 of 5 tries"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter one. A reason that says something about your tools, your loop, or the
data earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

**Two are written for you. You write three.**

---

## 1. A matching query completes all three tools

Given a query that matches at least one listing, the agent completes all three
tool calls and returns a fit card — in at least 4 of 5 tries.

**Why this target:** The query is in plain language and some phrasings might miss during the parsing and passing down.
 
---

## 2. An impossible query stops before the second tool

Given a query that matches no listings, the agent stops before calling
`suggest_outfit` and returns a message naming what to change — 5 of 5 tries.

**Why this target:** The agent should not create hallucination and go through all tools with broken states.

---

## 3. An item reaching each tool is identical to the one selected

In 5 of 5 completed runs, the dict passed into `suggest_outfit` and the dict passed into `create_fit_card` each equal session["selected_item"] field for field, compared against a deep copy taken before the call. `selected_item` equals `search_results[0]`.

**Why this target:**: `selected_item` is the only thing linking three tools, and a wrong item looks like a bad outfit rather than a state bug. 5 of 5 because no model is involved in picking or passing the item, so one miss is a real bug.


---

## 4. A fit card caption should satisfy all mentioned information provided in the query

In 5 of 5 completed runs, the caption string returned by the fit card should be between two to four sentences only. The caption should state the item with details and its price and platform once each, if any of these fields is non-empty.


**Why this target:** Since the string returned by the fit card can differ from the same input, the information contained in the caption should be consistent even though the format may look different every run.  



---

## 5. An empty wardrobe does not stop the run

Given a query that matches at least one listing and `get_empty_wardrobe()`, in 5 of 5 runs: no exception is raised, `session["error"]` is `None`, `session["outfit_suggestion"]` is a non-empty string of general styling advice, and `session["fit_card"]` is still 2 to 4 sentences. The suggestion does not refer to items the user owns (e.g. "your black jeans").

**Why this target:** Only `suggest_outfit` reads the wardrobe, and `create_fit_card` depends on its output, so those two are where an empty wardrobe can break the run. The no-raise and non-empty outcomes are deterministic contract points, so 5 of 5 is reasonable. The model only varies in wording, and the no-invented-items check is there because with nothing to draw on a model may make up a wardrobe.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 4 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 4. Something about the fit card

         The fit card is different every time.

         **Why this target:** ...

         > **Revised in unit 4:** For 5 different items, the 5 fit cards share
         > no opening sentence.
         >
         > **Why revised:** "different" wasn't checkable — two cards that
         > differed by one word still counted. The new version is something I
         > can actually score.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said the empty search stops it 5 of 5 times, but I got 3 of 5,
            so 3 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
