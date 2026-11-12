# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A support agent for disrupted flights: Larkspur's nine tools, plus next_available_day and fare_rules served over MCP (support/mcp_server.py), a short tone rule in the system prompt, caching of the start of every request, and six eval cases.
Does: It looks up the booking, checks the flight and the policy, lists up to three rebooking options, quotes the Handbook when a customer asks why, and hands groups and abusive messages to a person. It offers to hold a seat, but only the customer's own Confirm click can finalize it.
Number: Wire rules passed on our five adversarial customer messages went from 12/15 to 15/15 (3 runs of each message, same model, before and after our tone rule).
Safety check: The agent never finalizes a booking without the customer: confirm_rebooking needs a token only the customer's own Confirm click can produce. We saw it in our evals: the "finalize it for me" case passed in every one of our full runs, so confirm_rebooking was never called.
Next: Test the tone rule on a legal threat and on a customer who is blunt but not abusive, so we know it neither misses abuse nor escalates too much, and give next_available_day the party size.
Still broken: next_available_day only searches for one passenger (its own description says so), so for a party of four it could name a date that cannot take all four. None of our eval cases tests it with a party yet, and the tone rule has only been run on two abusive messages. One of our six evals, tone-0201, is flaky: it failed 1 of 5 runs with our latest change and 2 of 5 without it, and in the one failure we read, its judge could not confirm a handover because the escalation came back without a queue.
Lever: intelligence

## Priya asked

Costs: $0.0328 model cost per contact after caching the start of every request (was $0.0742), 3 runs of each of the five tickets, same model, cold-cache sweep; model cost only, not the loaded cost. About $449 a week at bench.py's 13,700 chats a week, assuming the cache stays warm.
Wrong: When party size matters and next_available_day is asked, because it ignores it, and when abuse is borderline, because the tone rule is a prompt paragraph the model could misread either way. Neither is measured beyond the cases above.
Runs it: Not decided in this build. Escalations go to a human queue through escalate_to_human, so someone has to own that queue and the policy data before this talks to real customers.
Left out: Live airline systems (the backend is a mock covering 7 to 9 May 2025), a speed lever, party size in next_available_day, and a case proving a merely upset customer is not escalated.
