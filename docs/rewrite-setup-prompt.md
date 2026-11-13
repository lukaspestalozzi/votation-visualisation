# Task: set up my spec-driven AI workflow in this repository

I develop my project "Sunshine" (lukaspestalozzi/sunshine) with a workflow in which no code is
written without an approved OpenSpec change, and in which the repository itself tells an agent in
a fresh session everything it needs to know. Set up the same workflow here.

This task is scaffolding only: no product code and no first feature. It ends with a pull request
that I review.

If you can read lukaspestalozzi/sunshine in this session, use its files as the reference for form.
Its content (Android, Kotlin, maps, sun computations) is specific to that project and does not
belong here. If you cannot read it, this prompt describes everything you need.

## About this project

- What it is and for whom: A web app that shows how Switzerland voted in national popular votes,
  municipality by municipality, and lets the user ask "what would the result have been if only
  municipalities of a certain kind had voted?". The user picks a vote, sets a lower and an upper
  limit on characteristics of municipalities (share of foreigners, age structure, party strength,
  language, ...), and sees the map and the national verdict recalculated from the municipalities
  that remain. It is for the curious public: anyone who knows what a Swiss popular vote is,
  without statistical training and without a manual. Its core value is directly steering a real
  vote result and seeing which kinds of municipalities carry it. Working title: "Popular Vote
  Explorer"; the name is not decided. The full non-technical spec is in the appendix of this
  prompt.
- Hard constraints (what must always hold):
  1. Matches the official results. With no limit set, every covered vote shows exactly the
     official federal result: the yes votes, the valid votes and the yes share of the voters, and
     the number of cantonal votes in favour.
  2. Follows the real voting rules. The majority of voters is more than half of the valid votes.
     Each canton has one cantonal vote, except Obwalden, Nidwalden, Basel-Stadt,
     Basel-Landschaft, Appenzell Ausserrhoden and Appenzell Innerrhoden with half a vote each (23
     in total); a tie of cantonal votes is a rejection. Popular initiatives and mandatory
     referendums need both majorities; optional referendums need only the majority of voters. A
     canton whose municipalities are all filtered out has no cantonal vote.
  3. Recalculates immediately. The map and both verdicts update while a limit is being dragged,
     without a button. The budget in milliseconds is not decided.
  4. Public data, reproducible, private. Only public sources, each named. Every data file the app
     loads can be regenerated from them by the pipeline. No accounts and no tracking of users.
- Stack: Decided: a static web app in TypeScript. All filtering and recalculation runs in the
  browser; there is no server, no database and no login, so it can be served from static hosting.
  A separate offline data pipeline turns the public sources into the data files the app loads.
  Reason: the data is a few megabytes per year, so the browser can do the work and the result is
  instant. Rejected: a web app with a backend (hosting cost and latency for nothing at this data
  size) and a Python dashboard (every slider move is a server round trip). Open: the UI framework
  (or none), the build tool, how the map is drawn, the language of the pipeline, the formatter,
  linter and test tools, the hosting target, and how the three interface languages are handled.
  There is no JVM build, so the Maven Central note below does not apply.
- State of this repo: empty
- Predecessor or legacy code to learn from: `lukaspestalozzi/votation-visualisation`, branch
  `master`. A course project from 2017 (D3 v3, plain JavaScript, Jupyter notebooks for the data).
  It is read-only and to learn from, not to port: do not copy its code or its data files. The
  front end is in `visualisation/` (`myscript.js` holds the map, the sliders and the result
  calculation), the prepared data in `data/`, the cleaning notebooks in `notebooks/`, and its
  `README.md` describes the municipality merger problem. Its `SPEC.md` is the source of the
  appendix; until pull request #1 there is merged, it is on the branch `ccr-6bce99f8-ca05wz`.
- I use Claude Code on the web for it: yes

Anything blank or unclear above: ask me before you write the files that depend on it. Do not
decide the product, the constraints or the stack for me. When you ask, give the options with your
evaluation of each and your recommendation.

### Failure modes of the predecessor

Each of these must be mapped in `docs/roadmap.md` to the change that specifies against it. The
first five were found by reading and running the predecessor; the last three are from its README
and its repository.

