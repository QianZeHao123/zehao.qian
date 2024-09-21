---
title: 'Analyzing Multilevel Models in Clinical Research'
summary: A Comprehensive Evaluation of Stress-Coping Training Effectiveness with multilevel modeling
tags:
  - ML
  - Software
date: 2024-04-02
# external_link: https://github.com/QianZeHao123/ISHPC
---

# Part 1 Introduction

## 1.1 Background of Multisite Trials

### 1.1.1 Definition of Multisite Trials

Multisite trials are a type of clinical research study where the intervention being tested is administered across multiple sites or locations. These trials are particularly valuable in assessing the effectiveness of an intervention in a broader, more diverse population. By including a variety of settings, such as different hospitals, clinics, or communities, multisite trials can provide more generalizable results, ensuring that the findings are not specific to a single location or population [@YouthEndowmentFund2024].

### 1.1.2 Relevance for assessing the effectiveness of an intervention

1.  **Generalizability**: Multisite trials enhance the external validity of the study findings. By testing the intervention across various demographic and geographic settings, the results are more likely to be applicable to a wider population.

2.  **Variability and Robustness**: These trials capture the variability across different sites, which can include differences in implementation, participant characteristics, and contextual factors. This variability helps in assessing the robustness of the intervention's effectiveness.

3.  **Standardization vs. Adaptation**: Multisite trials can explore the balance between the standardization of the intervention (to ensure fidelity) and its adaptation to different settings (to ensure relevance). This balance is crucial for interventions that aim to be scaled up or replicated in diverse contexts.

4.  **Statistical Power**: Conducting a trial across multiple sites often allows for a larger sample size, which increases the statistical power of the study. This is particularly important for detecting small to moderate effects of interventions.

5.  **Complex Interventions**: Many interventions, especially in healthcare, are complex and multifaceted. Multisite trials can provide insights into how different components of the intervention perform across various settings.

6.  **Healthcare System Insights**: For interventions implemented in healthcare settings, multisite trials can offer valuable insights into how different healthcare systems or practices impact the effectiveness of the intervention.

### 1.1.3 Bias of Multisite Trials

Multisite trials enhance the relevance of findings but face challenges like selection bias, variability in implementation, and contextual influences, which can skew results. Mitigating these requires careful site selection, standardization across sites, and statistical techniques like multilevel modeling to ensure the trials' findings are both robust and widely applicable.

### 1.1.4 Pros and Cons of Multisite Trials

Multisite trials offer enhanced generalizability and statistical power due to their diverse and large participant pools, and can be more resource-efficient through shared infrastructure. However, they also face challenges such as logistical complexities, variability in intervention implementation, regulatory hurdles, potential site-specific biases, and data integration issues. Balancing these pros and cons requires careful planning, standardization of protocols, and sophisticated statistical methods to ensure the reliability and applicability of the findings across varied settings [@Mudaranthakam2021].

## 1.2 Intro to the MST Dataset

### 1.2.1 Read the Dataset

```R
# --------------------------------------------------------
## clear the environment var area
# rm(list = ls())
## clear all plots
# graphics.off()
## clear the console area
# cat("\014")
# --------------------------------------------------------
# install.packages("gridExtra")
# --------------------------------------------------------
require(lme4)
require(lmerTest)
require(ggplot2)
require(sjPlot)
```

Download the dataset "MST" only once from GitHub and save it to csv files.

```R
# ----Download the Data --------------------------------
# MST <-
#   read.csv(
#     "https://andygolightly.github.io/teaching/MATH43515/summative/andy.csv",
#     header = TRUE
#   )
# write.csv(MST, "./Data/MST.csv", row.names = FALSE)
# ------------------------------------------------------
MST = read.csv("./Data/MST.csv")
dim(MST)
head(MST)
## Show three line table MST with sjPlot::tab_df
# tab_df(MST[1:5, ])
```

