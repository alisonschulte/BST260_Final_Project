BST 260 Final Project
================
2022-12-16

This analysis focuses on the sexual health of American adolescents using
data from the National Longitudinal Survey of Youth which followed a
cohort of adolescents born 1980-1984 beginning in 1997 to 2019. The
survey is sponsored by the U.S. Bureau of Labor Statistics and includes
8,894 respondents. At the time of the first interview in 1997,
respondents were aged 12 to 18, and are now 34 to 40 years of age. 51%
of the original sample was male and 49% was female. 51.9% of the sample
was Non-Black/non-Hispanic, 26% was Black non-Hispanic, 21.2% of the
sample was Hispanic or Latino, and 0.9% reported being mixed race. This
analysis uses data from the year 2002.

Using this data I sought to:

- Determine if there is an association between sex and ever being
  diagnosed with a sexually transmitted infection (STI).
- Determine if there is an association between use of condom during last
  sexual intercourse and ever being diagnosed with a STI.
- Determine the effect of being female on the likelihood of reporting
  ever being diagnosed with a STI.

The variables I use in this analysis are:

- Sex: Defined as a binary male or female.
- Race: Defined as Non-Black/non-Hispanic, Black non-Hispanic, Hispanic
  or Latino, or mixed race.
- Condom: If the respondent or their partner used a condom at the time
  of last sexual intercourse.
- STI: If the respondent has ever been diagnosed with a STI other than
  HIV/AIDS.

I begin by preparing the data:

    ## # A tibble: 6 x 12
    ##      ID   Sex  BD_M  BD_Y Sample_T~1  Race Condom Other   HIV   STI K_preg K_STI
    ##   <dbl> <dbl> <dbl> <dbl>      <dbl> <dbl>  <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl>
    ## 1     1     2     9  1981          1     4     -4    -4    -4    -4     -4    -4
    ## 2     2     1     7  1982          1     2     -4    -4    -4    -4     -4    -4
    ## 3     3     2     9  1983          1     2      1     6    -4    -4     -1     2
    ## 4     4     2     2  1981          1     2     -4    -4    -4    -4     -4    -4
    ## 5     5     1    10  1982          1     2      1     3    -4    -4     -4    -4
    ## 6     6     2     1  1982          1     2     -4    -4    -4    -4     -4    -4
    ## # ... with abbreviated variable name 1: Sample_Type

    ## [1] 1

I was hoping to investigate both STIs and HIV/AIDS diagnoses, but in
this dataset, there was only 1 HIV diagnosis, so the analysis would not
be useful or valid.

    ## 
    ##         0         1 
    ## 0.1764706 0.8235294

The proportion of respondents who reported that they or their partner
used a condom at the time of last sexual intercourse is 0.824.

    ## 
    ##          0          1 
    ## 0.04117647 0.95882353

The proportion of respondents who reported using any form of modern
birth control at the time of last sexual intercourse is 0.959. I define
modern birth control as: condom, foam, jelly, cream, sponge, or
suppository, diaphragm, IUD, morning after pill, birth control pill,
depo-provera or injectable, or norplant. I do not include withdrawal or
rhythm (safe time).

My approach to answering my research question will begin with a simple
test of association to determine if there is statistically significant
evidence of an association between sex and reporting ever being
diagnosed with an STI. The data is binomial data and not all cells in
the 2x2 table are greater than 5, so I will use a Fisher’s Exact Test.
To determine if there is an association between using a condom at last
sexual intercourse and reporting ever being diagnosed with a STI, I will
use a Fisher’s Exact Test as well. Then, I will conduct a binomial
regression to determine the effect of having correct knowledge about
what form of birth control is most effective for preventing STI
transmission on reporting ever being diagnosed with a STI. I will
estimate the effect using a logit and a probit model while controlling
for race and then conduct a likelihood ratio test to determine which is
a better estimation.

**Results**

Tests of Association

Is there an association between sex and reporting ever being diagnosed
with a STI?

    ##          
    ##           Male Female
    ##   No STI    70     88
    ##   Yes STI    1     11

    ## 
    ##  Fisher's Exact Test for Count Data
    ## 
    ## data:  data$STI and data$Sex
    ## p-value = 0.01514
    ## alternative hypothesis: true odds ratio is not equal to 1
    ## 95 percent confidence interval:
    ##    1.207452 381.243185
    ## sample estimates:
    ## odds ratio 
    ##   8.671163

