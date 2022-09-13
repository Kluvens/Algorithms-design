# Introduction: Some Representative Problems

## A First Problem: Stable Matching
The stable matching problem originated in 1962 when David Gale and Lloyd Shapley asked the question:
Could one design a college admissions process, or a job recruiting process, that was self-enforcing?

To understand the question better, we can think of a practical situation.
Students in college majoring in computer science, begin applying to companies for summer internships.
Each applicant has a preference ordering on companies, and each company forms a preference ordering on its applicants.
Based on these preferences, companies extend offers to some of their applicants, applicants choose which of their offers to accept, and people begin heading off to their summer internships.

However, both companies and applicants may change their minds and choose someone else, retracting offers.
Situations like this can rapidly generate a lot of chaos, and many people, both applicants and employers can end up unhappy with the process as well as the outcome.
One basic problem is that the process is not self-enforcing- if people are allowed to act in their self-interest, then it risks breaking down.

Then we might prefer a more stable situation, in which self-interest itself prevents offers from being retracted and redirected.
The situation is where companies prefer the applicants they accepted and applicant is happy to stay in the company even though there's a chance to go to another company.

So this is the question Gale and Shapley asked:
Given a set of preferences among employers and applicants, can we assign applicants to employers so that for every employer E, and every applicant A who is not scheduled to work for E, at least one of the following two things is the case?
1. E prefers every one of its accepted applicants to A
2. A prefers her current situation over working for employer E

If this holds, the outcome is stable, individual self-interest will prevent any applicant/employer deal from being made behind the scenes.

It is useful, at least initially, to eliminate some complications such as each applicant may apply to many companies and a company may be seeking for many employees,
and arrive at a more 'bare-bones' version of the problem:
each of n applicant applies to each of n companies, and each company wants to accept a single applicant.

Following Gale and Shapley, we observe that there's a scenario that makes more sense in the real world in which each of n men and n women can end up getting married:
our problem naturally has the analogue of two genders - the applicants and the companies - and the case we are considering, everyone is seeking to be paired with exactly one individual of the opposite gender.

So consider a set M = {m1, ... ,mn} of n men, and a set W = {w1, ... , wn} of n women.
Let M × W denote the set of all possible ordered pairs of the form (m, w), where where m ∈ M and w ∈ W.
A matching S is a set of ordered pairs, each from M × W, with the property that each member of M and each member of W appears in at most one pair in S.
A perfect matching S' is a matching with the property that each member of M and each member of W appears in exactly one pair in S'.

In the present situation, a perfect matching corresponds simply to a way of pairing off the men with the women, in such a way that everyone ends up married to somebody, and nobody is married to more than one person - there is neither singlehood nor polygamy.

Now we can add the notion of preferences to this setting. 
Each man m ∈ M ranks all the women; we will say that m prefers w to w' if m ranks w higher than w'.
We will refer to the ordered ranking of m as his preference list.
We will not allow ties in the ranking. 
Each woman, analogously, ranks all the men.

However, there's another case we have to think about:
THere are tow pairs (m, w) and (m', w') in S with the property that m prefers w' to w, and w' prefers m to m'.
In this case, there's nothing to stop m and w' from abandoning their current patners and heading off together.
The set of marriages is not self-enforcing.
We'll say that such a pair (m, w') is an instability with respect to S: (m, w') does not belong to S, but each of m and w' prefers the other to their partner in S.

Generally, our goal is a set of marriages with no instabilities.
We'll say that a matching S is stable if 
1. it is perfect
2. there is no instability with respect to S

One important thing to keep in mind:
It's possible for an instance to have more than one stable matching.
