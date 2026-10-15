# Team Contract

## Team Members

* Nate Poppe (poppent@mail.uc.edu)
* Braden Hayes (hayesbp@mail.uc.edu)

## Advisor

* [Advisor Placeholder] - Faculty Advisor (not yet assigned)

## Meeting Schedule

* Team meeting: once a week, Thursdays @ 3pm
* Advisor meeting: every other week, once assigned, day/time TBD

## Communication

* Discord - day-to-day communication between team members
* Email - communication with advisor
* Expected response time: 2 weekdays

## Roles

**ML Lead**

* Braden Hayes
* Owns model design, training pipeline, and the live risk-scoring system

**QA Lead**

* Braden Hayes
* Owns test planning, including verifying risk scores and explanations against real games

**UI / Database Lead**

* Nate Poppe
* Owns the website itself, the API, and the database

**Coordinator / Documentation Owner**

* Nate Poppe
* Tracks deadlines, keeps meeting notes, and maintains the project writeup

## Project Objectives

* Build an ML system that predicts blunder risk during a chess game using position complexity, time pressure, rating, game phase, and engine-derived features, and explains that risk in plain language rather than a raw evaluation number.
* Validate the model in stages: first confirm the risk score behaves sensibly on individual test positions, then test it across full games, then test the aggregate trend view across a player's game history. Any stage that reveals inaccurate scoring sends the model back for adjustment before moving to the next stage.
* After each major component is finished (risk model, explanation output, game replay interface, trend view), run a dedicated round of accuracy and bug testing before starting the next component.

## How We'll Make Decisions

Either team member can raise a decision that needs to be made. When that happens, we'll talk through the options and trade-offs together before committing. If we can't agree, we'll bring it to our advisor once one is assigned; until then, we'll default to whichever approach lowers project risk or keeps us closer to our current timeline.

## If We Run Into Conflict

We'll start by talking it out directly and honestly, making sure we're both working from the same facts before deciding anything. If that doesn't resolve it, we'll raise it with our advisor at the next opportunity. If the advisor can't help resolve it, we'll bring it to the instructor.

## Signatures

Nate Poppe
Braden Hayes
