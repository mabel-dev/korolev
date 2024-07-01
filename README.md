# Korolev - Moonshot Data Platform

**Committing ourselves to achieving the goal, before this year is out, of building a high-performance, low-cost petabyte-scale data platform.**

## Introduction

This document outlines the vision, goals, and context for the security data platform. It aims to provide a comprehensive understanding of the platform's objectives, the current data landscape, and the strategic approach we will take to build a world-class data platform.

## Vision and Goals

The goal, in it's simplest terms, is to build a data platform to perform acquisition, persistence, processing, and publishing. This system will be a combination of bought and built components, with an emphasis on building a world-class engineering capability to support a world-class, data-led, security function.

World-class engineering requires world-class problems to organize and measure the best of our skills. We are currently behind where we need to be to achieve these goals, but we do not intend to stay behind and in this year, we shall build foundations to meet our goals and aspirations.

For the division to meet its objective of helping the organization to go faster safely, it not only needs supporting tools but also needs to grok how the business delivers and creates value. As such, it is imperative the division masters software engineering and shares in this experience along with the business it serves.

We chose to be engineering-first, not because it is easy, but because it is hard.

## Context

The current data landscape consists of disparate systems; providers, processors, and consumers are envisioned, maintained, and executed without due consideration of the other components within the ecosystem. Aligning the efforts and expertise from these various systems to achieve unified goals will lead to significantly better outcomes for the owners, engineers, and consumers of these systems.

Each of these existing systems has fundamental flaws, with much of these systems built rather than designed, or designed with overly restrictive perspectives or motivations. The limitations and flaws in existing systems present a rich opportunity to build a superior, unified platform - built leveraging the lessons hard learnt and earnt from these existing systems.

### Organizational Transformation

The organization is currently undergoing a transformation informed by software engineering practices; this, however, is from the lens of products. Some of the challenges of this transformation are likely to be eased with the support of a strong engineering capability and culture.

### Build-over-Buy Approach

A build-over-buy approach gives us a distinct advantage; it decouples our capabilities from commercial vendors and enables us to iterate and improve at the pace we can move at, limited only by our imagination and ingenuity. This approach allows us to keep up with the dynamic and envolvinf needs of the business rather than following where vendors see a return on investment.

## Design Goals

Designing an optimal data platform with the following goals:

- **Encourage Sharing**: Maximize data value through easy analysis and sharing.
- **Engineer Satisfaction**: Enhance engineer motivation in order to improve customer outcomes.
- **Simplicity and Speed**: Develop simple systems for rapid value delivery.
- **Cost Efficiency**: Maintain low-cost core systems, adding expense only where they provide value, emphasizing modular design.
- **Reliable**: Reliable performance and behaviour, handling errors and failures.
- **Support Automations**: All components are described in code, all functions are exposed as APIs.
- **User Abstraction**: Users shouldn't be forced to change behaviours based on internal implementations.

## Scope and Purpose

### Purpose of this Document

The purpose of this document is to describe how the opportunity to create a unified set of tools and common interfaces for the acquision, processing and publishing of data could be realized.

It also serves as a sales pitch and motivation to embed a software engineering culture and approach into the division.

### Scope

The system will be used as the primary data ecosystem for secutity, this primarily includes the storage and processing of data, and methods to expose this data for summarization, interaction, and building data products.

This does not describe to any significant degree the data products which may be built on this platform - this is intentional, as the data platform should not overfit to current fads in data consumption and usage in order to meet evolving business needs and to provide reasonably flexibility for future enhancements in this space.

## Problem Space

Key constraints and factors influencing technical direction:

- Data Engineering is a relatively immature industry, with high degrees of fragmentation.
- Support petabyte-scale datasets, with terabyte-scale daily loads.
- Ensure reliable and durable storage.
- Maintain immutable data, potentially used in legal procedures.
- Detect and gracefully recover from job failures.
- Optimize storage and compute costs.
- Ensure transparency, traceability, and auditability of all actions.
- Mix of scheduled, event-based, and ad hoc data processing.
- Majority of data processing will be repeatable.
- Support real-time access to data via dashboards (including PowerBI), and analyst workbooks.

