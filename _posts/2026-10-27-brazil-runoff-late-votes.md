---
layout: post
title: "34,050 votes after the polls closed: late voting in Brazil's 2026 runoff"
description: Counting every vote cast after 17:00 from 498,000 voting-machine logs.
---

{: .lede}
**34,050 Brazilians voted after 17:00 Brasília time in the 25 October 2026 runoff, spread across 29,609 polling sections.** That is about 1 in every 3,750 votes cast in Brazil.

Polls in Brazil close at 17:00, but anyone already in line still gets to vote. Nobody publishes how many people that is. So we counted it ourselves, vote by vote, from the electronic voting machines' own logs.

## How we counted

Every Brazilian voting machine (*urna*) writes a timestamped event log, and the TSE, Brazil's electoral court, publishes one per polling section on [resultados.tse.jus.br](https://resultados.tse.jus.br). Each ballot shows up in the log as a `Voto computado` event. We downloaded the log for every section and counted the votes stamped after 17:00.

1. Enumerate every section from the TSE's open data portal.
2. Fetch and decompress each section's log, merging logs when a machine was swapped mid-day.
3. Convert each log's clock to Brasília time and count `Voto computado` events after 17:00:00.
4. Sum by state and nationally.

The logs add up. Together they hold 128,235,039 votes, against the TSE's official count of 128,236,074. The 1,035-vote gap sits entirely in 72 sections for which the TSE published results but no log. Sections with logs plus those 72 come to exactly the official 499,248 machines.

We did not use the RDV, the machine's digital record of votes. It is shuffled and carries no timestamps, by design, to protect ballot secrecy.

## Where people voted late

São Paulo, the biggest state, had the most late votes: 8,787. Relative to turnout, though, late voting was most common in Rondônia, the Federal District, Roraima and Amazonas, and rarest in the South and Espírito Santo.

<figure>
{% include charts/late-votes-by-state.svg %}
<figcaption>Votes after 17:00 per 100,000 votes cast, by state. Blue: above the national rate. Source: TSE urna logs, 497,895 sections.</figcaption>
</figure>

The gap is wide. Rondônia saw about 56 late votes per 100,000 cast; Santa Catarina saw under 10.

## The first five minutes

Most late voting was just the end of the line. 84% of late votes came before 17:05, and two-thirds (22,284) within two minutes of closing.

<figure>
{% include charts/late-votes-by-minute.svg %}
<figcaption>Votes cast after 17:00 Brasília time, in 5-minute bins. Overseas sections excluded. Source: TSE urna logs.</figcaption>
</figure>

Then comes a long, flat tail of roughly 200 votes every five minutes until 18:45. 6,987 sections closed after 18:30, mostly in Rio de Janeiro, São Paulo, Salvador and João Pessoa, and some kept taking votes until about 19:00.

## Time zones, overseas voters and missing logs

**Log timestamps are in each machine's local time, not Brasília time.** Brazil spans four time zones, but voting runs 08:00–17:00 Brasília time everywhere. So a machine in Manaus opens at 07:00 on its own clock and must close at 16:00.

We found each machine's offset from the log itself. Every machine opens for voters at 08:00 Brasília time, so the local hour of that event gives the offset. Taking the most common offset per municipality picked up exactly the expected exceptions: 11 municipalities in western Amazonas on UTC−5, and Fernando de Noronha on UTC−2.

**Overseas sections are reported separately.** Brazilians abroad vote on local time, closing at 17:00 wherever they are. Across 1,281 overseas sections with logs, 416 votes came after 17:00 local time in 252 sections. They are not in the 34,050.

**72 sections have no log.** The TSE published their results but no log file, and a retry found nothing new. Two are in Brazil (one in Rio de Janeiro, one in São Paulo) and 70 are overseas. Around 18,000 other listed sections share a machine with a neighbouring section, so their votes are already counted in that section's log.

## How far to trust it

- **Clock accuracy matters most.** Two-thirds of late votes fall within two minutes of 17:00, so a machine whose clock runs a minute fast or slow moves the count. The only check available is whether a log's closing time comes after the TSE received that section's results. Only 8 Brazilian sections failed, each by under a minute, holding just 2 late votes between them.
- **Some sections ran very late.** 6,987 sections closed after 18:30. Their clocks look normal, so their votes count. We have not yet established why they stayed open: long queues, court-ordered extensions, or both.
- **Paper ballots are not included.** When a machine fails and can't be replaced, voting moves to paper. Those votes never touch a log.
- **Overseas is a judgement call.** We thresholded at 17:00 local time. Converting to Brasília time instead would change the overseas figure, though not the national one.

The TSE will publish bulk log files for the runoff on its [open data portal](https://dadosabertos.tse.jus.br) after totalization. We will re-run the count against them as a cross-check.
