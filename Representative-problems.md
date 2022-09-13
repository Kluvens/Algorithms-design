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

### Designing the Algorithm
By now, we can show that there exists a stable matching for every set of preference lists among the men and women.
Additionally, we will give an efficient algorithm that takes the preference lists and constructs a stable matching.

Here is a concrete description of the Gale-Shapley algorithm:
```
Initially all m ∈ M and w ∈W are free
While there is a man m who is free and hasn’t proposed to every woman
  Choose such a man m
  Let w be the highest-ranked woman in m’s preference list
    to whom m has not yet proposed
  If w is free then
    (m, w) become engaged
  Else w is currently engaged to m'
    If w prefers m' to m then
      m remains free
    Else w prefers m to m'
      (m, w) become engaged
      m' becomes free
    Endif
  Endif
Endwhile
Return the set S of engaged pairs
```

However, even though the Gale-Shapley algorithm is simple to state, it is not immediately obvious that it returns a stable matching or even a perfect matching.

### Analyzing the Algorithm
First consider the view of a woman w during the execution of the algorithm.
Initially, no one proposed to her, and she is free.
As time goes on, a man m may propose to her and then she become engaged.
Later, she may receive more proposals from other men.
So we discover the following:

(1.1) w remains engaged from the point at which she receives her first proposal;
and the sequence of partners to which she is engaged gets better and better (in terms of her preference list).

The view of a man m during the execution of the algorithm is rather different. 
He is free until he proposes to the highest-ranked woman on his list; 
at this point he may or may not become engaged. 
As time goes on, he may alternate between being free and being engaged; 
however, the following property does hold.

(1.2) The sequence of women to whom m proposes gets worse and worse (in terms of his preference list)

Now we show that the algorithm terminates, and give a bound on the maximum number of iterations needed for termination.

(1.3) The Gale-Shapley algorithm terminates after at most n^2 (n * n) iterations of the While loop

**Proof.** In every round of the While loop, one man proposes to one woman;
every man can propose to a woman at most once;
thus, every man can make at most n proposals;
there are n men, so in total they can make less than or equal to n^2 proposals.
Thus, the While loop can be executed no more than n^2 many times. (end of the proof)

(1.4) If m is free at some point in the execution of the algorithm, then there is a woman to whom he has not yet proposed (i.e. every man is eventually paired with a woman).

**Proof.** Suppose there comes a point when m is still free after the While loop has terminated.
They by (1.1), m has already proposed to every woman.
Thus, every woman is paired with a man, because a woman is not paired with anyone only if no one has made proposal to her.
But this would mean that n women are paired with all of n men so m cannot be free, so this is a contradition. (end of the proof)
