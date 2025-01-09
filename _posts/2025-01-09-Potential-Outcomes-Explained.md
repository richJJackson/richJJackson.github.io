---
layout: post
title:  "Potential Outcomes"
date: 2025-01-09
categories: jekyll update
---


One of the things I'm involved with at the moment is to set up a course to help non-statisticians understand causal inference. At some point during the initial discussions I agreed to introduce potential outcomes, probably because the actual seemed so far away. But these things do tend to creep up on you.   There was one condition, given the audience, I had to avoid if as much mathematical notation as is possible.

You may well have heard something along the lines of "Correlation does not imply causation".  Its one of those handy phrases we like to use when we've found something interesting in our data.  A kind of insurance we attach to cover ourselves.  For the most part this is fine, but when it comes to interventions (e.g. treatments) correlation isn't enough.  If we find that patients are doing better on the new treatment then we want to know that the treatment *caused* this effect.

So how do we get from correlation to causation?? To understand this, we need to understand potential outcomes.  Lets say we have a single patient who is about to be delivered one of two treatments and at some point following the treatment we can measure their outcome.  Our imaginary patients path will look something like this

![potOut_fig1](/assets/potOut_fig1.png)

We call out treatment assignment 'A' which can take two values; A=0 for the control treatment and A=1 for the experimental treatment.  

Our patients outcome is Y.  The main principle of understanding potential outcomes is that before the treatment is administered, patients have 2 potential outcomes.  
* Their outcome if they receive the control treatment; Y(0)
* Their outcome if they receive the experimental treatment; Y(1)

This, slightly confused patient, is what we will use to represent a patient with both potential outcomes before they have received any treatment.

![potOut_fig2](/assets/potOut_fig2.png)

If we want to understand which treatment works, ideally we would like to measure Y(0) and Y(1) and estimate their difference; say d = Y(1) - Y(0).  This is our causal estimate and exactly what we are after with the slight hiccup that it is impossible. Oh well - plan B.

In practice one outcome will be observed and and the other will remain unseen and illusive. The snark of the statistical world.

As soon as one treatment is given this decision is made and we then know which patient we have, a Y(0) patient or a Y(1) patient.  The mechanics of *how* this decision is made govern much about the causal inference tools we need to use but more about that some other time.

![potOut_fig3](/assets/potOut_fig3.png)

For convenience lets assume there are no issues in actually delivering the treatment - we now have two subtle but important changes:

1. Patients no longer have potential outcomes but observed outcomes.  We'll simply call this y to distinguish this a single observed value
2. These outcomes are conditional on what treatment a patient had

This first point is key I think, one of the earliest mental blocks I had in understanding potential outcomes was understanding the difference between 'potential' and 'observed' outcomes.

The second point means that in mathematical terms we now have $`y|A=0`$ and $`y|A=0`$.  We can (and do!) still compare these outcomes.  We could write it like this:

$$
d = [y|A=0] -  [y|A=1] 
$$

The only problem is that on it's own - this is not a causal statement. Shame.

The root of this is that they are comparing different populations.  One group is conditional on A=0 and the other is conditional on A=1.  For 'd' above to be a causal estimate we need a little more work.

Exactly *what* work needs to be done depends very much on the setting but what we will need at the very least are a few assumptions.  Three in particular:

1. No Interference
2. Exchangability
3. Consistency

Thankfully these are in no way confusing or convoluted..... Lets start:

## No Interference

	The Exposure of one patient does not effect the potential outcomes of any other patient

My first reaction to reading this was rather bland ("Well obviously").  Why would one patient having a treatment effect in anyway the potential of another patient.  In many cases this is perfectly reasonable.  Where patients are entirely independent certainly (e.g. in different countries).  Even where two patients are placed on the same hospital, treating one patient usually won't impact the potential outcome of another.   

![potOut_fig4](/assets/potOut_fig4.png)

