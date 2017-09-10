---
date: 2017-09-10T13:24:38-04:00
title: "Probabilistic View of Getting Jobs"
---

<!-- https://github.com/rstudio/blogdown/blob/master/docs/01-introduction.Rmd -->

## Background

I like probability and statistics, mainly because they are very useful for organizing thoughts and details. This blog post is an attempt to use those mathmatical tools to understand complex things surrounding us, using US software-engineer job market as an example.

 
## Formalization

Let `$\mathbf{J}$` and `$\mathbf{C}$` be the sets of software-engineer job **positions** and **candidates**, respectively. For notational conventions, I use two numbers `$n$` and `$m$` as `$|\mathbf{J}| = n$` and `$|\mathbf{C}| = m$`. Then, **The US job market** `$\mathbf{U}$` is a tuple of positions and candidates, `$(\mathbf{J}, \mathbf{C})$`.


Let `$Y_{ij}$` (`$i \in \{1, 2, \ldots, n\}, j \in \{1, 2, \ldots, m\}$`) be a random variable denoting a job offer of a software-engineer position `$i$` to a candidate `$j$`. The statement `$Y_{ij} = 1$` means "an j-th applicant got the i-th job offer" and `$Y_{ij} = 0$` means "the company didn't provide the applicant an offer". For simplicity, I will use a symbol `$Y$` as a fixed instance of `$Y_{ij}$`. You could think this is the case when you (j-th fixed candidate) really want to get a particular position `$i$` in your dream company.

Note that `$\sum_{j = 1}^{m}{Y_{ij}} = 1$` does not necessarily hold since some positions are offered to multiple people.

`$Y$` can be decomposed into four binary random variables, `$A$`, `$S$`, `$C$`, and `$R$`. The four variables encode whether the applicant goes through the processes of **Application**, **Resume Screening**, **Coding Interview**, and **the Rest**, respectively. For example, `$A = 1$` means the applicant applied to a job position, and `$A = 0$` means he or she did not. And `$S = 1$` means "the applicant passed Resume Screening."

In other words, the random variable `$Y$` can be described as a random vector `$\mathbf{Y} = (A, S, C, R)$`. The random variable `$Y$` takes a value of 1 only if its corresponding random vector `$\mathbf{Y}$` takes the value of (1, 1, 1, 1). 


If you serious about getting the position, you would implicitly or explicitly think about the probability of the company provides a job offer to a particular applicant j with a techincal skill set `$\mathbf{X}_j$`. That is, `$P(\mathbf{Y} = \mathbf{1} | \mathbf{X}_j)$`. Then, you would try to know more about this probability by looking at some numbers on news websites, asking friends who got their positions recently, or just integrating all the information you obtained.