The MST dataset encompasses data from a longitudinal multisite trial assessing a stress-coping training program's effectiveness for nurses in 20 hospitals' Accident and Emergency (A&E) departments. It comprises 200 entries across 9 columns, detailing anonymized nurse identifiers (ID), hospital IDs, treatment assignment (Trt) with 0 indicating control and 1 indicating receipt of the training program, nurse experience in years, gender (0 for male, 1 for female), and A&E department size (0 for small, 1 for large). The dataset tracks the program's impact on job-related stress through post-test stress scores (Responset1, Responset2, Responset3) measured at 1, 2, and 3 months post-training, respectively, on a scale from 0 (no stress) to 100 (maximum stress), providing a multifaceted view of the intervention's short-term effects on nurse stress levels across diverse hospital settings.

### 1.2.2 Identify the variables at each level

![Variables at each level](variables%20at%20each%20level.png)

**Considerations for "Size" Placement**: It is no longer true that other variables are identified as different hierarchies. However, it is worth discussing whether the Size is placed on the Nurse level or the Hospital level. Based on my observations of the Hospital and Size columns in the dataset, there is only one possible Size value for each hospital. For the study's focus on the intervention's impact on stress responses, placing Size at Level 3 is generally more suitable. This approach recognizes department size as a contextual factor at the hospital level that may influence the stress-coping intervention's effectiveness across different settings. It allows the study to explore how the broader environmental and organizational context, such as the scale of A&E departments, moderates the treatment outcomes, providing insights into adapting the intervention for varied hospital environments to enhance its efficacy.

### 1.2.3 Aims of Multilevel Modeling

The study aims to determine whether the experimental intervention, a stress-coping training program, significantly reduces job-related stress among nurses in A&E departments post-test. Additionally, it seeks to explore how the intervention's impact on stress levels changes over time, providing insights into the sustainability and temporal dynamics of the program's effectiveness.

## 1.3 Exploratory Data Analysis

### 1.3.1 Check missing values and imputation

```R
# Check if there are any missing values in the entire dataset
any_na <- any(is.na(MST))
print(paste("Are there any missing values in the dataset? ", any_na))
remove(any_na)
```

### 1.3.2 Summary Statistics

```R
# Summary statistics for continuous variables
summary(MST$Experience)
summary(MST$Responset1)
summary(MST$Responset2)
summary(MST$Responset3)
```

```R
# Load the necessary libraries
library(gridExtra)

# Frequency counts for categorical variables
# Bar chart for Treatment Groups
bar.Trt <- ggplot(data = MST, aes(factor(Trt))) +
  geom_bar(fill="lightblue") +
  labs(x="Treatment Group", y="Count", title="Treatment Group Distribution")

# Bar chart for Gender
bar.Gender <- ggplot(data = MST, aes(factor(Gender))) +
  geom_bar(fill="green") +
  labs(x="Gender", y="Count", title="Gender Distribution")

# Bar chart for Department Size
bar.Size <- ggplot(data = MST, aes(factor(Size))) +
  geom_bar(fill="pink") +
  labs(x="Department Size", y="Count", title="A&E Department Size Distribution")

# Bar chart for Hospital
bar.Hospital <- ggplot(data = MST, aes(factor(Hospital))) +
  geom_bar(fill="gray") +
  labs(x="Hospital", y="Count", title="Hospital Distribution")


grid.arrange(bar.Trt, bar.Gender, bar.Size, bar.Hospital, ncol = 2)

remove(bar.Trt, bar.Gender, bar.Size, bar.Hospital)
```

### 1.3.3 Distribution Analysis

```R
# List of variable names to plot
variables_to_plot <-
  c("Experience", "Responset1", "Responset2", "Responset3")

# Initialize lists to store the plots
hist_plots <- list()
box_plots <- list()

# Loop through each variable and create histograms and box plots
for (i in 1:length(variables_to_plot)) {
  var <- variables_to_plot[i]
  
  # Histogram with Density Plot
  hist_plots[[i]] <- ggplot(MST, aes_string(x = var)) +
    geom_histogram(
      aes(y = ..density..),
      binwidth = 1,
      color = "black",
      fill = "skyblue"
    ) +
    geom_density(alpha = .2, fill = "#FF6666") +
    ggtitle(paste("Distribution of", var)) +
    xlab(var) +
    ylab("Density")
  
  # Box Plot
  box_plots[[i]] <- ggplot(MST, aes_string(y = var)) +
    geom_boxplot(fill = "lightblue") +
    ggtitle(paste("Box Plot of", var)) +
    ylab(var)
}

# Arrange the histograms in a 4-graph plot
grid.arrange(grobs = hist_plots, ncol = 2)

# Arrange the box plots in a 4-graph plot
grid.arrange(grobs = box_plots, ncol = 2)

remove(hist_plots, box_plots, variables_to_plot, i, var)
```