Where this may fall down is in the evaluation of vaccine.  If the treatment is vaccine vs no vaccine and the outcome is getting some infection then one patient being vaccinated may impact the likelihood of another patient getting infected....


# Exchangeability
	The potential outcome under every treatment is the same irrespective of what treatment a patient gets

To me I think of the potential outcome [Y(0) and Y(1)] as being set in stone. E.g. they don't depend on the treatment a patient is given and because of this, we could theoretically *exchange* patients between the two treatment groups and arrive at the same inference. 

![potOut_fig5](/assets/potOut_fig5.png)

Importantly in mathematical terms this allows us to write things like

$$
[Y(0)|A=0] = [Y(0)|A=1] = Y(0)
$$

I.e the potential outcome for the control treatment [Y(0)] stays the same.  If they are allocated to the control group we get to observe it.  If they are allocated to the experimental group then we don't;  but the *potential* outcome hasn't changed.

## Consistency
	A patients observed outcome is the same as the potential outcome under the observed treatment

This says that if a patients is allocated to receive a control treatment the observed outcome [Y(0)] that we see is the same as the potential outcome.  E.G. the act of giving the treatment has not changed the potential outcome, it is 'consistent'.


![potOut_fig6](/assets/potOut_fig6.png)


Mathematically this allows us to say

$$
[y|A=0] = [Y(0)|A=0]
$$

##  A Causal Estimate

Put together these three assumptions have a powerful effect.  No interference ensures that the potential outcomes prior to treatment remain fixed and not altered by other patients.  Exchangeability allows us to state that the potential outcomes are not impacted by the allocation of treatments and consistency ensures that the data we observe are a consistent reflection of the potential outcomes.

Recall at the outset, our aim really was to compare these potential outcomes

$$
d = Y(1) - Y(0)
$$
In practice, for a single patient we are still restricted by only ever observing one outcome.  For a group of patients however we will observe both.   This leads us to estimating the Average Treatment Effect (ATE).  If we call this $\tau$ we can write this as

$$
\tau = \sum_N \frac{d}{N} = E[Y(1) - Y(0)]
$$
Conceptually, the ATE is simply the expected value of the differences in all the potential outcomes.  It's convenient to split up the expectations...

$$
\tau  = E[Y(1)] - E[Y(0)]
$$

Recall from the definition of exchangeability above, we can now re-write this as: 

$$
\tau = E[Y(1)|A=1] - E[Y(0)|A=0]
$$
And lastly due to consistency we can write:

$$
\tau = E[y|A=1] - E[y|A=0]
$$

This is precisely what we observe.  So when we carry out an experiment and compare treatments between two groups.  This is what allows us to say that a treatment has had a causal effect.  At last.
## Last note

I think it's perfectly natural to think about this problem in the context of Randomised Controlled Trials.  Whilst not without their faults RCTs are not called the gold standard for no reason.  Many of the assumptions required for causal inference are easier to justify in an RCT than they would be in (say) an analysis performed on an observational dataset.

I hope (if you have got this far) that you have found this in some way useful but its important to note that there is a lot here we haven't even touched on yet.  Perhaps most importantly the role of potential confounders and the mechanism by which treatment is allocated.

Randomisation is powerful because it controls both these elements.  The allocation mechanism is removed from clinicians/patients and any potential confounders (be they known or unknown) are, conceptually at least, balanced out between the treatment groups.

Where Causal Inference is attempted on observational datasets the analyst has to worry about both of these issues and that is the reason for their being a number of different tools which can be applied.  Each have their own intended settings, assumptions and idiosyncrasies to be understood. 

For those interested; Multivariable Regression, G-Computation, Instrumental Variables, Synthetic Controls, Difference-in-Differences, Regression Discontinuity, Targeted Maximum Likelihood Estimators and Propensity Score Analyses are all tools that have been applied.

And then there are Causal Machine Learning approaches, controlling for treatment (non) compliance and the longitudinal effect of treatments given over time.... but maybe thats a good place to stop.

