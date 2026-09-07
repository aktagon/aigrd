---
id: RUBRIC-001
title: Investment thesis
genre: THESIS
---

# RUBRIC-001: Investment thesis

A short memo that pitches one position to the person at a fund who decides
what the fund owns. That reader reads it in two minutes and sizes it, asks
for more, or passes. The criteria follow Michael Steinhardt's brief to his
intern in "No Bull: My Life In and Out of Markets" (Wiley, 2001): the idea,
the consensus view, the variant perception, and a trigger event, with no
interest where there is no variant perception. The passages are quoted at
https://casnocha.com/2007/06/variant_percept.html (the four things) and
https://acquirersmultiple.com/2021/12/michael-steinhardt-develop-a-variant-perception/
(the definition and the payoff). Each criterion names the passage it comes
from. Three criteria are the owner's, taken from the house thesis validator
rather than from Steinhardt: the answered bear case (C-007), the kill
condition (C-008), and fact kept apart from forecast (C-009). Position size
is not here; the validator sizes after the thesis holds.

Each criterion is one decidable claim and says what is exempt. Word counts
and the two-minute length are ctxgrd's; a check a regex can decide is not
here. With three should criteria, a threshold of 0.75 blocks on any miss,
so the genre sets 0.66: one should miss warns and two block.

## Criteria

- **C-001** (must): The memo states the idea as one position: the security, the direction (long or short), and the outcome the writer expects. A memo that names a company, a sector, or a theme with no direction or no expected outcome fails. (The four things, item one: the idea)
  - hint: the opening paragraph, or a section headed Idea or Thesis
- **C-002** (must): The memo states the consensus view as what other holders and analysts expect, and names where that expectation shows: a price, a multiple, an estimate, a rating, a named report, or a quoted commentator. A consensus given only as "the market is wrong" or "nobody sees this", with no expectation named, fails. (The four things, item two: the consensus view; "a keen understanding of what the market expectations truly were")
  - hint: a section headed Consensus, or the sentences that say what others believe
- **C-003** (must): The memo states the variant perception as a rejection of something the consensus in C-002 relies on: a premise, an event, or a judgement the consensus holds and the writer says is wrong. A view that keeps every consensus premise and moves only the estimate, such as earnings a little above the street's or a solid growth case within consensus, fails. (The four things, item three: the variant perception; "no variant perception ... I generally had no interest")
  - hint: a section headed Variant perception, or the sentence that turns on "but", "however", or "we believe"
- **C-004** (must): The memo gives the ground for the variant perception: a fact, a source, or an analysis that the consensus omits or misreads, and says why the writer has it and the market does not. A variant perception asserted with no ground, or grounded only in the writer's conviction or record, fails. ("a well-founded view that was meaningfully different from market consensus"; "knowing more and perceiving the situation better than others did")
  - hint: the evidence under the variant perception, and any cited filing, dataset, or interview
- **C-005** (must): The memo names a trigger event: a future event that would move the consensus toward the variant perception, and says when the writer expects it. An earnings report, a filing, a ruling, a contract, a refinancing, or a spin-off qualifies. A trigger given only as "the market will realise" or "in time" fails. (The four things, item four: a trigger event)
  - hint: a section headed Trigger or Catalyst, and any date or quarter in the memo
- **C-006** (must): The memo says what the writer earns when the variant perception becomes consensus: the price, multiple, or return expected at that point, set against the current one. A payoff given only as "significant upside" fails. ("the process by which a disparate perception, when correct, became consensus would almost inevitably lead to meaningful profit"; "a return both from perception change as well as valuation adjustment")
  - hint: a target price, a target multiple, or a return figure, and the current price beside it
- **C-007** (should): The memo states the strongest case against the variant perception, in the words a holder of the consensus would use, and answers it with a fact or an argument. A list of risks with no counter-argument answered fails. (The owner's rule, from the house thesis validator, Perspective 2, the Bear; not Steinhardt's)
  - hint: a section headed Bear case, Risks, or What the consensus would say
- **C-008** (should): The memo names the observation that would prove the variant perception wrong and says what the writer does then. Risks listed with no exit for any of them fail. (The owner's rule, from the house thesis validator, Phase 6, the kill switch; not Steinhardt's)
  - hint: a section headed What kills it, Kill switch, or Risks
- **C-009** (should): Each claim about the company, its market, or its numbers is either a fact with its source named or a forecast marked as the writer's expectation. A forecast written as a settled fact fails. The consensus view in C-002 is exempt, since it is attributed to others. (The owner's rule, from the house thesis validator, Phase 1, fact versus assumption versus prediction; not Steinhardt's)
  - hint: each sentence that carries a number or a future tense