### 1.3.4 Correlations

Calculate the correlations between every two variables ($X$ and $Y$) with the Pearson correlation coefficient:

$$
 r = \frac{\sum_{i=1}^{n} (X_i - \bar{X})(Y_i - \bar{Y})}{\sqrt{\sum_{i=1}^{n} (X_i - \bar{X})^2} \sqrt{\sum_{i=1}^{n} (Y_i - \bar{Y})^2}}
$$

```R
# Correlation between two varibales with GGpairs
library("GGally")
ggpairs(MST[, c("Experience", "Responset1",
                "Responset2", "Responset3")]) +
  theme_bw()
```

### 1.3.5 **Treatment and Control Group Comparison**

In Section 1.3.5-13.7, I focus on the Intervention. For easier making the graphs, I change the dataset into **long format**.

```R
# Reshape data to "long format" for easier plotting
library(tidyr)
MST_long <-
  pivot_longer(
    MST,
    cols = starts_with("Responset"),
    names_to = "TimeStamp",
    values_to = "StressScore"
  )

# Convert TimeStamp to a factor for ordered plotting
MST_long$TimeStamp <-
  factor(MST_long$TimeStamp,
         levels = c("Responset1", "Responset2", "Responset3"))
```

```R
# Create a bar plot comparing treatment and control groups over time
ggplot(MST_long, aes(x = TimeStamp, y = StressScore,
                     fill = factor(Trt))) +
  geom_bar(
    stat = "summary",
    fun = "mean",
    position = "dodge",
    width = 0.7
  ) +
  scale_fill_manual(
    values = c("#0F9999", "#9999FF"),
    labels = c("Control", "Treatment")
  ) +
  labs(title = "Comparison of Treatment and Control Groups Over Time",
       x = "TimeStamp",
       y = "Mean Stress Score",
       fill = "Group") +
  theme_minimal() +
  # Center the plot title
  theme(plot.title = element_text(hjust = 0.5))
```

### 1.3.6 Temporal Trends

```R
# Plotting temporal trends
ggplot(MST_long,
       aes(
         x = TimeStamp,
         y = StressScore,
         group = Trt,
         color = factor(Trt)
       )) +
  geom_line(stat = "summary", fun.y = "mean") +
  geom_point(stat = "summary", fun.y = "mean", size = 3) +
  scale_color_manual(
    values = c("#60F999", "#123456"),
    labels = c("Control", "Treatment")
  ) +
  labs(
    title = "Temporal Trends in Stress Scores",
    x = "TimeStamp",
    y = "Mean Stress Score",
    color = "Group"
  ) +
  theme_minimal() +
  # Center the plot title
  theme(plot.title = element_text(hjust = 0.5))
```

### 1.3.7 Categorical Variable Analysis

```R
# Scatterplot of Experience vs. StressScore, colored by Treatment Group
ggplot(MST_long, aes(x = Experience, y = StressScore, color = factor(Trt))) +
  geom_point(alpha = 0.6) +
  scale_color_manual(
    values = c("orange", "purple"),
    labels = c("Control", "Treatment")
  ) +
  # Optional: To separate plots by time points
  facet_wrap( ~ TimeStamp) +
  labs(
    title = "Experience vs. Stress Score by Treatment Group",
    x = "Experience (Years)",
    y = "Stress Score",
    color = "Group"
  ) +
  theme_minimal() +
  # Center the plot title
  theme(plot.title = element_text(hjust = 0.5))
```

