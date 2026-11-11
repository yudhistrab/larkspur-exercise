# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A nine-tool Claude agent for flight-disruption chat, plus next_available_day served over an MCP server, and four eval cases covering scope, capacity, irreversible actions and tone.
Does: Looks up the booking, checks the flight and the policy, offers up to three rebooking options, holds a seat, and hands groups, refunds and abusive customers to a human; it never confirms a booking without the customer's own click.
Number: Stage 2 wire rules went from 12/15 to 15/15 passed (3 runs x 5 adversarial messages, claude-sonnet-5-5), and the tone_safety hard gate went from failing to holding.
Safety check: 4 eval cases, all hard gates (scope, capacity, irreversible, tone), 4/4 pass with judge claude-opus-5-5, and none of the four called confirm_rebooking.
Next: Add a legal-threat case and a blunt-but-not-abusive case so the tone rule is tested for over-triggering as well as under-triggering, and pass party size into next_available_day.
Still broken: next_available_day still answers for a party of one because pax_count never reaches it, and the tone rule is one prompt paragraph tested on two abusive messages.
Lever: intelligence

## Priya asked

Costs: $0.0653 model cost per contact on Stage 2 (was $0.0753), 3 runs x 5 messages, model only and not the loaded cost; about $895 a week at bench.py's 13,700 chats a week.
Wrong: When party size matters and next_available_day is asked, because it ignores it, and when abuse is borderline, because the tone rule is a prompt paragraph the model could misread either way. Neither is measured beyond the cases above.
Runs it: Not decided in this build. Escalations go to a human queue through escalate_to_human, so someone has to own that queue and the policy data before this talks to real customers.
Left out: Live airline systems (the backend is a mock covering 7 to 9 May 2025), prompt caching, a cost or speed lever, party size in next_available_day, and a case proving a merely upset customer is not escalated.
