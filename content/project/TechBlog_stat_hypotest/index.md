---
title: 'Statistical (Hypothesis) Test'
summary: 'Notes from statistical test (t-test, Z-test, Chi-Square Test, F-test, U Test, Signed-Rank Test, K-S Test and ANOVA) to p-value'
tags:
  - Math
date: 2024-10-07
---

In `hypothesis testing`, the test statistic is constructed to convert certain characteristics of the sample data (such as mean, variance, etc.) into a quantifiable value for comparison with the critical value under the theoretical distribution. Through the test statistic, we can determine whether the data deviates significantly from the null hypothesis, and thus draw a conclusion whether to reject the null hypothesis. Test statistics are often constructed based on sample data and specific hypothesis testing methods (such as t-test, Z-test, etc.) to ensure that its distribution is known under the null hypothesis, usually the standard normal distribution or chi-square distribution etc. so we can calculate a p-value or determine a confidence interval.
## 1 Statistics
### 1.1 t-test
* Applicable scenarios: The t test is used to compare whether there is a significant difference between two means. It is suitable for situations where the sample size is small or the population standard deviation is unknown.
* Test statistic: t statistic
* The t statistic follows a t distribution.
* t distribution: [t distribution in wiki](https://en.wikipedia.org/wiki/Student%27s_t-distribution#:~:text=In%20probability%20theory%20and%20statistics,%20Student's)
* Variants: independent t-test, paired t-test, etc.
* One-sample t-test $$ t=\frac{\bar{X}-\mu_0}{\frac{s}{\sqrt{n}}} $$
	* $\bar{x}$ is the mean of sample, $\mu_0$ is the hypothesized population mean, $s$ is the sample standard deviation, and $n$ is the sample size. 
	* $df=n-1$
* Independent t-test  $$ t=\frac{\bar{X_1}-\bar{X_2}}{\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}} $$
	* when $s_1=s_2$, $df=n_1+n_2-2$
	* when $s_1 \neq  s_2$, $$df=\frac{ (\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2})^2 }{ \frac{(\frac{s_1^2}{n_1})^2}{n_1-1} + \frac{(\frac{s_2^2}{n_2})^2}{n_2-1} }$$
* Pairs t-test $$ t=\frac{\bar{D}}{\frac{s_D}{\sqrt{n}}} $$
	* $\bar{D}$ is the mean of the paired sample differences, that is, the average of the differences between each pair of data;
	* $s_D$ is the standard deviation of D;
	* n is the number of pairs
	* $df=n-1$
### 1.2 Z-test
* Applicable scenarios: The Z test is used when the sample population is compared big (normally bigger than 30) and known the sample standard deviation. Mainly used to test significant differences in means or proportions.
* Test statistic: Z statistic
* The Z statistic follows a standard normal distribution.
* Z test: $$ Z=\frac{\bar{X}-\mu_0}{\frac{\sigma}{\sqrt{n}}} $$
### 1.3 Chi-Square Test
* Applicable scenarios: The chi-square test is used for categorical data and is suitable for testing the independence or goodness of fit between two variables (i.e., whether the data conforms to a certain theoretical distribution).
* Test statistic: $\chi^2$ statistic
* The chi-square statistic follows a $\chi^2$ distribution.
* $\chi^2$ can be calculated by: $$ \chi^2=\sum \frac{ (O_i-E_i)^2 }{ E_i } $$
	* $O_i$ is the observed frequency
	* $E_i$ is the expected frequency.
### 1.4 F-test (ANOVA)
* The F test is a type of statistical test used to compare the variances or other model parameters of two or more groups of data. Its core is to calculate the F statistic and compare it with the F distribution. The F statistic is usually the ratio of two variances, and its basic form is: $$ F=\frac{Bigger\ Variance}{Smaller\ Variance} $$
* Applicable scenarios:
	* Homogeneity of variance test: used to test whether the variances of two samples are equal.
	* Significance test in regression analysis: used to evaluate the overall explanatory power of the independent variables on the dependent variable in the regression model.
	* Mean difference test in ANOVA: used to compare whether the means of multiple samples are significantly different.
* Analysis of variance (ANOVA) is a specific statistical method used to compare the means of three or more groups for significant differences. The core assumption of ANOVA is that if multiple groups have the same mean, then the difference between the groups should be mainly caused by random error rather than the true difference in means.
	* ANOVA uses the F test for comparison. The core idea is to decompose the overall variation into "variation between groups" and "variation within groups", and compare the variation between groups with the variation within groups to obtain an F statistic. Specifically: $$ F=\frac{MSB}{MSW} $$
		* $$ MSB = \frac{SSB}{df_B} = \frac{\sum_{i=1}^{k} n_i (\bar{X}_i - \bar{X})^2}{k - 1} $$
		* $$ MSW = \frac{SSW}{df_W} = \frac{\sum_{i=1}^{k} \sum_{j=1}^{n_i} (X_{ij} - \bar{X}_i)^2}{N - k} $$
		* Mean square between groups (MSB): reflects the degree of variation between groups. It represents the difference between the means of different groups.
		* Mean square within groups (MSW): reflects the degree of variation between individuals within a group, representing the random error or noise within the group.
### 1.5 Mann-Whitney U Test
### 1.6 Wilcoxon Signed-Rank Test
### 1.7 K-S Test
## 2 Transfer to p-value