# Assessing Balance in Natural Experiments with Machine Learning

In this project Tyler Reny and I use various machine learning techniques to asses balance in natural experiments. In the experimental framework, causal inference can be made since the potential outcomes of the treatment and control group are identical due to researcher controlled randomization. However, social scientists are often confronted with the inability to randomize due to feasibility and ethical concerns. One way to get past that is to find natural sources of variation, referred to as natural experiments where the researcher provides convincing evidence that the only difference between two groups is due to as-if randomization. The example we examine in this project is media market discontinues. 

Media markets in the U.S. sometimes cross state lines which some researchers argue provides a natural source of variation or a natural experiment. These researchers suggest this is one way to test the impact of media in a campaign context. Think about Nevada and California. In most presidential elections, NV is a battleground state and receives much more campaign advertisement compared to California, a safe Democratic state. Yet, the Reno media market actually crosses into parts of CA. Thus, those living in CA are receiving the campaign content primarily intended for residents in the Reno market. The researchers make the case that the only difference between someone who lives in CA in the Reno market compared to someone who lives in CA and is in another media market is as-if random. Intuitively, this makes some sense and the closer you live to this discontinuity, the more credible the case seems. Researchers have gone one step further and provided some empirical support that the people are not statistically different by providing tables and figures that show the balance on a number of covariates. 

Balance in researcher controlled experiments is guaranteed by randomization. This is true for observable and unobservable covariates, even though its impossible to ever test this assumption on unobservable factors. It does work on things that are observable however. Here is a toy example to prove to yourself this works. We know age and income aren't distributed that way, but this will almost always work regardless of the distribution of these.

```{r}
library(tidyverse)

N <- 10000 # 10,000 respondents
id <- 1:10000 # id number
age <- round(runif(N, min = 18, max = 98),0) # lets say age is uniform
income <- rnorm(N, 50000, 5000) # and inc is normal

df <- data.frame(id, age, income) # make a data.frame

df$treat <- sample(rep(0:1, each = nrow(df)/2), nrow(df), replace = FALSE) # assign treatment

df %>% group_by(treat) %>% 
  summarise(age = mean(age, na.rm = T), # mean of age
            income = mean(income, na.rm = T)) # mean of income 

t.test(df[df$treat == 1,]$age, df[df$treat == 0,]$age) # t.test 
t.test(df[df$treat == 1,]$age, df[df$treat == 0,]$age)$p.value # just the pvalue

t.test(df[df$treat == 1,]$income, df[df$treat == 0,]$income) # t.test
t.test(df[df$treat == 1,]$income, df[df$treat == 0,]$income)$p.value # just the pvalue
```

If we know this works, then we should be able to apply the same idea to the natural experiments right? If we gather up a bunch of covariates, we can do the same thing and we can show that the literally is no difference between treatment and control groups, right? In theory yes, but there are a number of issues that come up. 

# Our Applied Example

We replicated the 2007 paper "Identifying the Persuasive Effects of Presidential Advertising" by Gregory A. Huber (Yale University) Kevin Arceneaux (Temple University) available in *AJPS.* We present the code and data to perform the replication as well as a short write up of our findings and a presentation we used for the class. 

One of the first issues is the potential for balance on linear and addictive functions of the covariates yet the possibility that in non-linear or non-additive functions, there is imbalance. We figured machine learning is the correct approach to solve this type of problem. While it's conceivable to write out all meaningful non-linear or non-additive functions, this process quickly becomes quite intensive. It's also quickly realized that there a too many possible functions to write down. Machine Learning gets around this problem. Since this was part of a course, we used a number of standard ML algorithms to start and then moved to more complex ones later. 

As applied social scientists, we also had another goal. If there is imbalance outside simple linear and additive functions, what do we do about it? For us, solutions to address the problem are almost as important as the very problem itself. We tried a number of matching techniques to solve this problem when we did find imbalance. That code and write is up also included. 