1. The cantons in favour were miscalculated: per canton, only the last municipality processed
   counted instead of the sum (`computeMajorityCanton` in `visualisation/myscript.js`).
2. Schaffhausen was treated as a half canton and Appenzell Ausserrhoden as a full one.
3. Merged municipalities appeared as several rows with identical figures and were counted more
   than once. For the vote of 2016-02-28 on the taxation of couples, the municipal yes votes sum
   to 1,622,968 against 1,609,152 in the official result.
4. Every vote was judged by the double majority, including optional referendums.
5. The slider for the elderly took its range from the data on under-18-year-olds and filtered on
   the data on over-65-year-olds.
6. The sources were inconsistent, and nothing caught it early: mergers applied at different dates
   in different data sets, counts delivered as integers, floats or text, and one missing value in
   the vote results that surfaced far down the pipeline.
7. There were no automated tests and no CI. The only integrity check was a notebook run by hand.
8. The libraries were loaded from third-party servers at run time, one of them over plain HTTP.

### Roadmap input

Decided scope. The split into changes, their order and what counts as v1 are not decided: draft
the rows with me.

- The idea of the predecessor, done correctly: the list of votes, the coloured map, the details of
  a municipality on pointing at it, limits on characteristics, the two verdicts, reset, zoom. Its
  characteristics were: inhabitants, age groups, share of foreigners, population density; the
  strength of FDP, CVP, SP, SVP, GLP, GPS and the other parties; the shares of German, French,
  Italian and Romansh speakers; area; cows per inhabitant.
- Period: all national votes from 2013 to today, with each new vote day added.
- Languages: German, French and Italian, for the interface and for the vote titles.
- Usable on a phone: the map, the limits and the verdicts on a small touch screen.
- Share a selection: a link reproduces a chosen vote together with all limits.
- Compare two votes: the same selection of municipalities applied to two votes side by side.
- More characteristics per municipality than the predecessor had. Which ones is open.

Not asked for so far: a view by canton or district, an English version, data export.

### Still open

Ask me about these when the file that depends on them is due:

- The name of the product and of its packages.
- What counts as v1, and the order of the changes.
- Which further characteristics (candidates named so far: income, education, religion, urban or
  rural type).
- Every open part of the stack listed above.
- The performance budget behind hard constraint 3.
- The module boundaries. At least the pipeline and the app are separate. Whether the result
  calculation becomes a module of its own without any dependency on the user interface is to be
  decided; failure modes 1, 2 and 4 speak for it.
- Which branches CI runs on.
- The language of code, specs and docs. This prompt and the predecessor are in English; confirm.
- Not verified, and a candidate for the first investigation: whether the public sources offer
  the results per municipality for every national vote since 2013 and the characteristics for
  matching years, and how votes that belong to no municipality on the map are counted (some
  cantons may count the Swiss abroad separately).

## How the workflow runs

Every piece of work is one OpenSpec change:

1. `/opsx:explore` (optional): think an idea through, measure, run spikes.
2. `/opsx:propose <change-name>`: creates `openspec/changes/<change-name>/` with the proposal, the
   spec deltas, the design and the tasks. Decisions that shape the change are asked while
   proposing and recorded in the design with the alternatives considered.
3. I review the whole change.
4. `/opsx:apply`: the tasks are implemented in order. Each task ends with its own verification
   step.
5. `/opsx:verify`: the implementation is checked against the change.
6. `/opsx:archive`: the spec deltas are merged into `openspec/specs/`, and the roadmap is updated.

So `openspec/specs/` is always the current truth of what the product does, the archive holds why
it is so, and CI validates both.

## What to set up

**OpenSpec.** The CLI is the npm package `@fission-ai/openspec`. Sunshine pins 1.13.2; use the
latest stable version at setup time and pin that exact version everywhere. Set
`OPENSPEC_TELEMETRY=0`. Before initializing, set the global configuration:
`openspec config set profile custom` and
`openspec config set workflows '["propose","explore","apply","update","sync","archive","verify"]'`.
Check with `openspec config list` that delivery is `both` (skills and commands). Then run
`openspec init --tools claude`. It generates `.claude/commands/opsx/*.md` and
`.claude/skills/openspec-*/SKILL.md`; commit them as generated.

