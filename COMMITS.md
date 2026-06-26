# COMMITS.md

### THE TANDEM COVENANT OF CONVENTIONAL COMMITS

#### Being a Binding Instrument for the Adjudication of Commit Type Disputes

---

**PREAMBLE**

WHEREAS the engineers of Tandem (hereinafter "the Parties") have, on numerous occasions, engaged in protracted and unproductive disagreement over the proper classification of commits;

WHEREAS such disagreements have included, but are not limited to, whether the addition of a `console.log` constitutes a `feat`, a `fix`, a `chore`, or grounds for revocation of repository access;

WHEREAS the document at `conventionalcommits.org` provides only general guidance and does not, in the considered opinion of the Parties, sufficiently arbitrate edge cases involving the moral character of one's diff;

NOW, THEREFORE, the Parties hereby adopt this Covenant. By pushing to any branch, the committer ("the Author") agrees to be bound by its terms.

---

## ARTICLE I — DEFINITIONS

**1.1** "Commit" shall mean a single, atomic unit of change recorded in the version-controlled history. Commits which are not atomic shall be deemed non-conforming and may be subject to interactive rebase under Article XI.

**1.2** "User-Observable Behaviour" shall mean any behaviour that a non-engineering stakeholder, given a working build and ten (10) minutes of clicking, would be capable of perceiving. The opinions of QA engineers shall, for the avoidance of doubt, count as those of stakeholders.

**1.3** "Production" shall mean the environment from which revenue is derived, or in the absence of revenue, the environment from which embarrassment is derived.

**1.4** "The Diff" shall mean the totality of changes between two refs, inclusive of whitespace, regardless of whether such whitespace was introduced intentionally.

**1.5** "A Reasonable Reviewer" shall mean a hypothetical Tandem engineer who has had at least one (1) coffee and no more than three (3), and who is not the Author.

---

## ARTICLE II — THE PERMITTED TYPES

**2.1** The following types, and only the following types, are permitted at the head of a commit message:

- `feat` — a new feature
- `fix` — a bug fix
- `perf` — a performance improvement
- `refactor` — a change that neither fixes a bug nor adds a feature
- `style` — formatting, whitespace, and other changes that do not affect meaning
- `test` — changes confined to test files
- `docs` — changes confined to documentation
- `build` — changes to the build system or external dependencies
- `ci` — changes to CI configuration files and scripts
- `chore` — anything that does not fit the above, and which the Author cannot be bothered to categorise more precisely

**2.2** The use of any type not enumerated in Section 2.1 — including but not limited to `wip`, `misc`, `stuff`, `things`, `final`, `final_v2`, or `pls-work` — shall constitute a breach of this Covenant.

---

## ARTICLE III — THE DOCTRINE OF `feat` vs `fix`

This Article governs the most frequent source of disagreement among the Parties.

**3.1 — The Intent Test.** A change shall be classified as `fix` if, and only if, the behaviour prior to the change was _unintended_ by some reasonable party at the time of its original authorship. Where the prior behaviour was intended, but is now undesired, the change is `feat`.

**3.2 — The Ticket Test.** A change made in response to a ticket whose title begins with "Bug:", or whose Jira type is "Bug", shall be presumed `fix`. This presumption is rebuttable upon a showing that the ticket was miscategorised, which showing requires the agreement of the Project Lead.

**3.3 — The "But It Never Worked" Doctrine.** A change which causes a feature to function for the first time, where the feature was previously merged in a non-functional state, shall be classified as `fix`, notwithstanding the fact that, technically, nothing was broken because nothing ever worked. This is known among the Parties as the _Schrödinger's Feature_ doctrine.

**3.4 — The Adjacent-Feature Doctrine.** Where, in the course of fixing a bug, the Author also adds a small new capability, the commit shall be classified as `feat`, _provided that_ the new capability is necessary to the fix. If the new capability is not necessary to the fix, it must be split into a separate commit under Article XI. Bundling unrelated work is grounds for review rejection.

**3.5 — The Edge Case of Restoration.** Where a feature previously existed, was removed (whether by accident or by misguided cleanup), and is now reintroduced, the commit shall be classified as `feat` if the removal was intentional, and `fix` if the removal was an unintended consequence of another change. The git blame of the removing commit is conclusive evidence of intent.

**3.6 — The Validation Edge Case.** Adding input validation where none previously existed is `feat` if the validation enables new use cases, and `fix` if the absence of validation was producing crashes, data corruption, or security exposure. _"It could theoretically crash"_ is insufficient; the Author must demonstrate that it _did_ crash, or that a reasonable user _would_ cause it to.

