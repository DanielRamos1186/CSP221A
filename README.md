# CSP221A Remedial Coding Set

Ten new coding problems (5 per section) for students who did not pass the original Part 2 coding exam (`references/previous exam/exam-original.md`). Each section (BSCS4A, BSCS4B) has its own set of 5 problems so that an instructor can assign one problem per student (to discourage copying) or all five, with every problem roughly equal in difficulty and no problem sharing a domain, dataset shape, function/class name, or primary failure rule with any other problem in this set — including across sections.

## Assumptions

These were not specified and had to be decided to move forward:

1. **10 total problems, not 5.** The instructor asked for a separate `remedial/problem 1`–`problem 5` folder under *each* of two sections (BSCS4A, BSCS4B), for 10 problems total. To keep sections from being trivially comparable, each section's 5 problems cover the same 5 skill categories as the other section (OOP, Pandas cleaning, NumPy vectorization, pure Python, Pandas + sets/asserts) but with a different domain, dataset, and function/class names each time.
2. **No sklearn; hardest topic capped at Week 3 Chapter 4 (Pandas Data Cleaning).** The instructor removed sklearn from scope after reviewing the first draft. Problem 3 (originally cosine-similarity semantic search) was redesigned as a pure-NumPy vectorization problem (broadcasting, `np.where`, `.mean(axis=...)`) with no vectors/embeddings framing. Problem 5 (originally `train_test_split`) was redesigned as a second Pandas-cleaning problem layering in Week 1 sets and Week 2 `assert` statements, with no ML-splitting framing. Week 3 Chapters 5 and 6 (Vectors & Similarity, Train/Test Splitting) are out of scope for this remedial set.
3. **Libraries are not restricted to a fixed list.** Students may use any Python standard-library module, plus Pandas/NumPy where a problem's own "Libraries" section lists them, if it genuinely helps. No sklearn for this remedial set.
4. **Submission is one commit.** Each problem's "Submission" section asks for a single commit with a clear message, rather than a multi-commit history — a deliberate simplification from the original exam's Git-history expectations, per instructor direction.
5. **Deadline and late policy are placeholders.** Every `instructions.md` has `[INSTRUCTOR TO FILL IN]` for both; fill these in before distributing.
6. **All datasets, expected outputs, and console output shown in each `instructions.md` were taken from an actual run of the matching `answer_key.py`** on Python 3.12 with `pandas`, `numpy`, and `scikit-learn` available in the environment (scikit-learn is installed but unused by the final problem set — it was only needed during the discarded first draft). No expected output was hand-typed or guessed.
7. **The word "scores" and the 0–100 numeric range were avoided everywhere**, along with the exam's names (Amara, Leo, Priya, Sam, Jade) and its `Student` class, per the uniqueness constraint.

## Repository Layout

```
BSCS4A/
  remedial/
    problem 1/   Product Inventory Validator      (instructions.md, answer_key.py)
    problem 2/   Sensor Reading Cleaner            (instructions.md, answer_key.py)
    problem 3/   Fitness Challenge Lap Tracker     (instructions.md, answer_key.py)
    problem 4/   Support Ticket Triage             (instructions.md, answer_key.py)
    problem 5/   Customer Feedback Response Auditor(instructions.md, answer_key.py)
    submission/  (empty — where a student's own submission would go, if collected here)
BSCS4B/
  remedial/
    problem 1/   Library Book Catalog Validator
    problem 2/   Delivery Package Weight Cleaner
    problem 3/   Warehouse Robot Battery Cycle Tracker
    problem 4/   Volunteer Shift Signup Parser
    problem 5/   Employee Training Log Auditor
    submission/
```

`instructions.md` in each folder is the student-facing handout. `answer_key.py` is the instructor's tested solution — every dataset, printed line, and error message in `instructions.md` was copied verbatim from actually running that file.

## Difficulty Audit

Counts for the original exam were taken by reading `references/previous exam/exam-original.md` directly. Counts for the new problems are identical (within the stated tolerance) across both sections, since each pair (e.g. Problem 1 in BSCS4A and BSCS4B) was built to the same specification with a different domain.

| Metric | Original exam | New problems (target) | New problems (actual, each) |
|---|---|---|---|
| Graded requirements | ~10 (score validation, lock/add_score errors, `__str__`/`__repr__`, Pandas clean+dedupe, batch-build with failure collection, `np.mean` average, lambda-sorted ranking) | ~8 | 8–9 |
| Integration points (one stage's output feeds the next) | ~4 (raw dict → Pandas clean → Student build → NumPy average → lambda ranking) | ≤2 | 2 |
| Printed outputs | 5 | ≤4 | 4 |
| Functions/classes to write | ~6 (`Student`, 2 exceptions, combined clean-and-build function, ranking function, plus `.lock()`/`.add_score()`/`.average()` methods) | ≤5 | 3–5 |
| Concept families | 7 (OOP, exceptions, dunders, Pandas, NumPy, lambda, Git) | 4–5 | 4–5 |
| Unresolved ambiguities | ≥2 (e.g. whether duplicates are dropped before or after score parsing; what "no valid scores" means for an empty list) | 0 | 0 — every rule states an exact, testable outcome for every trap row |

### Per-problem detail

| Problem | Primary skill(s) | Concept families | Functions/classes | Integration points |
|---|---|---|---|---|
| P1 — Inventory/Catalog Validator | OOP: `@property` validation, exception hierarchy, `@classmethod`, dunders | 4: classes, custom exceptions, dunders, lambda sort | 5 (main class, 2-level exception hierarchy, `from_dict`, `build_*`, `rank_by_*`) | 2 (dict → object via classmethod; objects → ranked list via lambda) |
| P2 — Sensor/Package Cleaner | Pandas: string cleaning, `to_numeric(errors="coerce")`, dedupe, `.apply()`, `np.where` | 4: Pandas cleaning, string methods, vectorized flagging, file-read exception handling | 1 (single `clean_*` function; complexity is in the pipeline, not function count) | 2 (raw dict → cleaned DataFrame; cleaned DataFrame → derived columns/filter) |
| P3 — Lap/Battery Tracker | NumPy: broadcasting, vectorized `.mean()`/`.where()`, custom exceptions, dunders | 4: NumPy vectorization, broadcasting, custom exceptions, batch-building without crashing | 4 (2 exceptions, 1 class, 1 `build_tracker` function) | 2 (raw rows → tracker via validated batch build; tracker → vectorized stats/flags) |
| P4 — Ticket/Signup Triage | Pure Python: decorators, generators, sets, logging | 5: decorators/closures, generators, custom exceptions, `try/except/else/finally`, sets & dict comprehension | 5 (exception, decorator, parse function, generator, count function) | 2 (raw lines → cleaned dicts via decorated parser + generator; cleaned dicts → counts/set lookups) |
| P5 — Feedback/Training Auditor | Pandas + Week 1 sets + Week 2 `assert` | 5: Pandas cleaning, `.apply()`/lambda, sets, `assert`, custom exceptions | 3 (exception, `categorize`, `build_*_report`) | 2 (raw rows → cleaned DataFrame with sets/categories; cleaned DataFrame → group-size check + set lookup) |

All five categories land within 1 point of each other on every metric above, so any single problem — or any combination — can be assigned without materially changing the workload.