This test indicates that based on these data, there is a statistically
significant association between sex and reporting ever being diagnosed
with a STI (p = 0.01514).

Is there an association between use of condom during last sexual
intercourse and reporting a STI?

    ##             
    ##              No STI Yes STI
    ##   No Condom      27       3
    ##   Yes Condom    131       9

    ## 
    ##  Fisher's Exact Test for Count Data
    ## 
    ## data:  data$Condom and data$STI
    ## p-value = 0.446
    ## alternative hypothesis: true odds ratio is not equal to 1
    ## 95 percent confidence interval:
    ##  0.1421317 3.7928193
    ## sample estimates:
    ## odds ratio 
    ##  0.6203122

This test indicates that based on these data, there is not a
statistically significant association between the respondent or their
partner using a condom at the time of last sexual intercourse and
reporting ever being diagnosed with a STI.

Binomial Regression

What is the effect of being female on the likelihood of reporting ever
being diagnosed with a STI? Beginning with a logit model:

    ## 
    ## Call:
    ## glm(formula = STI ~ female + Black + Hispanic, family = binomial(link = "logit"), 
    ##     data = data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -0.5570  -0.5289  -0.3190  -0.1508   2.9940  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  -3.9910     1.0306  -3.872 0.000108 ***
    ## female        2.2061     1.0588   2.084 0.037193 *  
    ## Black        -1.1675     1.0932  -1.068 0.285525    
    ## Hispanic     -0.4798     0.7135  -0.672 0.501328    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 86.754  on 169  degrees of freedom
    ## Residual deviance: 77.968  on 166  degrees of freedom
    ## AIC: 85.968
    ## 
    ## Number of Fisher Scoring iterations: 7

    ## `geom_smooth()` using formula = 'y ~ x'

