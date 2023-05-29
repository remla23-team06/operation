# Increasing Sentiment Validation: The Impact of Emoji Feedback on ML Model Evaluation

## The IVs (Independent Variable)

> Type of feedback from text to emoji.

We changed the feedback from `correct` to 👍 and `incorrect` to 👎.

## The DVs (Dependent Variable)
- validations $v$
- model predictions $p$


We will be measuring the ratio:
$$\frac{v}{p}$$

## Hypothesis
> The difference in ratios $\frac{v}{p}$ is not significant or the mean is not bigger than the text case when the feedback options have been changed. ($H_0$)


The ratio $\frac{v}{p}$ increases as the feedback options for the user are indicated with emoji rather than text. ($H_A$)

## Decision process

We will make two histograms where the ratios are plotted per user. One histogram for the text case and another for the emoji case.
If the mean of the emoji case is bigger than the text case and the distances of the means are wide enough for it to be significant (one-tailed independent samples t-test); 
then we can reject $H_0$ in favor of $H_A$.




