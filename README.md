# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->
Donald Witherspoon - Corpus: `campus_life`

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

# write down number - 26

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

This project is a small retrieval-augmented generation (RAG) system for answering questions about campus life. It loads documents from the campus_life corpus, splits them into chunks, retrieves the most relevant chunks for a question, checks whether the question is relevant to the corpus, and then generates an answer using the retrieved information.

The system also names the source document used for the answer and refuses questions when the best retrieved result is not relevant enough. I tested it with five in-scope questions and five out-of-scope questions. The final relevance cutoff is 0.6, and I kept top-k at 5.

## Chunking Strategy

**Chunk size:**
About 350 characters
**Overlap:** 0 characters

I chose about 350 characters because the campus_life corpus contains many short posts, but some documents contain several separate pieces of information such as dining hours, wait times, costs, or course workload. The starter's fixed 800-character window produced 88 chunks from 88 documents, so almost every document stayed as one chunk. I changed the chunker to keep paragraphs together when possible and to split longer paragraphs at sentence boundaries. This produced 117 chunks and made the chunks more focused while keeping complete pieces of information together.

I used 0 characters of overlap because the documents are generally short and the new chunker keeps complete paragraphs or sentences together. I did not want to duplicate information between chunks unnecessarily.


<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: admin_add_drop_deadline.txt`` — produced by: chunker.py::split_documents ``

On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.

```
```

**Chunk 2** — source: course_cs_210.txt`` — produced by: chunker.py::split_documents``

CS 210 Data Structures

I'm a junior and I've done this twice now. Format is lecture with weekly labs; slides go up after class, not before. Assessment: two midterms and a final, all drawn from lecture material rather than the textbook. Midterms are curved, the final is not.

Expect 8 to 10 hours a week outside class.

```
```

**Chunk 3** — source: course_math_220_workload.txt`` — produced by: chunker.py::split_documents``

Workload for MATH 220 Linear Algebra

People keep asking so: 6 to 8 hours a week, almost all of it on problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.

```
```

**Chunk 4** — source: dining_the_ridgeway_cafe.txt`` — produced by: chunker.py::split_documents ``

The Ridgeway Café

Second-year here. Wait times: 10 to 15 minutes at 12:30, none after 2:00. The thing worth going for is the only place on campus with real espresso. The thing to know is that seating is tight; about 40 seats for a building of 900.

Hours are 7:00am to 4:00pm weekdays only. Costs declining balance only, no meal swipes.

```
```

**Chunk 5** — source: housing_innisfree_hall_laundry.txt `` — produced by:chunker.py::split_documents ``

Laundry in Innisfree Hall

Machines take $1.75 wash, $1.75 dry, app-based. There are eight washers and six dryers for the building, which is the wrong ratio and means the dryers back up on Sunday evenings.

