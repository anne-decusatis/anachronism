---
layout: post
title:  "Statistics: rapier crown support"
date:   2024-04-05 11:00:00 -0400
categories: 
- "event"
---

## There is overwhelming support among survey respondents for a rapier crown variance for the East Kingdom

This is a response to the following:

* The Alternative Crown Feedback survey <https://ma.eastkingdom.org/results-of-the-alternative-crown-feedback-survey/>
* The data from the current Eastern crown's poll is linked from there, and is available at <https://drive.google.com/file/d/1zR070bjEwnieGrGGgstt6bOeOkAecEVR/view>
	* There were 1518 distinct respondents
	* There were 484 distinct respondents in the East Kingdom
	* 94% of East Kingdom respondents were in favor of a rapier crown tournament: 30 no and 454 yes (the percentage was 93% over all kingdoms)

This survey is a direct response to the Board of Directors saying that more data needed to be collected before a variance would be up for approval: <https://ma.eastkingdom.org/responding-to-the-boards-feedback-regarding-the-spring-crown-variance/>

## But this survey response was self-selecting, so it could be that literally everyone who didn't respond feels a certain way and the result is biased

In general this is true. A sample should be representative to more easily draw a relevant conclusion. But you wouldn't say that it wasn't representative if every person except one responded. Where is the cutoff? Statistics should be able to tell us this.

### The statistical test setup

Alternative hypothesis: 67% or more of the East Kingdom supports Rapier Crown in the East Kingdom.

Null hypothesis: We don't know whether more than 2/3 of the East Kingdom supports Rapier Crown Tournament. 

I picked 67% arbitrarily as my personal value for "overwhelming support, above which we should definitely do it".

Cutoff: p-value of 0.05 (standard choice)

As of Sept 2023 (the latest available data at time of writing) the East Kingdom had 2900 members: <https://www.sca.org/wp-content/uploads/2023/10/2023-Membership-Summary-to-9-30.pdf>

It is possible to be a person who plays in the SCA or responds to this survey without being a currently paid member. Feb 2023 was the 2023 high point of the data available, and saw 3167 members in the East. In 2022, the East Kingdom never broke 3000 members <https://www.sca.org/wp-content/uploads/2023/02/2022MembershipStats.pdf> but there's been an uptick in members as we rebound from worst-of-the-pandemic behaviors in my kingdom and I believe in the East as well. 

I'll round up a little bit and say there's 3200 people in the East Kingdom. 

### Using a statistical test

This is the hard part for me! As a software engineer (even one with some specialized training in data science), I don't do statistics often enough to really have this down. Given that I currently work for a company which literally sells interpretations of real-world sampled medical data this is kind of embarrassing, although to be honest I just write the pipelines to put the data in the right place for the analysts to work on it. Anyway, I usually google it and then ask a friend who's an actual scientist/researcher if it sounds right (thanks Bella, Clara, and Kett for suggestions!!)

Here are the suggestions that my statistically oriented friends and internet searches gave me. I'm going to not just calculate but also interpret whether they made sense in this case, since running the math is trivially easy compared to interpreting the result. 

#### Sample size calculator
Use a sample size calculator to confirm the margin of error for the sample of the population that we got. 

Important note: since our sample cannot be assumed to be representative I don't think this is actually valid - the suggestor normally does survey design in the standard way of picking a sample size and then getting that many presumed-representative-sample respondents from e.g. a museum kiosk. Anyway the calculator says "there is a 95% chance that the real value is within ±1.95% of the measured/surveyed value." <https://www.calculator.net/sample-size-calculator.html?type=2&cl2=95&ss2=484&pc2=94&ps2=3200&x=Calculate#findci> 

This illustrates the danger of statistics - it is easy to do math but hard to know if the necessary preconditions all hold.

#### t-test
Use a single-tailed t-test where the continuous variable we're comparing between sample and EK population is the % of yes respondents

One pro of this is that t-tests are bog-standard for statistics, so in general I have more confidence that I'm not going off on a weird tangent. 

Minor note for reader: At this point I gave up on finding a specific enough calculator online and opened up <https://docs.scipy.org/doc/scipy/reference/stats.html>

An answer from <https://stackoverflow.com/a/47926231> also gave me some info for how to call this in scipy. I used the sample_mean of 454/484, sample_std calculated for a Bernoulli distribution of sqrt(p(1-p)) <https://en.wikipedia.org/wiki/Bernoulli_distribution>, n_samples 484, mean_2 the .67 that we're testing, std2 0 per SO answer, and per SO answer and my experimentation, nobs2 does not affect the answer as long as it's positive so I set it to 3200 as discussed above. equal_var=False gives us a Welch's t-test for unequal variances <https://en.wikipedia.org/wiki/Welch%27s_t-test>. alternative=greater makes it one-tailed in the appropriate directionality. 

```
>>> ttest_ind_from_stats(mean1=sample_mean, std1=sample_std, nobs1=n_samples, mean2=pop_mean, std2=0, nobs2=pop_n, equal_var=False, alternative=alternative)
Ttest_indResult(statistic=24.453502200458754, pvalue=7.824018377966384e-87)
```
This result gives us a very tiny p-value (see the e-87 at the end) so we can feel confident in rejecting the null hypothesis. 

#### Other statistical methods to explore in the future
Use a general linear model (GLM), graph it, look for 67% on the graph (suggestor then clarified this doesn't take population size into account and might not be as valid due to that)
I didn't do this because it's marginally harder to graph in Python (the language I use) than it is in R (the language the suggestor uses), and I just ran out of time. 

Use a chi-squared goodness of fit test (I found this online but in several places)
This is probably viable but I gave up on trying to format binary data for SciPy and the online calculator I tried had too many ads to load properly.

## Conclusion

I feel fairly confident that at least 2/3 of the East Kingdom is in favor of a rapier crown, given that 90+% support occurred at polling time and 15% of the population turned out for the poll.

I'll leave you with some thoughts from my group chat, which is where I asked my friends who are researchers: 

> "honestly I think common sense stands here" - Anonymous Person A

and

>  "no vote no opinion xoxo" - not one of the scientists, just someone else in the group chat