Since you learn [Bayes Rule](https://en.wikipedia.org/wiki/Bayes%27_theorem) for decomposing joint probability into the product of conditional and merginal probabilities, you would proceed your inference as the following equalities:

`$$
\begin{align}
P( \mathbf{Y} | \mathbf{X}_j )
 & = P( A, S, C, R | \mathbf{X}_j ) 
 \\\\ & = P(A | \mathbf{X}_j ) P(S | \mathbf{X}_j, A = 1)
 \\   & \ \ \ \ \ P(C | \mathbf{X}_j, A = 1, S = 1 ) P(R | \mathbf{X}_j, A = 1, S = 1, C = 1)
\end{align}
$$`

`$P(A, S, C, R)$` is a four-dimensional discrete probability distribution which underlies everywhere in the (software-engineer) US job markets, and `$P(A, S, C, R | \mathbf{X}_j)$` is its personalized version. Everything you obtained from various information sources can be embedded in this framework or its varients. While more sophisticated formalizations are possible, I am happy enough with this (over-)simplified view or model.

We could discuss any part of this probabilistic model, and I will pick `$P(A | \mathbf{X}_j )$` as an example for understanding the scale and mechanism of US software-engineer job markets.

## About `$P(A | \mathbf{X}_j )$`

`$P(A | \mathbf{X}_j )$` is a probability distribution showing how likely the applicant will apply to a particular position, given the applicant's technical skill set. 

> "The probability of whether he/she applies to the job? Isn't that just the problem of willpower and braveness?"

Well, no. What I observed is the application variable `$A \in \{0, 1\}$` can become zero or one irrespective of whether we want to apply for the job.

+ **Reference**: I got several job interviews via references, all of them were not in the public job markets such as company's web sites. Therefore, there was no hope to know about the positions through the Internet. This seems to happen in the case that the company is in its first stage, and/or **the founders already belong to large existing developer communities**; they can quickly recruit skilled developers from previous communities. An interesting aspect of this hiring process is that your technical skill set `$X_j$` affects only when you have some access to the community. No matter how skilled you are in the particular tasks, your `$P(A | \mathbf{X}_j )$` would be zero unless you are in the developer community. This suggests even if you are not actively applying to jobs, it might be better to belong to communities and converse with others for potential career paths. From that perspective, `$A = 1$` does not actually mean "the applicant applied to a job position" but means "the engineer is recognized by recruiters as a potential applicant." Willpower cannot make deterministically the variable `$A$` take a particular value.

<!-- I saw PhD students tend to get their positions through their faculty advisors; becoming a PhD student already requires lots of skills and achievements, so it depends on our formalization whether "being a researcher in academia" is in `$\mathbf{X}_j$`. I wonder if this sometimes leads to inefficiency because some master/undergrad students had multi-year (research) experience in industry, and they could be the best match for the position. But I know, hiring process is not afraid of false negatives; contacting PhD students is the safest bet. -->

+ **The number of job posting websites you know**: while I browsed several job posting sites last year, I found that some companies posted their job descriptions only in subsets of websites. They might want to get a small number of candidates (because some companies get flooding number of applications), they might be doing A/B test for job posting sites, or they thought posting to hundreds of websites is not worth their time. Regardless of the truths, if you don't find a particular post for the job `$i$`, your `$P(A | \mathbf{X}_j )$` is zero.

### Discussion

There are several random thoughts about `$P( A, S, C, R | \mathbf{X}_j ) $`:

+ If we measure KL-divergence between the unconditional `$P(A, S, C, R )$` and the conditional `$P(A, S, C, R | \mathbf{X} = \mathbf{X}_{you})$`, we could measure how the applicant is competitive in the job market; that's the whole point of your engineering education and self-learning. And potentially answer the following questions:
  - How effective the customized resume is for passing Resume Screening?
  - How much practice is the threshold for going through coding interviews?
  - What's the most typical skills applicants with high `$P(R | \mathbf{X}_j, A = 1, S = 1, C = 1)$` have? 
+ This distribution implicitly assumes US job market. If we regard the unconditional `$P(A, S, C, R )$` is actually dependent on the geological location area `$G$`,  `$P(A, S, C, R | G = USA )$` and  `$P(A, S, C, R | G = (other_area) )$` would help understand US job market characteristics. I feel the software-engineer job markets in US and my home country (Japan) are completely different, but I can't picture it clearly without getting more numbers.
+ Sometimes you get an interview **without application**: your friend refers you to a company before you know (i.e. automatically set your `$A = 1$`), and its recruiter thought your LinkedIn fit the position (`$S = 1$`).
+ Typical first steps of CS student's behavior is perhaps like this:
  - **making `$P(A | \mathbf{X}_j )$` bigger:** Job search by subsets of websites / referrals
  - **making `$P(S | \mathbf{X}_j, A = 1)$` bigger:** To prepare resumes, websites, etc.
  - **making `$P(C | \mathbf{X}_j, A = 1, S = 1 )$` bigger:** To practice on Leetcode
  - **making `$P(R | \mathbf{X}_j, A = 1, S = 1, C = 1)$` bigger:** To prepare interviews
  In what condition this behavior is optimal? Each student has different backgrounds and skill sets. One example is winners of ACM-ICPC, whose `$P(C | \mathbf{X}_j, A = 1, S = 1 )$` might already be saturated. They are smart enough to focus on other terms, but is there any typical mistakes that normal CS students would take?


### Conclusion

In this post, I attempted how I could make an inference on US software-engineer job market. There are some findings which I by myself found interesting:

+ Your application can happen (`$A = 1$`) without you actually applying the position, if you are recognized in developer communities.
+ Sometimes you cannot apply positions regardless of your skill set due to posting distribution. That is, `$\forall \mathbf{X}_j \ P(A = 1| \mathbf{X}_j ) = 0$`. This suggests the model needs to expand covariates `$\mathbf{X}_j$` from a technical skill set to more broader technical/non-technical factors, including information survey skills and social skills.

In this semester, I would take one more Bayesian Machine Learning class. I hope that would give me more power to improve my current formalization.