![](index_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

This analysis indicates that being female significantly reduces the
likelihood of reporting ever being diagnosed with a STI by 2.21 points
(p \<= 0.01). There is no significant effect of being Black or Hispanic
on the likelihood of reporting ever being diagnosed with a STI.

How does the estimate change when using a probit model?

    ## 
    ## Call:
    ## glm(formula = STI ~ female + Black + Hispanic, family = binomial(link = "probit"), 
    ##     data = data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -0.5457  -0.5254  -0.3190  -0.1556   2.9733  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  -2.0747     0.4054  -5.117  3.1e-07 ***
    ## female        0.9870     0.4251   2.321   0.0203 *  
    ## Black        -0.5608     0.5086  -1.103   0.2702    
    ## Hispanic     -0.1813     0.3507  -0.517   0.6050    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 86.754  on 169  degrees of freedom
    ## Residual deviance: 78.138  on 166  degrees of freedom
    ## AIC: 86.138
    ## 
    ## Number of Fisher Scoring iterations: 7

    ## `geom_smooth()` using formula = 'y ~ x'

![](index_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

This probit model predicts that being female significantly reduces the
likelihood of reporting ever being diagnosed with a STI by 0.987 points
(p \<= 0.01). There is no significant effect of being Black or Hispanic
on the likelihood of reporting ever being diagnosed with a STI.

Is the logit or probit model a better fit?

    ## Likelihood ratio test
    ## 
    ## Model 1: STI ~ female + Black + Hispanic
    ## Model 2: STI ~ female + Black + Hispanic
    ##   #Df  LogLik Df  Chisq Pr(>Chisq)    
    ## 1   4 -38.984                         
    ## 2   4 -39.069  0 0.1703  < 2.2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

This analysis indicates that the logit model specified above is the
better fit of the two.

**Summary**

These analyses answered the following questions:

- Is there an association between sex and ever being diagnosed with a
  STI?
- Is there an association between use of condom during last sexual
  intercourse and ever being diagnosed with a STI?
- What is the effect of being female on the likelihood of reporting ever
  being diagnosed with a STI?

To answer these questions, I used tests of association and binomial
regression. I found that there is an association between sex and ever
being diagnosed with a STI (p = 0.01514), but there is not evidence of
an association between use of a condom during last sexual intercourse
and ever being diagnosed with a STI. A logistic regression predicted
that being female significantly reduces the likelihood of reporting ever
being diagnosed with a STI by 2.21 points (p \<= 0.01) while a probit
model predicted that being female significantly reduces the likelihood
of reporting ever being diagnosed with a STI by 0.987 points (p \<=
0.01). Neither model predicted any significant effect of being Black or
Hispanic on the likelihood of reporting ever being diagnosed with a STI.
A likelihood ratio test identified the logit model as the better fit for
the data.

This analysis was successful in answering my questions based on the data
available. If I had more time I would be interested in finding data that
had greater internal validity. The quality of the data is limited in
that the researchers asked about biological sex but not gender.
Furthermore, sex is a nuanced concept that many people interpret
differently (e.g. what counts as “sex”), and by asking about practices
at last sexual intercourse, the researchers many not be capturing the
nature of non-heterosexual sex. Furthermore, STI transmission can occur
during sexual acts that are not intercourse. So, as a preliminary
analysis, this is interesting, but the data collection methods could be
more robust. I also worry about desirability bias in these data as
respondents may have altered their answer to seem more in accordance
with health recommendations.

**References**

Bureau of Labor Statistics, U.S. Department of Labor. National
Longitudinal Survey of Youth 1997 cohort, 1997-2017 (rounds 1-18).
Produced and distributed by the Center for Human Resource Research
(CHRR), The Ohio State University. Columbus, OH: 2019.

**Appendix: All code for this report**

``` r
knitr::opts_chunk$set(echo = TRUE)
#Load packages
library(tidyverse)
library(lmtest)

# Load the data
data <- read_csv("C:/Users/alisc/OneDrive - Harvard University/Harvard/Fall 2022/BST 260/BST260_FinalProject/default.csv", show_col_types = FALSE) 

# Rename columns to more clearly reflect the variables
colnames(data) <- c("ID", "Sex", "BD_M", "BD_Y", "Sample_Type", "Race", "Condom", "Other", "HIV", "STI", "K_preg", "K_STI")

head(data)

# Create dummy variables for demographics 
data$female <- ifelse(data$Sex == 2, 1, 0)
data$Black <- ifelse(data$Race == 2, 1, 0)
data$Hispanic <- ifelse(data$Race == 1, 1, 0)

# Remove respondents that skipped the question about condom use at time of last sexual intercourse and about if they have ever been diagnosed with an STI
data <- data[data$Condom != -4 & data$Condom != -5, ] 
data <- data[data$STI != -4 & data$STI != -5, ] 

# Create dummy variable for use of modern birth control method at time of last sexual intercourse 
data$bc <- ifelse(data$Condom == 1 | data$Other == 2 | data$Other == 4 | data$Other == 7 | data$Other == 11 | data$Other == 6 | data$Other == 8 | data$Other == 10, 1, 0)
data$HIV[data$HIV < 0] <- 0
data$HIV <- as.numeric(data$HIV)
sum(data$HIV)
prop.table(table(data$Condom))
prop.table(table(data$bc))
# Is there an association between sex and reporting ever being diagnosed with a STI?
data$STI <- factor(data$STI,
                   levels = c(0, 1),
                   labels = c("No STI", "Yes STI"))

data$Sex <- factor(data$Sex,
                   levels = c(1, 2),
                   labels = c("Male", "Female"))
table(data$STI, data$Sex)
fisher.test(data$STI, data$Sex)
# Is there an association between use of Condom during last sexual intercourse and reporting a STI?
data$Condom <- factor(data$Condom,
                      levels = c(0, 1),
                      labels = c("No Condom", "Yes Condom"))

table(data$Condom, data$STI)
fisher.test(data$Condom, data$STI)
# Logit model
data$STI <- as.numeric(data$STI)
data$STI <- ifelse(data$STI == 2, 1, 0)

m1 <- glm(STI ~ female + Black + Hispanic,  family = binomial(link = "logit"), data = data)
summary(m1)

ggplot(data, aes(x=female, y=STI)) + 
  geom_point() +
  stat_smooth(method="glm", se=FALSE, method.args = list(family=binomial(link = "logit")))
# Probit model

m2 <- glm(STI ~ female + Black + Hispanic,  family = binomial(link = "probit"), data = data)
summary(m2)

ggplot(data, aes(x=female, y=STI)) + 
  geom_point(alpha=.5) +
  stat_smooth(method="glm", se=FALSE, method.args = list(family=binomial(link = "probit")))
# Likelihood Ratio Test
lrtest(m1,m2)
```