```R
# Create box plots
boxplot_stress <-
  ggplot(MST_long, aes(x = TimeStamp, y = StressScore, fill = factor(Trt))) +
  geom_boxplot() +
  scale_fill_manual(values = c("skyblue", "salmon"),
                    labels = c("Control", "Treatment")) +
  labs(title = "Stress Scores by Timepoint and Treatment Group",
       x = "TimeStamp",
       y = "Stress Score",
       fill = "Group") +
  theme_minimal()

# Display the plot
print(boxplot_stress)
remove(boxplot_stress)
```

## 1.4 Convert the Numeric Data into Factors

Factors explicitly inform the statistical model that the data is categorical, not continuous, such as Hospital, ID, Trt, Gender (male or female), Size (small or large). Their original data structure is integer, I convert them into factor with tidyverse below (in addition, TimeStamp is a string, but our study focus on the treatment impact evolving over time, so I change them into integer as 1, 2 and 3):

```R
library(dplyr)

MST_long = MST_long %>%
  mutate(
    Trt.f = as.factor(Trt),
    Gender.f = as.factor(Gender),
    Hospital.f = as.factor(Hospital),
    ID.f = as.factor(ID),
    Time = as.numeric(TimeStamp),
    Size.f = as.factor(Size)
  )

head(MST_long)
```

## 1.5 The Distribution of StressScore

```R
MST_long %>%
  ggplot(aes(sample = StressScore)) +
  geom_qq() +
  geom_qq_line() +
  labs(title = "Q-Q Plot of StressScore",
       x = "Quantile",
       y = "Stress Score"
       )
```

```R
# Shapiro-Wilk Test (Statistical Test)
shapiro.test(MST_long$StressScore)
```

The Shapiro-Wilk test is a common test for normality. P-value is less than 0.05, which indicates that we can think of the distribution of the StressScore as a Gaussian distribution.

# Part 2 Methods

## 2.1 Multilevel models for MST Dataset

### 2.1.1 Multilevel Models Overview

Multilevel models, also known as hierarchical linear models or mixed-effects models, are a class of statistical models designed to analyze data with a nested or hierarchical structure. These models are particularly relevant for datasets where observations are grouped at more than one level, such as our case, repeated stress measurements within nurses who are, in turn, nested within hospitals.

### 2.1.2 Relevant for the modelling of this data set

![Hierarchical Data Structure](hierarchical%20structure.png)

Multilevel models (MLMs) are crucial for analyzing the MST dataset's hierarchical structure, effectively addressing the correlations within grouped data. By acknowledging the nesting of measurements within individuals and hospitals, MLMs prevent the underestimation of standard errors that could lead to incorrect significance claims. They meticulously partition variance to reveal the sources influencing outcomes, integrating both fixed effects, such as the overall intervention impact, and random effects that capture variability at individual and group levels. This approach enables a comprehensive assessment of interventions in varied contexts. Overlooking the dataset's hierarchical nature risks missing key insights and making inaccurate conclusions, particularly regarding how group characteristics like hospital policies may affect results. Therefore, MLMs are indispensable for ensuring the robustness and relevance of findings in nested data analyses.

## 2.2 Generalized Linear Mixed Model for MLM (Model Assumption)

According to fit the model, I choose Generalized Linear Mixed Model (GLMM), the reasons are shown below:

-   Limitation of Traditional Linear Model: Unlike traditional linear models that assume normally distributed residuals, GLMM can handle various types of distributions for the response variable (Poisson for count data, etc.). **And in section 1.3.5 and 1.5, it shows that the StressScore we want to predict obeys Gaussian Distribution.**

-   Hierarchical Structure and Longitudinal Data: A nested structure with multiple levels (Repeated measures within nurses, nurses within hospitals)

-   Allows for Fixed and Random Effects: Fixed effects (e.g., treatment or intervention, time points) capture population-level effects that are consistent across all observations, while random effects account for variations at the group level (e.g., differences among nurses or hospitals) that might affect the response variable.

## 2.3 Decompose variance

