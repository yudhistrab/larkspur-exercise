# Client review: Larkspur disruption-care agent

**To:** larkspur-exercise  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:57

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md claims 12/15 to 15/15 on five adversarial messages, but evals/cases.json holds four cases and only one is an abuse case.**

evals/cases.json has scope-0201, cap-0201, irrv-0201 and tone-0201. Only tone-0201 is abuse with no question asked, and PITCH.md's Still broken says the tone rule "has only been run on two abusive messages". The five messages behind 12/15 and 15/15 are not in the repository.

Paste the bench.py --compare output for your before and after labels, with the five message texts, so the 15/15 has a denominator Priya can open.

**2. PITCH.md's Safety check says all four cases pass, but readout-trace.json shows one run on one PNR and no eval output is committed.**

The committed trace has 4 API turns, 4 tool calls (lookup_booking, get_flight_status, check_policy, fare_rules) and never reaches search_alternatives, hold_seat or confirm_rebooking. That run does not show confirm_rebooking being withheld on irrv-0201.

Run python3 eval_harness.py and paste the full pass/fail output for scope-0201, cap-0201, irrv-0201 and tone-0201.

**3. cap-0201 in evals/cases.json expects LK375 with three seats left to be excluded, but its graders only check tool names.**

The rules grader has must_call search_alternatives and must_not_call confirm_rebooking. Nothing in it checks the seat count a party of four needs, and your Left out section says party size is not yet passed to next_available_day. Whether the case passes depends on the judge grader alone.

Run python3 run.py S4BF8M --trace and paste the search_alternatives result and the final reply text.

**4. Your lever is intelligence, but build_tools() in agent.py leaves hold_seat at 73 characters and send_confirmation at 71.**

The diff rewrote search_alternatives to 421 characters and added ISO-date guidance to get_flight_status. hold_seat and send_confirmation still carry one-line descriptions with bare option_id, pnr and message properties. Nothing in the diff tells the model when a hold may be placed or that send_confirmation follows a confirmed rebooking.

Run python3 run.py --show-tools and paste the hold_seat and send_confirmation entries.

**5. The cost line cites $0.0653 against $0.0753, but agent.py has no cache_control and the last wire run shows 0 cache reads and 0 writes.**

readout-trace.json records 18125 tokens in and 1127 out across 4 turns with a null hit ratio. run_agent() resends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM and all tools on every turn, and the 646-character TONE_ADDENDUM is part of that. A 17k-token input is the cost a model change alone would not touch, so Priya's question has a number nobody has reduced.

Run python3 run.py --tool-tax and paste the per-turn input token breakdown.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | $0.0653 model cost per contact on Stage 2 (was $0.0753), 3 runs x 5 messages, model only and not the loaded cost; about $895 a week at bench.py's 13,700 chats a week. | **Not supported by the repository.** "$0.0653 ... (was $0.0753)" and "$895 a week" have no bench file or compare output in the material. Paste the python3 bench.py --compare output, and say which stage and message set 13,700 chats a week multiplies. |
| **When it is wrong** | When party size matters and next_available_day is asked, because it ignores it, and when abuse is borderline, because the tone rule is a prompt paragraph the model could misread either way. Neither is measured beyond the cases above. | **Answered.** You name next_available_day ignoring party size and the borderline-abuse boundary, and say "Neither is measured beyond the cases above". evals/cases.json agrees: cap-0201 is the only party case and tone-0201 the only abuse case. |
| **Who runs it** | Not decided in this build. Escalations go to a human queue through escalate_to_human, so someone has to own that queue and the policy data before this talks to real customers. | **Thin.** "Not decided in this build" is honest, but it names no owner for the escalate_to_human queue or the policy data. TEAM.md or a named person would settle it. |
| **What you left out** | Live airline systems (the backend is a mock covering 7 to 9 May 2025), prompt caching, a cost or speed lever, party size in next_available_day, and a case proving a merely upset customer is not escalated. | **Answered.** Mock backend dates, prompt caching, party size in next_available_day and the missing upset-customer case all match the scan: no cache_control, and four cases with none for a blunt non-abusive customer. |

All four answered. Bring the artifact behind each one to our next meeting, not the sentence.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (248 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- `evals/cases.json`
- `PITCH.md (4 of 4 answers to Priya)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `pod_sync.py`, `run.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
