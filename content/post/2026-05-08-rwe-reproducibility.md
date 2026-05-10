---
layout:     post
title:      "Reproducibility in Real-World Evidence: Lessons from the REPEAT Initiative"
subtitle:   ""
excerpt:    "A practitioner's take on the landmark study evaluating reproducibility of 150 RWE studies"
author:     "Degang Wang"
date:       2026-05-09
description: "Reflections on Wang et al. 2022 Nature Communications paper on RWE reproducibility, and what it means for analysts working with claims and EHR data every day."
image: "/img/header-methods.jpeg"
publishDate: 2026-05-10
tags:
    - RWE
    - Reproducibility
    - Epidemiology

categories: [ Methods, RWD ]
URL: "/2026/05/08/rwe-reproducibility/"
---

## The Paper

Wang SV, Kattinakere Sreedhara S, Schneeweiss S, & REPEAT Initiative. *Reproducibility of real-world evidence studies using clinical practice data to inform regulatory and coverage decisions.* Nature Communications. 2022;13:5126. [DOI: 10.1038/s41467-022-32310-3](https://doi.org/10.1038/s41467-022-32310-3)

<!--more-->

## Why This Paper Matters

Real-world evidence (RWE) has moved from a supplementary role to a central one in regulatory and coverage decisions. The FDA, EMA, and payers increasingly rely on studies conducted in administrative claims and EHR databases. But a fundamental question persists: **if another team picks up your paper and tries to recreate your analysis in the same data, will they get the same answer?**

The REPEAT (Reproducibility of Evidence from observational studies using Existing data from A single site Providing Analysis and Transparency) Initiative tackled this head-on by attempting to independently reproduce 150 published RWE studies and evaluating reporting completeness for 250.

## Key Findings

The results are both reassuring and sobering:

- **Strong overall correlation**: Pearson's r = 0.85 between original and reproduced effect sizes
- **Median relative magnitude**: HR_original / HR_reproduction = 1.0 [IQR: 0.9, 1.1]
- **82%** of reproductions had point estimates on the same side of the null
- But **16%** had a meaningful flip in statistical significance (p crossing 0.05)
- **21%** of study sizes differed by more than 2-fold

The majority of studies could be reasonably reproduced — but a non-trivial subset could not, and the reasons are instructive.

## Three Root Causes of Irreproducibility

The paper identifies three key drivers:

### 1. Incomplete Reporting of Design Parameters

The reproduction team had to make assumptions in 3–5 of 6 key parameter categories (index date, inclusion/exclusion, exposure, outcome, follow-up, covariates) for the median study. Only 3 out of 250 studies required zero assumptions.

This resonates with my daily experience. When evaluating an RWD source or designing a study, the devil is in operational details: How do you handle overlapping days supply? What care setting qualifies for the outcome? Which diagnosis position counts? These decisions profoundly shape results but are often buried or omitted.

### 2. Internally Inconsistent Information

Discrepancies between methods text, attrition tables, design diagrams, and appendices forced the reproduction team to guess which description reflected actual implementation. This is more common than we'd like to admit — protocol amendments and iterative analysis often leave artifacts in the final manuscript.

### 3. Data Version Drift

Even using the "same" database, retroactive updates to historical data created meaningful differences. One data source changed how death was captured across versions; another expanded Medicare Advantage coverage. The paper notes that data versioning is rarely reported — partly because no harmonized versioning system exists for research databases.

This is an underappreciated challenge. Anyone who has worked with claims data across annual refreshes knows that year-over-year "same" data is never quite the same.

## What This Means for Practitioners

Having spent years working with RWD — from source evaluation to study execution — here's how I interpret the implications:

### Transparency Is Not Optional

The paper shows that no single study parameter dominated reproducibility failures. It's the accumulation of small ambiguities that compounds into divergent results. The bar for reporting needs to rise to the level of **operational specificity**: not just "we used ICD-10 codes for the outcome" but which codes, in what position, in what care setting, with what lookback window.

### Design Diagrams Should Be Standard

Only 8% of sampled studies included a design diagram. This is a missed opportunity. A well-constructed temporal diagram communicates index date anchoring, washout periods, covariate assessment windows, and follow-up rules more clearly than paragraphs of text.

### Data Provenance Needs Attention

We routinely specify the data source and study period, but rarely document the data version, extraction date, or refresh cycle. As the paper demonstrates, this can matter — especially for outcomes like death that are sensitive to data linkage updates.

### Reproducibility ≠ Validity

A critically important nuance: the paper emphasizes that reproducibility is necessary but not sufficient for validity. A study can be perfectly reproducible yet suffer from confounding, misclassification, or selection bias. But if we can't even reproduce a result, we certainly can't evaluate its validity.

## Connecting to Broader Trends

The REPEAT findings align with the broader push for structured protocols and transparency in RWE:

- **ISPE/ISPOR guidelines** on reporting for database studies
- **FDA's RWE Framework** emphasizing fitness-for-purpose and transparency
- **HARPER** and other protocol templates that formalize design documentation
- **HARmonized Protocol Template (HARPER)** requiring pre-specification of algorithms

As someone who evaluates RWD sources and executes studies in claims and EHR data, I see this paper as a call to treat every study as if it will be independently reproduced — because increasingly, it might be.

## Takeaway

The glass is more than half full: most RWE studies are reasonably reproducible. But "reasonably" isn't good enough when the stakes include regulatory approvals and coverage decisions affecting millions of patients. The path forward is clear: greater operational transparency, standardized reporting of design parameters, and honest acknowledgment that the data itself is a moving target.

The REPEAT Initiative didn't just measure reproducibility — it gave the field a roadmap for improving it.