**`openspec/config.yaml`.** This is the most important file: OpenSpec puts its `context` into the
instructions of every artifact, each entry of `rules` into the artifact of that name, and
`operations.<name>.guidance` into apply and archive. Write the context from what I told you, in
these sections: Product (what, for whom, core value), Hard constraints, Tech stack (decided, with
the reason and the rejected alternatives where they matter), Modules (boundaries and the allowed
dependency direction), Quality bar, Glossary. It records decisions, not wishes. Start from this:

```yaml
schema: spec-driven

context: |
  Product: ...
  Hard constraints
  - ...
  Tech stack (decided)
  - ...
  Modules
  - ...
  Quality bar
  - ...
  Glossary
  - ...

rules:
  proposal:
    - Include a "Non-goals" section.
    - Name the docs/roadmap.md entry this change implements, or state that it is off-roadmap and why.
  specs:
    - Numeric behavior is specified with concrete values and tolerances.
    - Specify the behavior when required data is missing or unavailable; never rely on an implicit default.
  design:
    - Ask the user about every decision that changes the specs, the approach or the task breakdown while drafting; record the answer under Decisions with the alternatives considered. "Open Questions" holds only deferrable unknowns.
    - State a performance budget for any computation triggered by user interaction.
  tasks:
    - Every task ends with its verification step, the exact command or manual check plus the expected result.
    - Tests are written and run together with the code they cover, one at a time.

operations:
  apply:
    guidance:
      - Run ./scripts/verify-local.sh before pushing; report failures with the raw error, do not silently retry.
  archive:
    guidance:
      - Run the verify workflow before archiving, and update the change's status in docs/roadmap.md.
```

Keep the rules that fit this project, drop those that do not (the numeric and performance rules
only make sense where the project has such behavior), and propose project-specific ones to me.
After writing it, run `openspec instructions proposal --change <any change>` once a change exists,
or read the CLI's help, to confirm the context and rules reach the instructions.

**`docs/roadmap.md`.** An ordered table of the planned changes, one row per change, with the
columns `#`, `Change` (the kebab-case change name), `Capabilities` (the spec names it touches),
`Scope`, `Open decisions` and `Status` (planned, in progress, archived with the date). Each change
is small enough to review in one sitting. In an empty repo the first change builds the skeleton:
the build, the module boundaries, the CI build job and one visible result. Below the table: what
counts as v1, later candidates, and, if there is a predecessor, its failure modes, each mapped to
the change that must specify against it. Draft the rows with me; do not invent features.

**`investigations/`.** The place for spikes and verified reference values. Each file names its
sources, the script and output that produced the values, and the expected values with tolerances,
so that specs and tests can cite them as oracles. Do not create placeholder files; describe the
directory in `CLAUDE.md`, and the first spike creates it.

**`scripts/verify-local.sh`.** Runs the same checks as CI in the same order: OpenSpec validation
(`openspec validate --all --strict --no-interactive`), then formatter or linter, static analysis,
tests and build in this stack's commands. Every step runs even after a failure, so one run shows
all failures; the script ends with the number of failed steps and a non-zero exit code. `--quick`
runs only the fastest check. The script reads the pinned OpenSpec version from the CI file and
refuses to run when the installed CLI differs, so that both validate by the same rules. In an
empty repo it has only the OpenSpec step, and the first change adds the rest.

**CI (`.github/workflows/ci.yml`).** A `specs` job that runs
`npx --yes @fission-ai/openspec@<pinned version> validate --all --strict --no-interactive`, and a
`build` job with the same steps as the script. The `build` job comes with the first change if
there is nothing to build yet.

**`CLAUDE.md`.** Short, and written for an agent that starts with no context: two lines on what
the project is, and its status; a "Where things are" table (path, what); a code map (file or
package, what) that each change's tasks keep current; the workflow under the heading "no code
without an approved change" with the six steps above and the useful CLI commands
(`openspec list`, `openspec show <name>`, `openspec validate --all --strict`); the build and
verify commands; the conventions. Every path it names exists.