Best time to do laundry here is Tuesday or Wednesday morning. Sunday after 6pm you will wait.
```
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:**
Is the housing lottery random?

**Answer:**
Answer: The housing lottery is not random in the way most people assume. Rising sophomores get a number drawn at random, but juniors and seniors are ordered by accumulated credit hours first, with random tie-breaking.

```
```
Source: admin_housing_lottery.txt

**My relevance cutoff:**
0.6

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question                                                                        | In corpus? | Best distance |
| ------------------------------------------------------------------------------- | ---------- | ------------: |
| Is the housing lottery random?                                                  | Yes        |        0.1801 |
| How many hours a week do students say CS 340 takes near the end of the project? | Yes        |        0.2866 |
| What are the wait times at Kestrel Commons during lunch?                        | Yes        |        0.1729 |
| How much does laundry cost at Morrow House?                                     | Yes        |        0.1929 |
| What are the weekend hours at Kestrel Commons?                                  | Yes        |        0.4233 |
| What is the capital of Mongolia?                                                | No         |        0.8246 |
| How do I change the oil in a diesel engine?                                     | No         |        0.9340 |
| Who won the 1994 World Cup?                                                     | No         |        0.8736 |
| What is the recommended dosage of ibuprofen for a headache?                     | No         |        0.8401 |
| How do I write a for loop in Rust?                                              | No         |        0.8907 |


## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->


I used AI to help me reason through the retrieval and chunking parts of the project, but I tested the suggestions against the actual project instead of copying them without checking.

1. Chunking: I asked AI to help me think through why the starter chunking approach might not be giving the best retrieval results. The starter used a fixed character-based approach, so I changed split_documents() to be paragraph-aware and to keep sentences together when possible. I tested the new chunker and got 117 chunks instead of the starter's 88, with an average chunk size of about 238 characters.

2. Retrieval and grounding: I used AI to help inspect the retrieval distances and decide whether top-k and the relevance cutoff needed to change. We compared the five in-scope questions with five out-of-scope questions. The in-scope best distances ranged from 0.1729 to 0.4233, while the out-of-scope best distances ranged from 0.8246 to 0.9340. Based on that gap, I kept the cutoff at 0.6 and top-k at 5. I also checked the GROUNDING_INSTRUCTION and kept it unchanged because it already required the model to use only the provided documents, avoid guessing, and name the source file.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. Chunks contain complete pieces of information without cutting off useful sentences | 4 of 5 | — | — | — | — |
| 5. Final answer directly answers the question using information from the retrieved chunks | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunk contains the answer | MET | All 5 of 5 test questions retrieved information containing the expected answer in each evaluation run, exceeding the target of 4 of 5. |
| 2 | Every answer names a source | MET | All 5 answers identified at least one source document in each evaluation run, meeting the target of 5 of 5. |
| 3 | Gate stops out-of-corpus questions | MET | The relevance gate refused 5 of 5 out-of-corpus questions in all three evaluation runs, exceeding the target of 4 of 5. |
| 4 | Chunks contain complete pieces of information without cutting off useful sentences | — | I have not judged this criterion yet because it requires inspecting 5 actual chunks. |
| 5 | Final answer directly answers the question using information from the retrieved chunks | MET | All 5 test questions received answers that directly addressed the question in the observed runs, exceeding the target of 4 of 5. |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

     ## **Diagnoses**

The main issue I found was at the **chunking** stage. The starter's fixed 800-character window produced 88 chunks from 88 documents, so almost every `campus_life` post stayed as one chunk even when the post contained several separate pieces of information, such as dining wait times, hours, and costs. This could make retrieval less precise because a chunk could match a question based on only one part of the post while also containing unrelated information.

I did not find a loading problem because all 88 documents were loaded. I also did not have evidence of an embedding or generation problem from the baseline runs. The main change I wanted to test was therefore making the chunks follow paragraph and sentence boundaries instead of treating each short post as one large fixed-size window.




## The Improvement

**What I changed:**
I replaced the fixed-size chunker.py::fallback_split strategy with my own chunker.py::split_documents function. My chunker tries to keep paragraphs together and combines nearby paragraphs only when they fit within about 350 characters. If a paragraph is too long, it groups complete sentences instead of cutting through a sentence. I used 0 characters of overlap because the campus_life documents are already short posts and I did not want to duplicate information between chunks.

**Why I picked it:**
The starter produced one chunk for nearly every document, even when a post contained multiple separate thoughts. My new strategy produced 117 chunks from the same 88 documents and keeps the information in complete paragraphs or sentences, which should make individual pieces of information easier for retrieval to match.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. Chunks contain complete pieces of information without cutting off useful sentences | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 5. Final answer directly answers the question using information from retrieved chunks | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

This addressed the chunking problem I identified earlier, where the starter's fixed 800-character windows kept almost every document as one chunk even when the document contained separate pieces of information.

I kept top-k at 5 and kept the relevance cutoff at 0.6. The retrieval test showed a clear separation between the questions covered by the corpus and the out-of-scope questions. The five in-scope questions had best distances from 0.1729 to 0.4233, while the five out-of-scope questions had best distances from 0.8246 to 0.9340. The 0.6 cutoff falls between these groups.

The grounding test also showed that the existing GROUNDING_INSTRUCTION was strict enough. The housing lottery answer used information from admin_housing_lottery.txt and named the source instead of using information from the unrelated retrieved chunks. I therefore did not change the grounding instruction.

The Milestone 4 changes did not increase the five-question answer success rate because the system was already answering all five correctly. Instead, the retrieval testing confirmed that the current top-k and cutoff separate covered questions from out-of-scope questions, while the grounding test confirmed that answers stay tied to the retrieved documents.

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->
None of the five acceptance criteria are still missed. The final checks reached 5 of 5 for each criterion across the three evaluation runs, and the relevance gate refused all 5 out-of-corpus questions in each run.

There are still things I could improve, such as adding automated scoring with scorer.py and testing more questions, but those are not failures of the five criteria I set for this milestone. I stopped here because the current evaluation showed that the system met the targets consistently.

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->

I would make the evaluation criteria more specific about how the results are measured. For example, Criterion 4 checks whether chunks contain complete pieces of information, but that required manual inspection because run_eval.py does not score it yet. I would define a more repeatable way to check chunk completeness so the result does not depend as much on manual judgment.

I would also include more than five in-scope questions if I were continuing the project. The five questions covered different parts of the corpus, but a larger test set would give more evidence that the retrieval and relevance cutoff work beyond these specific questions.

