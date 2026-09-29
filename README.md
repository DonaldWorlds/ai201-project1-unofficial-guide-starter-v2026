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


In this unit, I used AI to help inspect the results and compare the before and after runs. It helped me identify that the chunking change increased the number of chunks from 88 to 117, while the five-question pass/fail results stayed the same. I used that comparison to avoid claiming that the change improved the system when the evaluation did not show an improvement.

I also used AI to help organize the Milestone 3 diagnosis and check that the Milestone 4 write-up clearly connected the change to the evaluation results. The final conclusions were based on my actual project runs and the assignment criteria.

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
| 4. Chunks contain complete pieces of information without cutting off useful sentences | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
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
| 4 | Chunks contain complete pieces of information without cutting off useful sentences | MET | I inspected the five sample chunks produced by chunker.py::split_documents. The chunks preserve complete pieces of information and do not cut off useful sentences. |
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



I did not have any misses in the evaluation runs. All five criteria were MET, so I do not have a failed question that I can honestly assign to a specific pipeline stage.

The results do show one area that could still be improved: **chunking**. The starter's fixed 800-character approach produced 88 chunks from 88 documents, meaning most `campus_life` posts remained as one chunk even when they contained multiple separate pieces of information. My concern is that larger chunks can contain unrelated information, which may make retrieval less precise. However, because my retrieval and answer criteria still passed, I cannot call this a demonstrated failure.

I also did not find evidence of a loading, embedding, retrieval, or generation failure in these evaluation runs. The system successfully loaded all 88 documents, retrieved information containing the expected answers, identified sources, rejected the out-of-corpus questions, and produced answers that directly addressed the test questions.

Because I missed nothing, I should also examine whether my targets were too safe. Criterion 4 is the one I would tighten. Instead of checking five sample chunks manually, I would require the chunking test to verify that a larger sample of chunks preserves complete sentences and useful information without unnecessary unrelated material. This would make the criterion better at detecting an actual chunking problem.





## The Improvement

**What I changed:**
I replaced the fixed-size chunker.py::fallback_split strategy with my own chunker.py::split_documents function. My chunker tries to keep paragraphs together and combines nearby paragraphs only when they fit within about 350 characters. If a paragraph is too long, it groups complete sentences instead of cutting through a sentence. I used 0 characters of overlap because the campus_life documents are already short posts and I did not want to duplicate information between chunks.

**Why I picked it:**
The starter produced one chunk for nearly every document, even when a post contained multiple separate thoughts. My new strategy produced 117 chunks from the same 88 documents and keeps the information in complete paragraphs or sentences, which should make individual pieces of information easier for retrieval to match.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

**Before run:** `results/run_2026-09-28_1956_before.md`

**After run:** `results/run_2026-09-28_2018_after.md`

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

The chunking change did not improve the measured acceptance criteria. Before the change, the starter chunker produced 88 chunks from 88 documents. After the change, my chunker produced 117 chunks from the same 88 documents.

The before run had 5 of 5 in-scope questions answered across all three runs and refused 5 of 5 out-of-scope questions. The after run had the same results: 5 of 5 in-scope questions answered across all three runs and 5 of 5 out-of-scope questions refused.

The retrieval distances changed for some questions, but the pass/fail results did not change. Therefore, based on this five-question evaluation, the new chunking strategy did not improve the measured criteria. I kept the change because it was the one improvement tested for Milestone 4 and the experiment gave me a measurable result rather than assuming the change helped.


## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

None of the five acceptance criteria are currently missed in the final evaluation. The system answered all five in-scope questions, named sources, refused all five out-of-scope questions, and passed the manual chunk-completeness check.

There are still limitations. The evaluation only uses five in-scope questions, so the results are not enough to show that the system works reliably across the entire corpus. Criterion 4 is also judged manually rather than by an automated scorer. In the next unit, I would expand the evaluation set and make the chunk-completeness check more repeatable. I stopped here because the Milestone 4 experiment did not produce a measurable pass/fail improvement, and the assignment's required evaluation was already complete.

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->

I would write Criterion 4 more specifically in the next unit. "Chunks contain complete pieces of information without cutting off useful sentences" requires a manual judgment, so different evaluations could interpret it differently. I would define a clearer test for whether a chunk preserves the information needed to answer a question. I would also use more than five in-scope questions so the evaluation gives stronger evidence about the system's overall retrieval quality. As well as finishs my project early so I wont get 503 errors during the day.