**`README.md`.** The human-facing overview. If one exists, add only a short section on the
spec-first workflow with a table of `openspec/specs/`, `openspec/changes/`, `openspec/config.yaml`,
`docs/roadmap.md` and `investigations/`.

**Cloud sessions (only if I use Claude Code on the web here).** `.claude/settings.json` with a
`SessionStart` hook that runs `$CLAUDE_PROJECT_DIR/.claude/hooks/session-start.sh`. The script
exits at once unless `CLAUDE_CODE_REMOTE` is `true`, is idempotent, installs the pinned OpenSpec
CLI and sets the profile and workflows as above, installs the toolchain this stack needs, and
exports environment variables through `$CLAUDE_ENV_FILE`. Its OpenSpec version is the same as
CI's. One known problem of cloud sessions: Maven Central answers HTTP 429 there. For a Gradle
build, Sunshine's hook writes a Gradle init script that replaces the Maven Central repositories by
Google's mirror, `https://maven-central.storage-download.googleapis.com/maven2/`, and leaves the
project's build files untouched.

**Conventions**, stated in `CLAUDE.md` and in the context's quality bar: conventional commits
(`type(scope): description`; feat, fix, refactor, test, docs, chore, style, ci); files staged
individually, never `git add .`; the linter at zero issues and no suppressed warnings without a
written justification; numeric behavior tested against reference values with explicit tolerances.

## Decisions that are mine

Ask me about these, unless the block above answers them:

- The product statement, the hard constraints and every open stack choice.
- The roadmap: which changes, in which order.
- With existing code: whether to write specs for the current behavior now, or to let the specs
  grow with the changes that touch it.
- The formatter, linter and test tools, if the stack leaves a choice.
- Which branches CI runs on.

## Boundaries

- No product code, no build skeleton and no first change: those come through `/opsx:propose`.
- Do not copy Sunshine's content. Take the form, and write this project's facts.
- Existing files (`README.md`, CI, `.gitignore`, `CLAUDE.md`) are extended, not replaced. If
  something existing conflicts with this setup, tell me before changing it.
- If a command here does not behave as described, for example because the OpenSpec CLI has
  changed, stop and tell me what you saw.

## Done

- `openspec validate --all --strict --no-interactive` passes.
- `./scripts/verify-local.sh` passes, and CI is green on the pull request.
- `.claude/commands/opsx/` and `.claude/skills/` hold the seven workflows.
- `openspec instructions` shows this project's context and rules.
- Every path named in `CLAUDE.md` and `README.md` exists.
- Work on one branch, one commit per piece, and open a draft pull request.

End with a report: what you created, what you verified and how, what you could not verify, and
the questions that are still open. Then stop. The next step is mine: `/opsx:propose` for the first
roadmap change.

## Appendix: the non-technical product spec

Taken from `SPEC.md` of the predecessor. It is the source for the Product and Glossary sections of
`openspec/config.yaml` and for the roadmap. It is input for this setup, not a file to commit: once
the workflow is set up, `openspec/specs/` is the truth.

### In one sentence

Show how Switzerland voted, municipality by municipality, and let anyone ask
"what would the result have been if only municipalities of a certain kind had
voted?"

### The question behind it

Swiss citizens vote several times a year on national proposals. The national
result is one number, but behind it are more than two thousand municipalities
that often vote very differently from each other: cities and villages,
language regions, young and old populations, places with many or few
foreigners.

The project started from one concrete question: *what influence do
municipalities with a high share of foreigners have on the outcome of a vote?*
It quickly became a general one: *which kinds of municipalities carry a
result, and would the outcome flip without them?*

### The core idea

A thought experiment the user controls.

1. Pick a national vote.
2. See the result on a map of Switzerland, one colour per municipality
   (shades of green for yes, shades of red for no).
3. Describe a group of municipalities by their characteristics, for example
   "at most 27 % foreigners" or "at least 0.8 cows per inhabitant".
4. Every municipality outside the group is set aside, as if it had never
   existed. The national result is recalculated immediately from the
   municipalities that remain.
