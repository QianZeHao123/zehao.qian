---
# An instance of the Experience widget.
# Documentation: https://docs.hugoblox.com/page-builder/
widget: experience

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 20

title: Experience
subtitle:

# Date format for experience
#   Refer to https://docs.hugoblox.com/customization/#date-format
date_format: Jan 2006

# Experiences.
#   Add/remove as many `experience` items below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
#   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.

experience:
  - title: Data Scientist
    company: Siemens Ltd,. China
    company_url: 'https://www.siemens.com/cn/zh.html'
    company_logo: org-siemens
    location: Beijing
    date_start: '2025-08-04'
    description: |2-
      - Team: DI IT AIDA R APAC
      - Work Email: ze-hao.qian@siemens.com
  - title: Research Assistant
    company: Department of Information Systems, City University of Hong Kong
    company_url: 'https://www.cb.cityu.edu.hk/is/'
    company_logo: org-cityuhk
    location: Hong Kong
    date_start: '2025-02-10'
    date_end: '2025-07-01'
    description: |2-
      Mainly focus on:
      - Traditional Machine Learning, Deep Learning and Reinforcement Learning
        - RNN Algorithm: RNN, GRU and LSTM
        - Attention machenism based RNN
        - LightGBM related Algorithm
      - Distributed Machine Learning with Ray
      - Hyper Parameter Tuning with Optuna

      Projects:
      - [Deep Reinforcement Learning-Based Optimization of Debt Collection](https://github.com/QianZeHao123/LoanRL)
      - [Car MRO Prediction with LightGBM and LSTM.](https://github.com/QianZeHao123/MroPred)
      - [CityUHK Ride Hailing Project](https://github.com/QianZeHao123/RideHail)
  - title: Data Analyst Intern
    company: Siemens Ltd,. China
    company_url: 'https://www.siemens.com/cn/zh.html'
    company_logo: org-siemens
    location: Beijing
    date_start: '2024-10-18'
    date_end: '2025-01-17'
    description: |2-
        Team: 
        * RC-CN DI FIN ADBA, Advanced Data and Business Analysis

        Responsibilities include:
        * Smart ChatBot with LLM & LangChain
        * Business Algorithm Development

  - title: Teaching Assistant (Engineering Optimization)
    company: School of Management Engineering, Zhengzhou University
    company_url: 'https://ieyjzhou.github.io/teaching/2023-Engineering-Optimization'
    company_logo: org-zzu
    location: Zhengzhou
    date_start: '2023-02-21'
    date_end: '2023-06-30'
    description: |2-
        Responsibilities include:
        
        * $\LaTeX$ lecture notes and slides making
        * Research on Optimization Algos

  - title: Founder of FreeLeek
    company: FreeLeek Foundation
    company_url: ''
    company_logo: org-freeleek
    location: Tianjin, China
    date_start: '2020-10-01'
    date_end: ''
    description: |2-
      Build some useful Financial Quant Tools:
      * [Leek Box](https://github.com/QianZeHao123/LeekBox)
      * [FreeLeek](https://gitee.com/qian_zehao/free-leek)
  
  - title: Co-founder and Team Leader of OpenIE
    company: Open Source Industrial Engineering Foundation
    company_url: 'https://github.com/Open-Source-Intelligent-Engineering'
    company_logo: org-OpenIE
    location: Henan, China
    date_start: '2021-04-28'
    date_end: ''
    # description: |2-
    #   Build some useful Financial Quant Tools:
    #   * [Leek Box](https://github.com/QianZeHao123/LeekBox)
    #   * [FreeLeek](https://gitee.com/qian_zehao/free-leek)
    description: |2-
      [OpenIE](https://github.com/Open-Source-Intelligent-Engineering) aims to use both advanced computer technology and traditional engineering knowledge such as engineering control theory, optimization, electronics, mechanic and so on to make industry more efficiant and reliable.


design:
  columns: '1'
---
