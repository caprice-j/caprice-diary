---
date: 2017-09-10T13:24:38-04:00
title: "Probabilistic View of Software-Engineer Job Market"
---

<!-- https://github.com/rstudio/blogdown/blob/master/docs/01-introduction.Rmd -->

## Background

I like probability and statistics in that they are useful for organizing thoughts and details. This blog post is an attempt to use those mathmatical frameworks to understand complex things surrounding us like US software-engineer job market.


## Why job market?

I'm looking for a full-time job and having a motivation for figuring out how things are connected to each other.
 
## Formalization

Let `$\mathbf{C}$` and `$\mathbf{J}$` be the sets of software-engineer **job candidates** and **job positions**, respectively. For notational conventions, I use two numbers `$n$` and `$m$` as `$|\mathbf{C}| = n$` and `$|\mathbf{J}| = m$`. Then, **the US job market** `$\mathbf{U}$` is a tuple of candidates and positions, that is, `$ \mathbf{U} = (\mathbf{C}, \mathbf{J})$`.


Let `$Y_{ij}$` (`$i \in \{1, 2, \ldots, n\} = \mathbf{C}, j \in \{1, 2, \ldots, m\} = \mathbf{J}$`) be a random variable denoting a job offer to a candidate `$i$` for a software-engineer position `$j$`. The statement `$Y_{ij} = 1$` means "an i-th candidate got the j-th job offer" and `$Y_{ij} = 0$` means "the company didn't provide the candidate an offer". Note that `$\sum_{i = 1}^{n}{Y_{ij}} = 1$` does not necessarily hold since some positions are offered to multiple candidates.