## Assumptions

For the purposes of this document, it is reasonably assumed that:

- There is no need for dataset indexing (e.g., BigQuery, [Snowflake](https://www.youtube.com/watch?v=CPWn1SZUZqE)).
- Target Python 3.10 or later for development.
- Compute will primarily be containers within a Kubernetes-like environment.
- Multiple storage platforms will be used, balancing performance and cost.
- No access to running workers; only data outputs and logs are accessible.
- Manage code on GitHub, leveraging features like locked branches and Actions.
- Building a simple version in-house is faster and cost-effective over time than adopting an existing, opinionated service.

## Constraints

It is believed these are immutable things:

- BigQuery must be used as the store for data exposed to dashboards.

## Principles

### Warehouse

- **Scalability**: Design for growth, ensuring the system can handle increasing volumes of data without significant performance degradation or cost escalation.
- **Flexibility**: Support diverse data types and formats, enabling a wide range of analytical tasks.
- **Efficiency**: Optimize storage and retrieval processes to minimize costs and improve performance.

### Pipelines

- **Resilience**: Ensure pipelines can recover from failures gracefully and continue processing data.
- **Modularity**: Build pipelines as modular components, allowing for easy updates and replacements.
- **Transparency**: Make all data processing steps and transformations visible and auditable.

### Interfaces

- **User-Friendly**: Provide intuitive and accessible interfaces for users to interact with the data platform.
- **Consistency**: Maintain uniformity across different interfaces to streamline user experience and reduce learning curve.
- **Extensibility**: Allow easy integration with other tools and platforms via standardized APIs.

### Engineering

- **Maintainability**: Write clean, well-documented code to simplify maintenance and onboarding of new developers.
- **Testability**: Ensure all components are thoroughly tested, using automated tests to catch issues early.
- **Modularity**: Structure the system in a modular way to allow components to be updated or replaced without affecting the whole system.
- **RESTful APIs**: Expose functionalities through RESTful APIs to standardize interactions and integrations.
- **Separation of Concerns**: Clearly separate storage and compute functions to optimize resource usage and scalability.
- **Encourage Flow**: Fewer systems, put work, code, documentation and tests in the same system.

### Optimization

- **Order of Priority**: People > CPU > RAM > Storage - People are our most expensive resource, optimize for them.
- **Order of Priority**: Skills > Tools - There is more return on investing in people than in tools.

## Quality Indicators

Quality Indicators should have numeric goals, generally ratios - these are provided here without measures to are intended to support next layer of detail and to provide general guidance for quality considerations.

### Availability

- Ensure data can be found and read reliably.
- System components should have high uptime and minimal downtime.

### Latency

- Minimize time to respond to data queries.
- Minimize time between when data is committed and is available to query.

### Freshness

- Establish and enforce staleness thresholds for data to ensure its relevance.

### Correctness

- Ensure that all committed data conforms to predefined schemas.
- Verify that datasets contain accurate and correct record counts.

### Coverage

- Guarantee no unaccounted-for data, either processed successfully or resulting in clear, unambiguous errors.
- Ensure all non-raw data conforms to a schema.
- Implement robust job failure reporting, ensuring even catastrophic errors are eventually reported and logged.

### Durability

- Ensure that committed data remains readable and intact over time.
- Retain all types of data (business data, system logs, and error logs) for specified retention periods, complying with data retention policies.

### Tracibility

- Ensure all data is able to be traced through the system (e.g. this version of the code operated on this version of the data)

## Solution Overview

This diagram shows the primary components needed for this system.

~~~mermaid
flowchart TD
    ESI(Dashboards) --> BQ
    USER(User) --> WB[Analyst Workbench]
    WB --> CATALOG[Data Catalog]
    WB --> BROKER[Data Broker]
    WB --> PIPE[Pipelines]
    PIPE --> BROKER
    subgraph storage
        CATALOG
        BROKER --> DATA[LTS]
        BROKER --> BQ[BigQuery]
    end
    subgraph consumers
        ESI
        USER
    end
~~~

### Primary Components

**Analyst Workbench**

The design and implementation of the workbench is not part of this document, however, the interfaces to build pipelines and interact with data which will be used by the Workbench are part of this design.

**Data Catalog**

Metadata Catalog facilitating 
- Data Governance
- Data Discovery
- Data Security
- Ownership
- Encryption
- Form/Schemas
- Provenance
- Change tracking

> Google DataPlex does not support all features

> Iceberg does not adequately support continuous datasets (log streams), which is anticipated as the main data

**Data Broker**

Data federation, single interface for various data source.

Enforcement point for access permissions and perform decryption for authorized users.

Support SQL/GQL for data access

~~~mermaid
sequenceDiagram
    User->>+Broker: Get data
    Broker->>+Catalog: Where is this data
    Catalog-->>-Broker: It's here +perms +schema
    Broker-->>+Store: Retrive Data
    Store-->>-Broker: Here's the data
    Broker->>-User: Here's your data
~~~

Build on the existing capabilities developed for VAP to provide a low-cost SQL-based data access tool.

Leverage BigQuery for what [it was designed for](https://www.vldb.org/pvldb/vol13/p3461-melnik.pdf) specifically for time sensitive use cases.


> Presto/Trino

> BigQuery is a data broker marketted as a database with impressive capabilities, however it's cost model does not lend itself to a data discovery tool.

**Pipelines**

_development_

- toolset does logging, retries
- unit testable

_operations_

- idempotent (may cost storage)
- backfill/replayable
- pausable
- observable
- step failure doesn't kill entire workflow

> Airflow

> Apache Beam

**Data Storage**

_Operational Data_

Use Google Cloud Storage as the bulk store for data - structured to support querying via external engines.

~~~
dataset\
    data\
        year=YYYY\
            month=MM\
                day=DD\
                    hour=HH\
                        timestamp_mac_process.extention
~~~

Split data into different buckets based on the consumability of the data

- **Raw** [Postel's Law](https://en.wikipedia.org/wiki/Robustness_principle) - keep everything and only reject actively malicious data. This data requires treatment before it can be be used.
- **Managed** - Data which has been normalized to align to a schema which can be consumed into systems.
- **Curated** - Data which has been transformed and is suitable for users.

Data should be hashed with hashes stored in the Data Catalog.

Merkle Trees used to ensure irrefutablability of the provenance of data.

Sensitive data should be encrypted on storage (separate to storage being encrypted)

_Semantic Layer_

It is anticipated that BiqQuery will be used 

### Secondary Components

Just as important as the primary components, but are generally not touchable outside the development and operations teams.

**Management Console**

Design and delivery of a unified management console is key to ensuring high quality system as it simplifies diagnosis and recovery of errors, and improves devops experience by reducing cognitive load and effort to understand and monitor system performance and behaviour.

- performance metrics
    are we tending toward failure, what is the slow part
- alerting
    when there's problems, we should know

**Templates and Libraries**

- code abstractions
    engineers want to access data, not care about where it is hosted
- pipeline libraries
    error handling, logging, performance, all taken care of for data engineers

**Dynamic Configuration**

- dynamic configuration
    configuration should not be baked into code or repositories unless it is specific to that code

## Quality Control

### Data Quality

**Active**

Active checks can block the completion of a job.

- schema validation
- data sensitivity checks (e.g. non highly-conf data with data that looks like secrets)

**Passive**

Passive checks report but do not block completion of a job.

- record counts
- job durations

### Code Quality

The primary gateway for Quality is moving from the uncontrolled environment of the developer workstation into a controlled environment.

**Active**

CI: 
Approach              | Purpose                  | Proposed Implementations     | Goal
--------------------- | ------------------------ | ---------------------------- | -----
Regression Tests      | Function                 | `pytest`                     | 0 failures
Test Coverage         | Regression Test Coverage | `coverage`                   | 90% coverate
Code Typing           | Correctness              | `mypy`                       | 0 errors
Code Form             | Maintainability          | `ruff`, `black`              | 0 errors
Secret Scanning       | Detect Leaked Secrets    | not trufflehog, `fides`      | 0 secrets
Weak Coding Practices | Security                 | `semgrep`, `bandit`, `sonar` | 0 errors
Composition Analysis  | Poor maintenance (*)     | `bombast`, `trivy`           | 0 vulnerable components, 90% currency
Maintainability       | Difficult to read code   | `radon`, `sonar`             | 50 points

CD: Execute jobs writing to a null-writer to exercise everything but the commit

> (*) Composition Analysis has the dual purpose of identifying software with known vulnerabilities [OWASP], but also to identify where currency isn't maintained.

**Passive**

No passive checks

### Security

## Cost Estimation

Current usage of Explorer (June 2024)

**Unique users**: 33  
**Queries**: 1050   
**Average Execution**: 10.51 seconds  
**Median Execution**: 2.20 seconds  
**Processed Bytes**: 0.99 Tb  

Estimates are based on observed performance and anticipated volumes: 

- Cloud Run (*): 100 thousand executions, 8 CPU, 16 Gb, 60 seconds = £1k
- Storage: 1 Petabyte (compressed approx 4/5ths) 200Tb = £4k

Approximately £60k per annum at the upper end of the estimate. 

> (*), this is double the current memory and CPU specification of the comparitor execution environment and 4x the execution time of the existing service, but as prices were rounded up to the nearest £1k it made no difference to the cost estimate.

Anticipated most likely scenario is 10x longer queries due to increased data volumes and fewer queries per month; scaling just the number of queries to 50k/mo, the cost estimate remains the same - primarily due to the small numbers involved and rounding up to the nearest £1k per month.

## Options

### Google Data Stack

Focusng on BigQuery as the largest part of the experience and cost differentiation.

- BigQuery £45k per month (150 Tb interactive querying, 1 Pb processing)
- BigQuery £46k per month (1600 Slots)

Assuming all data in BigQuery as per current MVD.

Approximately £500k per annum at the lower end of the estimate.

This assumes a flat growth of data per query, what is more likely is some datasets will be many terabytes themselves, so rather than queries processing 10s of gigabytes as per Explorer today, some queries will be processing 10s of terabytes. Approximating this to 10 Pb of data accessed in queries is £1.74 million per annum.

### Apache Data Stack

Google essentially resells the Apache Data Stack with BigQuery being their only significant innovation. Using the Apache stack will still require a separate storage/query engine, which is anticipated to be the largest cost differentiator.

Apache does have engines such as Data Fusion

## Security Considerations

**Loss of Confidentiality**

The platform and system should be designed for confidential data, highly confidential data should be handled as extensions on the same platform - e.g. dataset/column encryption.

**Loss of Availability**

Storage should be multiregion, to allow for loss of a data centre.

Storage should be immutable to all service accounts and users without a specific, time-bound, access request.

Compute should be stateless, and where this is not possible, complete loss of state should not result in dataloss outwith appetite.

**Loss of Integrity**



**Concentration Risk**

Serious consideration should be given to if hosting security data on the same environment that it is used to secure is sensible.

**Abuse of Resources**



## Risks

- Infrastructure over Application advocates challenging the approach (risk to quality).
- Short-termism encouraging a buy-over-build, rather than a buy-for-value approach (risk to quality, risk to cost).
- Inability to motivate management to buy-in to an engineering-led future (risk to vision).
- Engineers may be comfortable with their role of assembling rather than building software (risk to vision).
- Technology doesn't respect designs, it will do what someone else coded it to do, complicating delivery (risk to pace).

## Future Work

This design is only concerned with building a platform for growth, it is anticipated that components delivered by this design are below the quality needed, this provides rich opportunity for continued focus on engineering excellence, required to build a world-class engineering team.

It is also not known what the next innovation will be in data engineering, disassociating capabilities from vendors where possible and using only primatives enables us to move at the pace we can move at based on our imagination, ingenuity and business needs rather than following where vendors see suitable return on investment.

## References

[Dremel Paper](https://www.vldb.org/pvldb/vol13/p3461-melnik.pdf)  
[Iceberg Internals](https://www.dremio.com/resources/guides/apache-iceberg-an-architectural-look-under-the-covers/)

[Snowflake Query Optimizer](https://www.youtube.com/watch?v=CPWn1SZUZqE)

