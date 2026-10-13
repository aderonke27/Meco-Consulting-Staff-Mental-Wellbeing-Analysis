# Meco Consulting Wellbeing Programme: Cohort Impact Analysis
# Did the Meco Wellbeing Programme Work? A Pre/Post Analysis, from One Participant to the Whole Cohort

A two-stage analysis of a 6-week mental health and wellbeing programme, using the DASS-21 and WHO-5 questionnaires. I built it during the YHPT Data Analytics Mentorship Program (Project 1). It starts with one participant's results and ends with the whole cohort, because one person's improvement can't tell you whether a programme works.

## Table of Contents

1. [Overview](#1-overview)
2. [The Question](#2-the-question)
3. [The Data and the Missing Records](#3-the-data-and-the-missing-records)
4. [Tools](#4-tools)
5. [Method: Cleaning and Scoring](#5-method-cleaning-and-scoring)
6. [Stage 1: One Participant](#6-stage-1-one-participant)
7. [Why One Participant Isn't Enough](#7-why-one-participant-isnt-enough)
8. [Stage 2: The Full Cohort](#8-stage-2-the-full-cohort)
9. [How Participant A Compares to the Cohort](#9-how-participant-a-compares-to-the-cohort)
10. [Conclusions](#10-conclusions)
11. [Limitations](#11-limitations)
12. [Recommendations](#12-recommendations)
13. [Repository Structure](#13-repository-structure)
14. [Write-ups](#14-write-ups)
15. [AI Use](#15-ai-use)
16. [What I Learned](#16-what-i-learned)
17. [Acknowledgements](#17-acknowledgements)

---

## 1. Overview

Meco Consulting, a mid-sized professional services firm, ran a 6-week wellbeing programme made up of workshops, peer support sessions and guided reflection exercises. Every participant completed the same survey at the start (Week 0) and again at the end (Week 6).

I played the data analyst supporting the company's HR and People team. The project ran in two stages, and this repo treats them as one piece of work:

- **Stage 1 (Week 1):** I analysed a single participant's before and after scores and wrote a report meant to be shared with that person.
- **Stage 2 (Week 2):** with my group, I zoomed out to the full cohort and asked whether the pattern held across everyone, and for whom it didn't.

## 2. The Question

Did the programme have a measurable effect on participants' mental health and wellbeing? And did the effect differ by department, job level or other demographics?

## 3. The Data and the Missing Records

There are two survey files: a pre-programme file with 30 participants and a post-programme file with 28. Each row holds demographics (age, gender, department, job level, employment type, years of experience), 21 DASS-21 items (Q1 to Q21) and 5 WHO-5 items (W1 to W5). Answers arrived as text labels like "Sometimes" and "Most of the time", not numbers.

**How I handled the missing post-surveys.** Two of the 30 participants never completed the post-survey, which gives a completion rate of 93.3%. I excluded them from every before/after comparison and worked only with the 28 people who had both records. A paired comparison needs two scores from the same person. Filling in the missing post scores (with the average, say) would have meant inventing results, and it would have pulled the numbers toward whatever I assumed.

I did not hide those two people. They appear in the dataset overview, they are counted in the 93.3%, and the recommendations ask HR to follow up with them directly. Dropping them is a choice with a cost: if they left because the programme wasn't working for them, the results look better than they should. I cover that in the limitations.

The same rule applied in Stage 1. My participant had both a pre and post record, so a full paired comparison was possible.

**The cohort (28 matched participants):**
- Gender: 16 female, 11 male, 1 prefer not to say
- Job level: 15 Junior, 8 Mid-level, 3 Senior, 2 Manager
- Employment: 14 Full-time, 8 Contract, 6 Part-time
- Departments: Technology 6, Finance 5, Strategy 5, HR 4, Operations 4, Client Services 4

> **Privacy note:** the raw survey files contain names and email addresses. `[Edit this line to say whether you uploaded an anonymised copy or left the raw data out.]`

## 4. Tools

- Microsoft Excel for cleaning, scoring, classification and aggregation, with formulas visible throughout
- Excel Tables and PivotTables for the demographic and band counts
- Excel charts for the visuals
- PowerPoint for both reports

## 5. Method: Cleaning and Scoring

**Turning text into numbers.** I converted every answer with a lookup table kept inside the workbook, so anyone checking the work can trace how "Sometimes" became a 1 instead of trusting that I typed the right value. DASS-21 answers map as Never = 0, Sometimes = 1, Often = 2, Almost Always = 3. WHO-5 answers map from Never = 0 up to All of the time = 5.

**DASS-21 scoring.** The 21 questions are not split into neat blocks of 1 to 7, 8 to 14 and 15 to 21. The subscale items are scattered, so you have to know which question belongs where:
- Depression: Q3, Q5, Q10, Q13, Q16, Q17, Q21
- Anxiety: Q2, Q4, Q7, Q9, Q15, Q19, Q20
- Stress: Q1, Q6, Q8, Q11, Q12, Q14, Q18

I summed each set of seven, then multiplied by 2, because the official severity bands are built for the 42-item version. Lower scores are better.

**DASS-21 severity bands.** Depression: Normal 0 to 9, Mild 10 to 13, Moderate 14 to 20, Severe 21 to 27, Extremely Severe 28+. Anxiety: Normal 0 to 7, Mild 8 to 9, Moderate 10 to 14, Severe 15 to 19, Extremely Severe 20+. Stress: Normal 0 to 14, Mild 15 to 18, Moderate 19 to 25, Severe 26 to 33, Extremely Severe 34+.

**WHO-5 scoring.** I summed the five items (0 to 25) and multiplied by 4 to get a 0 to 100 score. Higher is better. A score below 50 is flagged as a possible sign of poor wellbeing. The workbook also splits scores into Low, Moderate and High bands for the cohort counts.

**What the workbook does.** The coding sheets turn text into numbers and calculate each score. One sheet per measure (Depression, Anxiety, Stress, WHO) pulls the pre and post scores, classifies each person with `IFS`, calculates the shift (post minus pre) and builds a transition label like "Mild to Normal" with `CONCAT`. `COUNTIF` counts people per band before and after, and PivotTables handle the demographic cuts. Every calculation is a live formula. Nothing is pasted in as a value.

## 6. Stage 1: One Participant

Participant A is a 26-year-old woman in Technology. She's a Junior, full-time, with 3 years of experience. These are her scores:

- Depression went from 16 to 4, Moderate to Normal
- Anxiety went from 28 to 14, Extremely Severe to Moderate
- Stress went from 16 to 12, Mild to Normal
- WHO-5 wellbeing went from 72 to 84 out of 100 (raw 18 to 21 out of 25), a gain of 12 points

On paper that's improvement across the board. Every measure moved the right way, and anxiety had the biggest drop at 14 points. Even so, her anxiety ended in the Moderate range, not Normal, so I recommended continued anxiety-focused support alongside the wellbeing activities she was already using. I also wrote that the findings describe change for one person and don't show the programme alone caused it.

## 7. Why One Participant Isn't Enough

This is the part that's easy to skip when the numbers already look good. One person's improvement says nothing about whether the programme works. She might be unusually responsive, or she might have improved for reasons unrelated to the programme. To answer "did it work", you have to look at everyone who went through it and check whether the pattern holds across the group. That's what Stage 2 does.

## 8. Stage 2: The Full Cohort

I ran the same scoring on all 28 matched participants and compared averages and severity bands before and after.

**Average scores:**
- Depression fell from 12.4 (Mild) to 6.1 (Normal), a 51% drop
- Anxiety fell from 20.7 (Extremely Severe) to 10.6 (Moderate), a 49% drop
- Stress fell from 17.5 (Mild) to 10.4 (Normal), a 41% drop
- WHO-5 rose from 62 to 75, a 13-point gain, which moves the cohort average from Moderate to High wellbeing

**Distributions.** The share of people in the High wellbeing band went from 21% to 75%. For anxiety, 68% of participants started in the Extremely Severe band and 7% ended there. For depression, 79% finished in the Normal range.

**What stood out:**
- Anxiety had the largest absolute drop, about 10 points on average. It was also the biggest problem at the start.
- Depression had the largest relative drop.
- Stress moved the least, though it started as the mildest problem, so there was less room to fall.
- Junior staff, who make up 54% of the cohort, improved the most on every measure. Senior and Manager staff barely moved, but those groups are tiny (3 and 2 people), so I treat that as something to watch, not a conclusion.
- Technology showed the strongest all-round improvement. Finance had solid anxiety and stress gains but the smallest depression improvement.
- 2 of the 28 matched participants did not improve.

That last point mattered to me. Data analysis isn't only asking whether the average got better. It's asking who improved, who didn't, by how much, and what else to investigate.

## 9. How Participant A Compares to the Cohort

Putting the two stages side by side shows why the individual view alone would have misled. Participant A's depression fell by 12 points against a cohort average of 6.3. Her anxiety fell by 14 against 10.1. Her stress fell by only 4 against 7.1, so she improved less than the typical participant on that one. Her WHO-5 gain of 12 points sits close to the cohort's 13.

So she was a good example of the programme working, but not a typical one on every measure. Her story would have overstated the effect on depression and understated it on stress.

## 10. Conclusions

Every measure moved in the right direction on average, so the programme is clearly associated with better mental health and wellbeing. The gain was largest where the need was greatest, in anxiety. The improvement wasn't uniform, though. Junior staff gained the most, senior staff showed little change, and a small group didn't improve at all.

The data shows improvement after the programme. That isn't the same as proving the programme caused it.

## 11. Limitations

- **Small sample.** There are 28 matched people, and the subgroups are much smaller, so job-level and department patterns are indicative only.
- **No control group.** I can't separate the programme's effect from natural recovery, seasonal mood or unrelated events during the same six weeks.
- **Self-report bias.** Participants know the programme is being evaluated and may report feeling better than they do.
- **Non-response.** The 2 people who skipped the post-survey might differ from everyone else. If they dropped out because the programme wasn't helping, the results are too optimistic.
- **One follow-up point.** I only have Week 6, so I can't say whether the gains last.
- **No significance testing.** I report averages and band counts, not statistical tests.

## 12. Recommendations

1. Continue and expand the programme, since the overall signal is positive.
2. Redesign the format for senior staff, with sessions that address leadership-specific pressures.
3. Follow up individually with the 2 participants who didn't complete the post-survey and the 2 who didn't improve.
4. Re-assess at 3 and 6 months to see whether the gains hold. Anxiety is still Moderate on average after the programme, so it needs the closest watch.
5. Study what worked in Technology and test whether it can lift results in departments like Finance.

## 13. Repository Structure

```
.
├── README.md
├── data/
│   └── Mental_health_pre_and_post_survey.xlsx    # working file, all formulas visible
├── reports/
│   ├── Week1_Participant_A_Report.pdf             # individual report (7 slides)
│   └── Week2_Meco_Cohort_Report.pdf               # cohort report (9 slides)
├── brief/
│   ├── YHPT_Project1_Week1_Brief.pdf
│   └── YHPT_Project1_Week2_Brief.pdf
└── images/
```

The workbook has the raw `pre_survey` and `post_survey` sheets, two coding sheets that convert and score the responses, one sheet each for Depression, Anxiety, Stress and WHO, and a `Demographics_analysis` sheet with the PivotTables and charts.

## 14. Write-ups

I wrote about each stage on LinkedIn:

- Stage 1 post: `[add link]`
- Stage 2 post: `[add link]`

## 15. AI Use

I used AI tools for parts of this project, as the programme policy allows. I can explain and rebuild every formula and chart in the workbook myself, and the working file holds real formulas, not values pasted from AI output.

## 16. What I Learned

The biggest lesson was that a good-looking result for one person is the start of the analysis, not the end of it. The second was that how you treat missing data changes what the results mean, so it has to be written down. `[Add anything else in your own words: an Excel function you picked up, something that surprised you, what you'd do next.]`

## 17. Acknowledgements

Thanks to my mentor, Mercy Olaoluwa, for the guidance, and to Young Health Professionals in Tech (YHPT) for building a programme that hands you a real-world scenario instead of a toy dataset. The instruments used are the DASS-21 and the WHO-5 Wellbeing Index, both in the public domain.
