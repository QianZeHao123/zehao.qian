---
title: 'Mechanization of Labor: From LLM to Work with AI'
summary: Full Stack Research Design on LLM topic
tags:
  - Social
date: 2023-12-21
# external_link: https://github.com/QianZeHao123/ISHPC
---


# Mechanization of Labor: From LLM to Work with AI

## Contents
1. [Introduction to Research Question](#introduction-to-research-question)
    - [Research Background](#research-background)
    - [Research Question and Hypothesis](#research-question-and-hypothesis)
    - [Research Significance](#research-significance)
2. [Data Collection and Processing](#data-collection-and-processing)
    - [Data Requirements](#data-requirements)
    - [Data Acquisition Methodology](#data-acquisition-methodology)
    - [Data Preprocessing](#data-preprocessing)
        - [Missing Value Handling](#missing-value-handling)
        - [Qualitative Data Processing](#qualitative-data-processing)
        - [Integration and Segmentation of Data Set](#integration-and-segmentation-of-data-set)
        - [Noise and Bias](#noise-and-bias)
3. [Research Methodology and Techniques](#research-methodology-and-techniques)
    - [Classical Mathematical and Statistical Techniques](#classical-mathematical-and-statistical-techniques)
    - [Causal Inference Methods](#causal-inference-methods)
        - [Difference-in-Differences (DiD)](#difference-in-differences-did)
        - [Synthetic Control Method](#synthetic-control-method)
        - [Matching](#matching)
4. [Future Study](#future-study)
    - [Research Target](#research-target)
    - [Limitations and Bias](#limitations-and-bias)
    - [Measurement Issues](#measurement-issues)
        - [Technological Propensity of Industrial Companies Towards LLMs](#technological-propensity-of-industrial-companies-towards-llms)
        - [Representativeness of the Data](#representativeness-of-the-data)
5. [References](#references)

## Introduction to Research Question

### Research Background
With the rapid development of Large Language Models (LLMs), often compared to steam engines of this era, technologies like ChatGPT are revolutionizing productivity across industries [1]. These advancements are enhancing efficiency but also fostering job insecurity among intellectual workers, including clerks, programmers, and data scientists, as their roles may be replaced by AI.

Most LLM news highlights its office efficiency improvements and hallucinations [3][4], raising questions about their applications outside white-collar jobs. In contrast, LLMs appear to have less impact on engineers and technicians, though some, like Oluwatosin Ogundare, apply LLMs to problem decomposition and geomechanical modeling [6].

LLMs are pivotal in refining manufacturing processes, improving quality, and providing customer services. As technology evolves, it enables smarter operations and adapts to changing market demands. Despite these advancements, concerns arise around the lesser visibility of blue-collar workers in discussions about digital transformation.

### Research Question and Hypothesis
A report from OpenAI explored the potential impact of LLMs and other generative AIs on the labor market, finding that around 80% of the U.S. workforce is impacted by AI [1].

Our core hypothesis is that the impact of LLMs on labor demand varies across different industries. The influence is particularly pronounced in sectors heavily reliant on data-driven and technologically supported decision-making processes.

### Research Significance
This study aims to explore the impact of LLMs on labor, determining if LLMs help improve productivity and profitability or lead to layoffs and wage cuts. We will assess their impact on labor demand across data-driven industries.

The findings will guide governments and institutions in developing worker training and adaptation programs. Companies can leverage this research for employee training, improving productivity while maximizing profitability.

## Data Collection and Processing

### Data Requirements
We need data at both the individual and company levels:

1. **Individual Worker Data**:
   - **Skillset Changes**: Evolution of skill requirements for jobs after LLM implementation.
   - **Employment Trends**: Statistical data on employment rates, job displacement, and creation.
   - **Training and Development**: Information on training programs and their effectiveness.
   - **Worker Attitudes and Perceptions**: Survey data on worker attitudes towards LLMs.

2. **Company Data**:
   - **Operational Flow Changes**: Data on productivity changes due to LLMs.
   - **HR Policies**: Changes in HR strategies in response to LLMs.
   - **Investment in AI and Training**: Financial data on AI and training investments.
   - **Business Performance Metrics**: KPIs like revenue growth post-LLM integration.

### Data Acquisition Methodology
We plan to use APIs from online platforms like LinkedIn and Glassdoor. For platforms without APIs, we'll scrape relevant data with Python scripts [12].

**Data Sources**:

| **Category**             | **Source**              | **Description**                                                           |
|--------------------------|-------------------------|---------------------------------------------------------------------------|
| Online Job Platforms     | LinkedIn                | Job postings, skills, API for job descriptions, company info             |
| Online Job Platforms     | Glassdoor               | Company reviews, job listings, salary reports                            |
| Government Data          | Bureau of Labor Stats   | Employment trends, salaries                                              |
| Market Research          | Gartner, Deloitte       | Industry trends and tech impact reports                                  |
| Social Media and News    | Twitter, Reddit         | Public sentiment on job market changes                                   |
| APIs for Economic Data   | World Bank, Quandl      | Global economic data                                                     |
| Custom Web Scraping      | BeautifulSoup/Scrapy    | Data extraction from various sources                                     |

### Data Preprocessing

#### Missing Value Handling
Techniques for handling missing values:
1. **Deletion**: Remove rows with missing values.
2. **Simple Imputation**: Fill in missing values using mean, median, or mode.
3. **Multiple Imputation**: The R package "mice" simulates multiple values for missing data.
4. **Advanced Techniques**: Machine learning models predict missing values.

#### Qualitative Data Processing
Tools like NVivo and ATLAS.ti help categorize and analyze qualitative data [13]. Patterns will be identified and interpreted in the context of LLMs' influence.

#### Integration and Segmentation of Data Set
- **Integration**: Consolidate data from diverse sources using SQL or integration platforms.
- **Segmentation**: Segment by criteria like industry, company size, and region.

#### Noise and Bias
Noise and bias management strategies include data cleaning, statistical techniques, diverse data sources, blind analysis, and continuous updates.

## Research Methodology and Techniques

### Classical Mathematical and Statistical Techniques
Key analytical tools for labor market analysis:

| **Analysis Type**        | **Objective**           | **Applied Techniques/Models**                                          |
|--------------------------|-------------------------|-------------------------------------------------------------------------|
| Descriptive Statistics   | Understand trends      | Mean, median, mode, standard deviation                                 |
| Regression Analysis      | Explore relationships  | Linear, logistic, multivariate regression                              |
| Time Series Analysis     | Examine trends over time| ARIMA models                                                           |
| Predictive Modeling      | Forecast trends        | Random Forest, Support Vector Machines                                 |
| Sentiment Analysis       | Analyze qualitative data| Natural Language Processing (NLP) techniques                           |
| Cluster Analysis         | Identify patterns      | K-means, hierarchical clustering                                       |
| Factor Analysis          | Market drivers         | PCA, Exploratory Factor Analysis                                       |
| Network Analysis         | Examine interconnections| Graph theory, Social network analysis                                  |
| Data Visualization       | Communicate findings   | Tableau, Power BI, Python (Matplotlib, Seaborn), R (ggplot2)           |

### Causal Inference Methods

#### Difference-in-Differences (DiD)
Compare labor market changes between a group exposed to LLMs and a similar group not exposed.

**Formula**:  
$$\Delta Y = (Y_{Post, Treatment} - Y_{Pre, Treatment}) - (Y_{Post, Control} - Y_{Pre, Control})$$

#### Synthetic Control Method
Create a synthetic control group as a weighted combination approximating the treatment group before LLMs.

#### Matching
Pair each treated unit with similar control units and compare outcomes.

**Formula**:  
$$e(X) = P(Treatment = 1|X)$$  
$$ATT = \frac{1}{N_{t}} \sum_{i \in Treated}(Y_{i} - Y_{matched(i)})$$

## Future Study

### Research Target
The study should explore LLMs' impact on different industries, guiding governments and enterprises in staff training. Develop optimization models for employee training based on collected data.

### Limitations and Bias
Challenges include transparency of corporate data and compliance/ethical issues with API and web scraping.

### Measurement Issues

#### Technological Propensity of Industrial Companies Towards LLMs
Companies may adopt LLMs to cut costs due to declining labor needs rather than the other way around.

#### Representativeness of the Data
Difficulties include passive turnover due to labor costs/tech adjustments and active turnover due to worker preferences.

## References
1. Tyna Eloundou, Sam Manning, Pamela Mishkin, and Daniel Rock. "GPTs are GPTs: An Early Look at the Labor Market Impact Potential of Large Language Models," 2023.
2. Tarry Singh. "The Impact of Large Language Multi-Modal Models on the Future of Job Market," 2023.
3. Lei Huang, et al. "A Survey on Hallucination in Large Language Models," 2023.
4. Hongbin Ye, et al. "Cognitive Mirage: A Review of Hallucinations in Large Language Models," 2023.
5. Ziwei Ji, et al. "Survey of Hallucination in Natural Language Generation," ACM Computing Surveys, 2023.
6. Oluwatosin Ogundare, et al. "Industrial Engineering with Large Language Models," 2023.
7. Fangkai Yang, et al. "Empower Large Language Model to Perform Better on Industrial Domain-Specific Question Answering," 2023.
8. Schneider Electric Global. "How Large Language Models Can Boost Industrial Automation," 2023.
9. Beckhoff United Kingdom. "TwinCAT Projects with AI-Assisted Engineering," 2023.
10. Fabian Bause and Jannis Doppmeier. "Fast and Efficient PLC Code Generation with Artificial Intelligence," 2023.
11. Harrison Schell. "Which Jobs Will Be Most Impacted by ChatGPT?," 2023.
12. Jürgen Schedlbauer, et al. "Medical Informatics Labor Market Analysis Using Web Crawling," International Journal of Medical Informatics, 2021.
13. JHU Data Services. "Qualitative Data Analysis Software," 2023.
