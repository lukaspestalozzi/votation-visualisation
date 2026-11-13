# Popular Vote Explorer — the main idea

A non-technical description of what this tool is for and what it must do.
It is the basis for a rewrite. It says nothing about how to build it.

## In one sentence

Show how Switzerland voted, municipality by municipality, and let anyone ask
"what would the result have been if only municipalities of a certain kind had
voted?"

## The question behind it

Swiss citizens vote several times a year on national proposals. The national
result is one number, but behind it are more than two thousand municipalities
that often vote very differently from each other: cities and villages,
language regions, young and old populations, places with many or few
foreigners.

The project started from one concrete question: *what influence do
municipalities with a high share of foreigners have on the outcome of a vote?*
It quickly became a general one: *which kinds of municipalities carry a
result, and would the outcome flip without them?*

## The core idea

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

## What a person can do with it

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

## The voting rules the tool must respect

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

## The information it draws on

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

## Principles

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

## What the first version got wrong

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

## The hard part: municipalities change

Municipalities merge every year (2324 in 2015, 2294 in 2016). The official
statistics rewrite the past each time, as if the merged municipality had
always existed, and not every data set is updated at the same moment. Matching
a vote result, a set of characteristics and a map to the same list of
municipalities was the largest effort of the first version and the source of
most of its errors. Any new version has to treat this as a central concern,
not a detail.

## How to read the results

The tool shows what happens when whole municipalities are set aside. It does
not show how individual people voted. "Municipalities with many foreigners
voted yes" does not mean "foreigners voted yes" — foreigners cannot vote at
all. The recalculated outcome is a thought experiment, not a forecast. The
tool should make this clear to the user.

## Open decisions for the rewrite

Not part of the main idea, and not decided:

- Who is the audience: the curious public, journalists, teachers, researchers?
- Which period should it cover, and should new votes be added as they happen?
- Which languages should the interface and the vote titles be in?
- Which further characteristics are wanted (income, education, religion,
  urban or rural type, ...)?
- Should it also show results by canton or district, not only by
  municipality?
- Should a user be able to compare two votes, or save and share a selection?
- Should it work on a phone?