`$Y_{ij}$` can be decomposed into four binary random variables, `$A_{ij}$`, `$S_{ij}$`, `$C_{ij}$`, and `$R_{ij}$`. For simplicity, I will write them by `$Y$`, `$A$`, `$S$`, `$C$`, and `$R$`, respectively (only for `$Y$`, I'll sometimes write explicitly as `$Y_{ij}$`, though.) These four variables encode whether the candidate goes through the processes of **Application**, **Screening**, **Coding Interview**, and **Remaining Processes**, respectively. For example, `$A = 1$` means the candidate applied to a job position, and `$A = 0$` means he or she did not. And `$S = 1$` means "the candidate passed Screening." Screening can have multiple phases like resume screening and then background screening (e.g. GPA), but here we simply denote them by a single variable `$S$`.

To put it another way, the random variable `$Y_{ij}$` can be described as a random vector `$\mathbf{Y}_{ij} = (A, S, C, R)$`. The random variable `$Y_{ij}$` takes a value of 1 if and only if its corresponding random vector `$\mathbf{Y}_{ij}$` takes the value of `$(1, 1, 1, 1)$`. Random varibles `$Y_{ij}$` are useful for examining **results** (offer or not), and random vectors `$\mathbf{Y}_{ij}$` are useful for examining **processes**.


As we are interested in getting the position, we would implicitly or explicitly think about the probability that the company provides a (j-th) job offer to a particular candidate `$i$` with a technical skill set `$\mathbf{X}_i$`. That is, `$P(\mathbf{Y}_{ij} = \mathbf{1} | \mathbf{X}_i)$`. Then, you would try to know more about this probability by looking at some numbers on news websites, by asking friends who got their positions recently, or by integrating all the information you obtained. Or, you might focus on expanding your skill set `$\mathbf{X}_i$`.

BY using a formula for decomposing joint probability into the product of conditional and merginal probabilities, we can make our inference as the following equalities:

`$$
\begin{align}
P( \mathbf{Y} | \mathbf{X}_i )
 & = P( A, S, C, R | \mathbf{X}_i ) 
 \\ & = P(A | \mathbf{X}_i ) P(S | \mathbf{X}_i, A)
 \\ & \ \ \ \ \ P(C | \mathbf{X}_i, A, S ) P(R | \mathbf{X}_i, A, S, C)
\end{align}
$$`

`$P(A, S, C, R)$` is a four-dimensional discrete probability distribution which underlies everywhere in the (software-engineer) US job markets, and `$P(A, S, C, R | \mathbf{X}_i)$` is its personalized version. In other words, for a candidate having a skill set `$X_i$`, how hard is it to get the job? Everything you obtained from various information sources can be embedded in this framework or its varients. While more sophisticated formalizations are possible, I am happy enough with this (over-)simplified view; that is, this model basically claims if you apply for a job (`$A = 1$`), pass Screening (`$S = 1$`), show your coding skills in Coding Interviews (`$C = 1$`), and do reasonably well whatever you are asked during the Remaining Processes (`$R = 1$`), you would get a job offer (`$\mathbf{Y} = \mathbf{1}$`). If we consider each `$Y$` is an "edge" between a candidate node `$i$` and a job position node `$j$`, `$\mathbf{Y} = \mathbf{1}$` means we draw an edge between the two nodes in a [Bipartite graph](https://en.wikipedia.org/wiki/Bipartite_graph) `$\mathbf{U} = (\mathbf{C}, \mathbf{J}, \mathbf{Y}) $` where `$\mathbf{Y}$` is a n x m matrix whose (i, j) element is `$Y_{ij}$`.

We could discuss any part of this probabilistic model, and I will pick `$P(A | \mathbf{X}_i )$` as an example for getting the sense of the job market's mechanism.

## About `$P(A | \mathbf{X}_i )$`

`$P(A | \mathbf{X}_i )$` is a probability distribution showing how likely the candidate will apply to a particular position, given the candidate's technical skill set. 

> "Isn't that just the problem of willpower and braveness?"

Well, it's actually not. What I observed is the application variable `$A \in \{0, 1\}$` can automatically become zero or one irrespective of whether we want to apply for the job. There are at least two factors:

+ **Reference**: I got several job interviews via references, most of them were not in the public job markets such as company's websites. Therefore, there was almost no hope to know about the positions through the Internet. This seems to happen in the case that the company's **founders already belong to large existing developer communities**; they can quickly recruit trustable developers from previous communities. An interesting aspect of this hiring process is that your technical skill set `$\mathbf{X}_i$` affects only when you have some access to the community. No matter how skilled you are in the particular tasks, your `$P(A | \mathbf{X}_i )$` would be zero unless you are in the developer community. This suggests even if you are not actively applying to jobs, it might be better to belong to communities and converse with others for potential career paths. From that perspective, `$A = 1$` does not actually mean "the candidate applied to a job position" but means **"the engineer is recognized by recruiters as a potential candidate."** Willpower cannot make deterministically the variable `$A$` take a particular value. Also, having lots of automatic job applications could be annoying -- I know one person who is working in a prestegious company and gets tons of recruiting messages.

<!-- I saw PhD students tend to get their positions through their faculty advisors; becoming a PhD student already requires lots of skills and achievements, so it depends on our formalization whether "being a researcher in academia" is in `$\mathbf{X}_j$`. I wonder if this sometimes leads to inefficiency because some master/undergrad students had multi-year (research) experience in industry, and they could be the best match for the position. But I know, hiring process is not afraid of false negatives; contacting PhD students is the safest bet. -->

+ **The number of job posting websites you know**: while I browsed several job posting sites last year, I found that some companies posted their job descriptions only in subsets of websites. They might want to get a small number of candidates (because some companies get a tremendous number of applications), they might be doing A/B tests for job posting sites, or they thought posting to hundreds of websites is not worth their time. Regardless of the truths, if you don't find a particular post for the job `$i$`, your `$P(A = 1 | \mathbf{X}_i )$` is zero unless you get some referrals.

### Discussion

There are several other thoughts about `$P( A, S, C, R | \mathbf{X}_i ) $`:

+ If we measure KL-divergence between the unconditional `$P(A, S, C, R )$` and the conditional `$P(A, S, C, R | \mathbf{X}_{you})$`, we could measure how the candidate is competitive in the job market; that's the whole point of your engineering education and self-learning. And it potentially answers the following questions:
  - How effective the customized resume is for passing Screening?
  - How much practice is the threshold for going through Coding Interviews with high probability?
  - What's the most typical skills candidates with high `$P(R = 1 | \mathbf{X}_i, A = 1, S = 1, C = 1)$` have? 
+ This distribution implicitly assumes US job market. If we consider the unconditional `$P(A, S, C, R )$` is actually dependent on the geological location area `$G$`,  `$P(A, S, C, R | G = USA )$` and  `$P(A, S, C, R | G = (somewhere) )$` would help understand US job market characteristics. I feel the software-engineer job markets in US and my home country (Japan) are completely different.
+ Typical first steps of CS students are perhaps like this:
  - **making `$P(A = 1 | \mathbf{X}_i )$` bigger:** Job search by subsets of websites / referrals
  - **making `$P(S = 1 | \mathbf{X}_i, A)$` bigger:** To prepare resumes, websites, etc.
  - **making `$P(C = 1 | \mathbf{X}_i, A, S)$` bigger:** To practice on Leetcode
  - **making `$P(R = 1 | \mathbf{X}_i, A, S, C)$` bigger:** To prepare for interviews
  In what condition this behavior is optimal? Each student has different backgrounds and skill sets. One example is winners of ACM-ICPC, whose `$P(C = 1| \mathbf{X}_i, A = 1, S = 1 )$` might already be saturated. They are smart enough to focus on other terms, but is there any typical mistakes that normal CS students would take?


### Conclusion

In this post, I attempted how I could think abstractly about US software-engineer job market. There are some findings which I by myself felt interesting:

+ Your application can happen (`$A = 1$`) without you actually applying to the position, if you are recognized as enough skilled in developer communities.
+ Sometimes you cannot apply positions regardless of your technical proficiency due to posting distribution. That is, `$\forall \mathbf{X}_i \ P(A = 1| \mathbf{X}_i ) = 0$`. This suggests the model needs to expand covariates `$\mathbf{X}_i$` from a technical skill set to more broader technical/non-technical factors, including information survey skills and social skills.

In this semester, I would take one more Bayesian Machine Learning class. I hope that would give me more hints to improve my current formalization.