5. See whether the vote would still have passed.

Several characteristics can be combined. A municipality stays in only if it
meets all of them at once.

### What a person can do with it

- Browse the national votes, grouped by year, with date and official result.
- Choose one and see the map coloured by how strongly each municipality said
  yes or no.
- Point at a municipality to read its name, its key figures and its result
  for this vote.
- Narrow the set of municipalities by setting a lower and an upper limit on
  any characteristic.
- Watch the map and the result react while the limits are moved.
  Set-aside municipalities turn grey.
- Always see two verdicts side by side: the majority of voters and the
  majority of cantons, each clearly marked as passed or failed.
- Return to the real result in one step.
- Zoom and move around the map.

### The voting rules the tool must respect

The recalculated result has to follow the real rules, otherwise the thought
experiment is worthless.

- **Majority of voters.** More than half of the valid votes are yes. Votes are
  counted, not municipalities: a city weighs as much as its voters.
- **Majority of cantons.** Each canton counts as one vote and goes to whichever
  side won among its voters. Six cantons count as half a vote each: Obwalden,
  Nidwalden, Basel-Stadt, Basel-Landschaft, Appenzell Ausserrhoden and
  Appenzell Innerrhoden. That makes 23 cantonal votes in total. A proposal
  needs more than half of them; a tie counts as rejected.
- **Which majority is needed depends on the kind of vote.** Changes to the
  constitution (popular initiatives, mandatory referendums) need both
  majorities. Votes on laws (optional referendums) need only the majority of
  voters.
- **When a canton disappears.** If the user's selection removes every
  municipality of a canton, that canton no longer has a vote, and the number
  of cantonal votes needed shrinks accordingly.

### The information it draws on

- **Vote results** per municipality for every national vote covered: yes
  votes, no votes, valid ballots, registered voters, turnout.
- **A map** of municipalities, cantons and lakes that matches the
  municipalities as they existed at the time of each vote.
- **Characteristics of each municipality.** Today these are:
  - population: number of inhabitants, age groups, share of foreigners,
    population density
  - politics: strength of each major party
  - language: share of German, French, Italian and Romansh speakers
  - other: area, cows per inhabitant

The existing version covers 38 votes from March 2013 to June 2016. All of it
comes from public sources, mainly the Federal Statistical Office.

### Principles

- **General purpose.** Any figure that is available for every municipality can
  become a characteristic to filter on. Adding one must not require
  redesigning the tool.
- **Immediate.** The result changes while the user moves a limit, not after
  pressing a button. The feeling of directly steering the outcome is the heart
  of the tool.
- **Faithful to reality.** With nothing filtered, the tool shows exactly the
  official result, for voters and for cantons.
- **Says what it does.** Every control is labelled so that a person without
  statistical training understands which municipalities they are keeping.
- **Understandable without instructions.** Someone who knows what a Swiss
  popular vote is can use it without reading a manual.

### What the first version got wrong

The idea worked and the tool was convincing to play with. A better version
must fix these points:

- The count of cantons in favour was miscalculated, so the cantonal verdict
  could not be trusted.
- Schaffhausen was treated as a half canton and Appenzell Ausserrhoden as a
  full one.
- Municipalities that merged were counted more than once, so the unfiltered
  total was slightly off from the official result.
- Every vote was judged by the double majority, including votes on laws that
  only need the majority of voters.
- The control for the elderly population did not filter on what its label
  said.

### The hard part: municipalities change

Municipalities merge every year (2324 in 2015, 2294 in 2016). The official
statistics rewrite the past each time, as if the merged municipality had
always existed, and not every data set is updated at the same moment. Matching
a vote result, a set of characteristics and a map to the same list of
municipalities was the largest effort of the first version and the source of
most of its errors. Any new version has to treat this as a central concern,
not a detail.

### How to read the results

The tool shows what happens when whole municipalities are set aside. It does
not show how individual people voted. "Municipalities with many foreigners
voted yes" does not mean "foreigners voted yes" — foreigners cannot vote at
all. The recalculated outcome is a thought experiment, not a forecast. The
tool should make this clear to the user.
