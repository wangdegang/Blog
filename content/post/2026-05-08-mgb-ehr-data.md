---
layout:     post
title:      "Mass General Brigham EHR Data: A Real-World Data Resource for Research"
subtitle:   ""
excerpt:    "Overview of MGB's research data offerings for real-world evidence studies"
author:     "Degang Wang"
date:       2026-05-08
description: "An overview of Mass General Brigham's EHR-based research data, including its strengths, limitations, and use cases for real-world evidence generation."
image: "/img/header-data.jpeg"
publishDate: 2026-05-08
tags:
    - RWD
    - EHR
    
categories: [ RWD ]
URL: "/2026/05/08/mgb-ehr-data/"
---

## What Is Mass General Brigham?

Mass General Brigham (MGB), formerly Partners HealthCare, is one of the largest academic health systems in the United States. It includes Massachusetts General Hospital, Brigham and Women's Hospital, and several other hospitals and community health centers across New England. The system serves millions of patients and generates rich longitudinal electronic health record (EHR) data.

<!--more-->

## Research Data Offerings

### Research Patient Data Registry (RPDR)

MGB's primary research data tool is the **Research Patient Data Registry (RPDR)**, a centralized clinical data warehouse that aggregates EHR data across the health system. Key features include:

- **Patient population**: Millions of patients seen across MGB-affiliated institutions
- **Longitudinal depth**: Records spanning decades for long-tenure patients
- **Data domains**: Demographics, diagnoses (ICD codes), procedures (CPT), medications, lab results, vitals, radiology reports, pathology reports, and clinical notes
- **Unstructured data**: Access to free-text clinical notes enabling NLP-based research

### Biobank and Genomics

MGB hosts the **Mass General Brigham Biobank**, which links genomic data with EHR records for consented participants. This enables:

- Genotype-phenotype association studies
- Pharmacogenomics research
- Genetic risk prediction in diverse populations

### Specialty Registries

Individual departments maintain curated registries for specific conditions (e.g., oncology, cardiology, rheumatology) with deeper clinical detail than the general EHR.

## Strengths

| Strength | Detail |
|----------|--------|
| **Clinical depth** | Academic medical center data captures complex, specialty care not found in claims |
| **Longitudinal records** | Patients seen over many years within the system |
| **Unstructured data** | Clinical notes provide context beyond coded data |
| **Biobank linkage** | Genomic + phenotypic data for translational research |
| **Academic expertise** | Collaboration opportunities with clinical investigators |
| **Diverse specialties** | Comprehensive coverage of rare and complex conditions |

## Limitations

| Limitation | Detail |
|------------|--------|
| **Regional bias** | Primarily New England population; may not generalize nationally |
| **Fragmented care** | Patients seen outside MGB are missing data (labs, Rx, visits elsewhere) |
| **No claims linkage** | Lacks insurance claims data — incomplete medication fills, cost, and coverage info |
| **Selection bias** | Academic centers attract sicker, more complex patients |
| **Data access** | Requires IRB approval and collaboration agreements |
| **Coding variability** | Research-grade phenotyping often requires NLP or manual chart review |

## Typical Use Cases

- **Natural history studies** for rare diseases with specialist follow-up
- **Treatment effectiveness** research in complex patient populations
- **Biomarker discovery** leveraging linked genomic and clinical data
- **Phenotype validation** using clinical notes and structured data together
- **Safety surveillance** with longitudinal lab and vitals monitoring

## How It Compares to Other RWD Sources

| Feature | MGB (AMC EHR) | Claims (e.g., Optum, IQVIA) | Multi-site EHR (e.g., TriNetX) |
|---------|---------------|------------------------------|-------------------------------|
| Clinical depth | High | Low | Medium |
| Population size | Medium | Very large | Large |
| Generalizability | Low (regional) | High (national) | Medium |
| Longitudinal completeness | Medium | High (within payer) | Low–Medium |
| Unstructured data | Yes | No | Varies |
| Genomics linkage | Yes (Biobank) | No | Rare |
| Cost/access | Collaboration-based | Commercial license | Subscription |

## Key Takeaways

MGB EHR data is best suited for studies that need **clinical depth over breadth** — particularly rare diseases, complex conditions requiring specialist data, biomarker-linked analyses, and NLP-driven phenotyping. It is less ideal for studies requiring large, nationally representative populations or complete medication dispensing histories.

When designing an RWD study, consider whether your research question prioritizes **depth** (favoring AMC EHR like MGB) or **breadth** (favoring claims or multi-site networks), and plan accordingly.