**3.7 — The Error-Message Edge Case.** Improving the wording of an error message is `fix` where the prior message was misleading, and `chore` where the prior message was merely ugly.

**3.8 — Default to `fix`.** In genuine ambiguity, the Author shall prefer `fix`, on the basis that `fix` is the more humble classification and humility is a virtue in software engineering.

---

## ARTICLE IV — THE DOCTRINE OF `perf` [\[A\]](#appendix-a--on-the-false-friend-performance)

**4.1 — Measurement Requirement.** A commit shall not be classified as `perf` in the absence of measurement. The Author shall, in the body of the commit, cite at least one of:

(a) a benchmark, with before-and-after numbers;
(b) a profiler output;
(c) a production metric (e.g. p95 latency, bundle size, query plan cost);
(d) a measurement of memory or CPU usage from a representative workload.

Vibes, intuition, and _"this should be faster"_ shall not suffice. The phrase "I'm pretty sure" is expressly disallowed.

**4.2 — The Threshold of Materiality.** A change whose measured improvement is less than five percent (5%) on the relevant metric is `refactor`, not `perf`, unless the change is on a hot path and the Author can demonstrate that small improvements compound.

**4.3 — The Algorithmic Doctrine.** A change in algorithmic complexity (e.g., O(n²) → O(n)) is `perf` even without measurement, provided that the new complexity is _correct_ and the input sizes encountered in practice are large enough for the change to matter. A change from O(n²) → O(n) over an input of size three (3) is `refactor`.

**4.4 — The Edge Case of "Faster But Wrong".** A change that improves performance by removing necessary work (e.g., dropping a network call, skipping a validation) is `fix` or `feat` as appropriate, never `perf`, because the change has materially altered behaviour.

**4.5 — Bundle Size.** A reduction in shipped bundle size is `perf` if the change was made for that purpose. If the bundle reduction is incidental to a `refactor`, the commit remains `refactor` and the bundle reduction may be noted in the body.

---

## ARTICLE V — THE DOCTRINE OF `refactor`

**5.1** A `refactor` shall not, under any circumstances, change User-Observable Behaviour. Where User-Observable Behaviour is altered, even unintentionally, the commit shall be re-classified as `fix` (if the alteration is desirable) or reverted (if it is not).

**5.2 — The Test Suite Test.** A true `refactor` should not require changes to existing test assertions. Changes to test _implementations_ (e.g., updating mocks because an interface moved) are permitted; changes to test _expectations_ are evidence that the commit is not a refactor.

**5.3 — Renaming.** Pure renames (variables, files, types) are `refactor`. The mass-renaming of a public API constitutes a breaking change and shall be marked accordingly per Article IX.

**5.4 — Reorganisation.** Moving code between files, splitting files, or merging files, without behavioural change, is `refactor`. Where such reorganisation is performed solely to make a subsequent change easier, it shall be committed separately under the _Preparatory Commit Doctrine_ (Article XI, Section 11.3).

---

## ARTICLE VI — THE DOCTRINE OF `style`

**6.1** `style` is reserved for changes whose entire content is removed by a formatter. If a formatter would not produce the same diff, the commit is not `style`.

**6.2** The introduction or removal of a linter rule, even one whose enforcement produces only whitespace changes, is `build` or `ci`, not `style`, because the change to the rule itself is not whitespace.

**6.3** Run the formatter. Commit the result. Do not interleave `style` changes with substantive changes; the Reasonable Reviewer should not be required to disentangle them.

---

## ARTICLE VII — THE DOCTRINE OF `test`, `docs`, `build`, AND `ci`

**7.1 — `test`.** Changes confined entirely to files matching the project's test glob. A change that touches both production and test code is _not_ `test`; classify by the production change.

**7.2 — `docs`.** Changes confined entirely to documentation, comments, README files, JSDoc, and the like. Updating documentation _as part of_ a feature is permitted within a `feat` commit and does not require splitting.

**7.3 — `build`.** Changes to package.json (excluding `scripts` used only in CI), lockfiles, bundler configuration, compiler configuration, and the like. A dependency upgrade is `build`. A dependency upgrade that is required to fix a bug is `fix` with a note that a dependency was upgraded.

**7.4 — `ci`.** Changes to `.github/workflows/`, CI provider configuration, and scripts whose sole purpose is to be run in CI. A change to a script run both locally and in CI is `build`.

---

## ARTICLE VIII — THE DOCTRINE OF `chore`

**8.1** `chore` is the residual category. Before invoking it, the Author must satisfy themselves that no other type applies. The use of `chore` to avoid making a classification decision is an act of moral cowardice and shall be remarked upon in code review.

**8.2** Permissible uses of `chore` include: updating `.gitignore`, adding repository metadata, rotating credentials, housekeeping tasks that affect neither code nor configuration in a meaningful way.