Decomposing variance in a multilevel model involves quantifying how much of the total variability in the outcome is attributable to differences at each level of the hierarchy. One key measure used in this context is the Intra-class Correlation Coefficient (ICC), which provides an estimate of the proportion of the total variance that is due to the grouping structure.

High ICC values highlight the importance of multilevel modeling. Testing for significant variance components involves likelihood ratio tests to compare models with and against random effects at each level, guiding the inclusion of relevant variance components.

**Steps for Decompose variance**

1.  Using the MST_long (long format) to fit a **compared** empty GLMM:

    -   The family type of GLMM is **Gaussian** (mentioned in section 1.3.3 1.5 and 2.2).

    -   The compared model need the following necessary elements: interaction between time and Trt (Level 1), Nurse (Level 2) and Hospital (Level 3).

2.  Calculate the VPC and ICC

    $$
    VPC_{ID}=\frac{\sigma^2_{Hospital}}{\sigma^2_{Hospital}+\sigma^2_{Hospital:ID}+\sigma^2_e}
    $$

    $$
    VPC_{Hospital:ID}=\frac{\sigma^2_{Hospital:ID}}{\sigma^2_{Hospital}+\sigma^2_{Hospital:ID}+\sigma^2_e}
    $$

    $$
    ICC = \frac{\sigma^2_{between}}{\sigma^2_{between} + \sigma^2_{within}}
    $$

## 2.4 Methodological Approach

Here, we can use **half-way in** method to build the Model (Build a compared simple model and after that add fixed effects, interaction and random effects step by step). When add a new random effect, using **ranova()** to validate if it deserves (p-value). In addtion, using **anova()** (p-value as well) between models to compare which is better. Model efficacy is then evaluated using likelihood ratio tests alongside AIC/BIC criteria, ensuring the inclusion of meaningful variance components and optimizing model fit.

# Part 3 Analysis

Modeling Questions:

-   The significance of Intervention and its effect.

-   The different effect of Intervention between Hospitals.

-   The effect of Intervention over time.

-   Other covariates in the model.

## 3.1 Determine the level of Model

### 3.1.1 Build an empty GLMM with Random Effects

model 0: only care about the interaction between Trt\*Time and Multi-Level (a change of the intervention effect over time) (**Trt.f \* Time = Trt.f + Time + Trt.f:Time**)

```R
## Empty model with longitudinal data
Model.0 = glmer(
  StressScore ~ Trt.f * Time + (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
  family = gaussian(link = "identity")
)
```

The warning above is said when using **glmer(family=gaussian)** is same to **lmer()**. So the code bellowing I will use **lmer()** instead.

### 3.1.2 Calculate the VPC

```R
REsummary <- as.data.frame(VarCorr(Model.0))
REsummary
```

```R
# Residual variance
sig <- REsummary$vcov[3]
# Variance for Hospital
sigv <- REsummary$vcov[2]
# Variance for ID
sigu <- REsummary$vcov[1]
# total variance
totalvar <- sum(REsummary$vcov)
vpc.Hospital <- sigv / totalvar
vpc.ID <- sigu / totalvar
```

### 3.1.3 Calculate the ICC

```R
icc.Hospital <- sigv / totalvar
icc.ID <- (sigu + sigv) / totalvar
```

```R
icc.Hospital
```

```R
icc.ID
```

We can see high ICCs in Hospital and Nurse level. That means this two levels explain a substantial part of the variance in the outcome measure. So we make our model with 3-levels. And we can say there exists heterogeneity of the intervention effect between hospitals.

## 3.2 Only Time and Trt in Model (without Interaction)