---

## ARTICLE IX — BREAKING CHANGES

**9.1** A breaking change shall be indicated by appending `!` after the type/scope, _and_ by including a `BREAKING CHANGE:` footer in the commit body. Both are required; either alone is insufficient.

**9.2** A change is "breaking" if any of the following are true:

(a) a public API signature has changed in a non-additive way;
(b) a configuration option has been removed or renamed;
(c) a default value has changed in a way that could alter behaviour for existing consumers;
(d) a minimum runtime version has been raised;
(e) database schema changes that require manual migration;
(f) environment variables have been renamed or made required.

**9.3** Internal-only changes are not breaking, no matter how invasive. If no consumer outside this repository is affected, no breaking-change marker is required.

---

## ARTICLE X — SCOPES

**10.1** Scopes are optional but encouraged. The scope shall identify the part of the codebase affected. Permitted scopes include feature areas (`auth`, `billing`), package names in a monorepo (`web`, `api`), or sub-systems (`migrations`, `email`).

**10.2** Scopes shall be lowercase, kebab-case, and short. A scope longer than the description is presumed invalid.

**10.3** A commit affecting multiple scopes shall be split, unless the change is genuinely cross-cutting (in which case the scope may be omitted entirely).

---

## ARTICLE XI — COMMIT HYGIENE

**11.1 — Atomicity.** Each commit shall represent one logical change. If the Author finds themselves writing "and" in the description, they are likely committing two things.

**11.2 — The Subject Line.** The subject shall be in the imperative mood ("add", not "added" or "adds"), shall not exceed seventy-two (72) characters, and shall not terminate in a full stop.

**11.3 — The Preparatory Commit Doctrine.** Where a substantive change is made easier by a prior reorganisation, the reorganisation shall be committed first as `refactor`, and the substantive change shall follow. This produces a cleaner history and a more reviewable diff.

**11.4 — The Body.** Where the change is non-obvious, the commit body shall explain _why_, not _what_. The diff already shows _what_.

**11.5 — Attribution.** No commit shall include a `Co-Authored-By: Claude` footer or equivalent. Authorship of every line of code committed by an Author rests with that Author, per the doctrine of `README.md`, Section "How We Work", Item 1.

---

## ARTICLE XII — DISPUTE RESOLUTION

**12.1** Where two or more Parties disagree on the classification of a commit, and the disagreement cannot be resolved by reference to this Covenant, the dispute shall be referred to the Project Lead, whose decision shall be final and non-appealable.

**12.2** Where the Project Lead is themselves a Party to the dispute, the dispute shall be referred to whichever other engineer is currently in the office, or, in the absence of any such engineer, to a coin toss conducted in good faith.

**12.3** No Party shall persist in arguing a classification beyond the second round of code review comments. Continued argument shall be deemed bikeshedding and may be cited in performance review.

**12.4** This Covenant is amendable. Proposed amendments shall be submitted as a pull request to this file, with a commit type of `docs` (the meta-classification, being itself a documentation change, is hereby established by precedent).

---

## ARTICLE XIII — SEVERABILITY AND FORCE MAJEURE

**13.1** If any provision of this Covenant is found to be unenforceable, the remaining provisions shall continue in full force and effect.

**13.2** The Parties acknowledge that, in the event of a production incident requiring an immediate hotfix, strict compliance with this Covenant may be temporarily suspended. The hotfix shall be classified as `fix`, scoped as narrowly as possible, and the suspension noted in the post-mortem.

**13.3** Nothing in this Covenant shall be construed as preventing an engineer from doing the right thing in a hurry.

---

_Executed in perpetuity by the engineers of Tandem._

_This document supersedes all prior agreements, vibes, and watercooler consensus on the matter of commit classification._

---

## APPENDIX A — ON THE FALSE FRIEND "PERFORMANCE"

*\* Re: Article IV.* For the avoidance of all doubt, `perf` is reserved exclusively for changes affecting the *computational efficiency* of the software — that is, the speed of execution, the memory or CPU consumed, the bytes transferred over the wire, the size of the artifact shipped, or the latency observed by a user or downstream system.

`perf` is **not** a synonym for *"this feature now performs its job better"* in the colloquial sense. A change that causes a shield to bounce 75% of harmful attacks where previously it bounced only 50% is `fix` (if 50% was a bug) or `feat` (if 75% is a new capability), and shall not, under any circumstances, be classified as `perf` on the basis that the feature's "performance" has improved.

The word "performance" is hereby deemed a false friend. The Author shall mentally translate it to "speed/efficiency" before invoking Article IV. If the translation does not survive, the commit is not `perf`.