```R
Model.1 = lmer(
  StressScore ~ Trt.f + Time + (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

```R
# Compare Model.0 and Model.1 (with without Interaction between Time and Trt)
anova(Model.0, Model.1)
```

This indicate it exist a change of the intervention effect over time.

## 3.3 Only Time and Trt's Interaction

```R
Model.2 = lmer(
  StressScore ~ Trt.f:Time + (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

```R
# Compare Model.0 and Model.1 (with Time)
anova(Model.0, Model.2)
```

The intervention effect is significant!

## 3.4 Add other covariates as Fixed Effect

```R
# Model.3 add Experience
Model.3 = lmer(
  StressScore ~ Trt.f*Time + Experience +
    (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

```R
# Compare Model.0 and Model.3
anova(Model.0, Model.3)
```

I put the other model and comparison in the **Appendix 1**, after test all the possible combination, the best model till now is **Model.5**:

```R
Model.5 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f + Size.f +
    (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

## 3.5 Add Random Effects to the Model

```R
Model.6 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f + Size.f +
    (1 | Hospital.f:ID.f) + (Trt.f | Hospital.f),
  data = MST_long,
)
```

```R
ranova(Model.6)
```

```R
anova(Model.5, Model.6)
```

This is also the example of model building. I put the other try in **Appendix 2**.

From Model.7 to Model.9, I cannot find a Model better than **Model.5**.

## 3.6 Final Model Router

![Model Roadmap](Final%20Model%20Router.png)

# Part 4 Discussion of results

## 4.1 Conclusions of Model 0-9

Like what section 3.6 shows, I summarize the Model 0-9 here:

-   Model 0: Only Trt.f\*Time in the fix effect, calculating the VPC and ICC. **Important Conclusion: significant difference in intervention effect between hospitals.**

-   Model 1: **Important Conclusion: Evidence of a change of the intervention effect over time.**

-   Model 2: **Important Conclusion: intervention effect significant.**

-   Model 3, 4, 5: Add Size.f, Experience, Gender.f. **Important Conclusion: These covariates have importance in explaining model.**

-   Model 6,7,8,9: We cannot find any Random Slope in the Model.

## 4.2 Summary of the Best Model (Model.5)

### 4.2.1 Mathematical Model

$$
\begin{aligned}
Y_{ijt} = & \ \beta_0 \\
& + \beta_1 \cdot \text{Trt.f}_{ijt} \\
& + \beta_2 \cdot \text{Time}_{ijt} \\
& + \beta_3 \cdot (\text{Trt.f} \times \text{Time})_{ijt} \\
& + \beta_4 \cdot \text{Experience}_{ij} \\
& + \beta_5 \cdot \text{Gender.f}_{ij} \\
& + \beta_6 \cdot \text{Size.f}_{j} \\
& + u_{j0} \\
& + u_{ij} \\
& + \epsilon_{ijt}
\end{aligned}
$$

-   $\text{Trt.f} \times \text{Time}$ represents the interaction term, which models the effect of the Treatment over Time.

-   The random effects $u_{j0}$ and $u_{ij}$ are assumed to be normally distributed with mean zero and their respective variances.

-   The residual errors $\epsilon_{ijt}$ are assumed to be independent and identically distributed normal random variables with mean zero and variance $\sigma^2$.

### 4.2.2 Model Summary

```R
summary(Model.5)
```

### 4.2.3 Model.5 Residual Diagnostics

Our model assumes normal distributions for both the random effects and the residual errors.

```R
# Model of Residuals
ggplot(data.frame(Fitted = fitted(Model.5),
                           Residuals = resid(Model.5)),
       aes(x = Fitted, y = Residuals)) +
  # Plot points with some transparency
  geom_point(alpha = 0.5) +
  geom_hline(yintercept = 0,
             linetype = "dashed",
             color = "red") +  # Add a horizontal line at 0
  labs(title = "Residual Plot for Model.5",
       x = "Fitted Values", y = "Residuals")
```

The Residuals seems satisfied the normal distribution.

$$
\epsilon_{ijt} \sim N(0, \sigma^2_{e}) 
$$

```R
ggplot(data.frame(Residuals = resid(Model.5)),
       aes(sample = Residuals)) +
  # Creates the Q-Q plot
  stat_qq() +
  # Adds the theoretical Q-Q line
  stat_qq_line(color = "red") +
  labs(title = "Q-Q Plot of Residuals for Model.5",
       x = "Theoretical Quantiles", y = "Sample Quantiles")
```

**Q-Q plot of Random Effects:**

```R
# # Extract random effects
ran_ef <- ranef(Model.5)$Hospital.f
ran_ef_id <- ranef(Model.5)$`Hospital.f:ID.f`
# Prepare data frames
ran_ef_df <- data.frame(RandomEffects = ran_ef$`(Intercept)`)
ran_ef_id_df <- data.frame(RandomEffects = ran_ef_id$`(Intercept)`)
```

Hospital Level's Residual Diagnostics (whether $u_{j0}$ satisfied Gaussian Distribution or not)

$$
u_{j0} \sim N(0, \sigma^2_{Hospital})
$$

```R
ggplot(ran_ef_df, aes(sample = RandomEffects)) +
  stat_qq() +
  stat_qq_line(color = "red") +
  labs(title = "Q-Q Plot of Random Effects (Hospital Level)",
       x = "Theoretical Quantiles", y = "Sample Quantiles")
```

ID within Hospital Level Diagnostics (whether $u_{ij}$ satisfied Gaussian Distribution or not)

$$
u_{ij} \sim N(0,\sigma^2_{Hospital:ID})
$$

```R
ggplot(ran_ef_id_df, aes(sample = RandomEffects)) +
  stat_qq() +
  stat_qq_line(color = "red") +
  labs(title = "Q-Q Plot of Random Effects (ID within Hospital Level)",
       x = "Theoretical Quantiles", y = "Sample Quantiles")
```

Confidence intervals for the fixed effects

```R
confint(Model.5)
```

This represents the effect of the treatment (with the first level of `Trt.f` possibly being the reference category, and `Trt.f1` representing another category). The confidence interval (-6.088 to -4.385) suggests that the treatment is associated with a decrease in StressScore, and since zero is not within this interval, the effect is likely significant.

## 4.3 Using Model to predict

```R
MST_long$Prediction = predict(Model.5)
```

```R
ggplot(MST_long, aes(x = TimeStamp, y = Prediction, fill = factor(Trt))) +
  geom_boxplot() +
  scale_fill_manual(values = c("skyblue", "salmon"),
                    labels = c("Control", "Treatment")) +
  labs(title = "Predicted Stress Scores by Timepoint and Treatment Group",
       x = "TimeStamp",
       y = "Predicted Stress Score",
       fill = "Group") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 0, hjust = 1))
```

The boxplot using predicted value shows the same result as what I make in Section 1.3.7. The conclusion is: with the time getting longer, the StressScore gets bigger (Both Trt and Control Group). The Trt group's Stress Score is lower than control group's.

## 4.4 Limitations

1.  Random Intercept Only: Our model includes random intercepts but not random slopes for all predictors. This assumes that the relationship between the predictors and the outcome is consistent across groups, which might not be the case.
2.  Simplicity of Interaction: The model includes an interaction between treatment and time, which assumes a simple form of interaction. And we didn't try other possible interaction between covariates.

# Appendix

## Appendix 1: Add/Del Covariates in the model

```R
# Model.4 add Gender.f
Model.4 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f +
    (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

```R
# Compare Model.4 and Model.3
anova(Model.4, Model.3)
```

```R
# Model.5 add Size.f
Model.5 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f + Size.f +
    (1 | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

```R
# Compare Model.5 and Model.4
anova(Model.5, Model.4)
```

## Appendix 2: Add Random Effects to the Model

```R
Model.7 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f + Size.f +
    (1 | Hospital.f:ID.f) + (Trt.f*Time | Hospital.f),
  data = MST_long,
)
```

```R
ranova(Model.7)
```

```R
anova(Model.5, Model.7)
```

```R
Model.8 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f + Size.f +
    (1 | Hospital.f:ID.f) + (Size.f | Hospital.f),
  data = MST_long,
)
```

```R
ranova(Model.8)
```

```R
anova(Model.5, Model.8)
```

```R
Model.9 = lmer(
  StressScore ~ Trt.f*Time + Experience + Gender.f + Size.f +
    (Gender.f | Hospital.f:ID.f) + (1 | Hospital.f),
  data = MST_long,
)
```

```R
ranova(Model.9)
```

```R
anova(Model.5, Model.9)
```