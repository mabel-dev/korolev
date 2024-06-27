# Korolev - Moonshot Data Platform

**Commiting ourselves, before this year is out, of building a high-performance, low-cost petabyte-scale data platform**

The objective is to build a data platform to perform acquision, peristance, processing and publishing. This system will be a combination of bought and built components.

## Context

The current landscape consists of disparate data systems. Aligning the efforts and expertise from these various systems to achieve common goals can lead to significantly better outcomes. The limitations and flaws in existing systems present an opportunity to build a superior, unified platform.

It should be made clear early that this may exist as parallel systems utilizing common tools and approaches with local embelishments to provide either technical separation or offer unique service offerings within an ecosystem, and this design does not force a single 'platform', rather a single stack with unification points.

## Design Goals

Designing an optimal data platform with the following goals:

- **Encourage Sharing**: Maximize data value through easy analysis and sharing.
- **Engineer Satisfaction**: Enhance engineer motivation to improve customer outcomes.
- **Simplicity and Speed**: Develop simple systems for rapid value delivery.
- **Cost Efficiency**: Maintain low-cost core systems, adding expenses only where they provide value, emphasizing modular design.
- **Reliable**: Reliable performance and behaviour, handling errors and failures.
- **Support Automations**: All components are described in code, all functions are exposed as APIs.

## Scope and Purpose

### Purpose of this Document

The purpose of this document is to describe how the opportunity to create a unified set of tools and common interfaces for the acquision, processing and publishing of data could be realized.

### Scope

## Problem Space

Key constraints and factors influencing technical direction:

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
- Building a simple version in-house is faster and cost-effective than adopting an existing, opinionated service.

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

## Quality Indicators

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

Workbench

**Data Catalog**

Metadata Catalog facilitating 
- Data Discovery
- Data Security
- Ownership
- Encryption
- Form
- Provenance
- Change tracking

**Data Broker**

handles decryption for data in buckets

**Pipelines**

- idempotent (may cost storage)
- backfill/replayable

**Data Storage**

Data pipelines split data into categories:
- raw [Postel's Law](https://en.wikipedia.org/wiki/Robustness_principle) - keep everything and only reject malicious data
- managed
- published

Leverage BigQuery for what [it was designed for](https://www.vldb.org/pvldb/vol13/p3461-melnik.pdf)  

### Secondary Components

Just as important as the primary components, but are generally not touchable outside the development and operations teams.

- performance metrics
    are we tending toward failure, what is the slow part
- alerting
    when there's problems, we should know
- pipeline libraries
    error handling, logging, performance, all taken care of for data engineers
- dynamic configuration
    configuration should not be baked into code or repositories unless it is specific to that code
- code abstractions
    engineers want to access data, not care about where it is hosted

## Quality Control

### Data Quality

**Active**

- schema validation

**Passive**

- record counts
- record durations

### Code Quality

**Active**

- execute jobs writing to a null-writer to exercise everything but the commit

**Passive**

The primary gateway for Quality is moving from the uncontrolled environment of the developer 

Approach              | Purpose                          | Proposed Implementation
--------------------- | -------------------------------- | ----------------------------
Regression Tests      | -                                | `pytest`
Test Coverage         | -                                | -
Code Typing           | -                                | `mypy`
Code Form             | -                                | `ruff`
Secret Scanning       | Detect Leaked Secrets            | not trufflehog, `fides`
Dry Runs              | Test execution before deployment | -
Weak Coding Practices | -                                | -
Composition Analysis  | Poor maintenance                 | `bombast`, `trivy`
Maintainability       | Difficult to read code           | `radon`

### Security

## Cost Estimation

Current usage of Explorer (30 days to 24 June 2024)

**Unique users**: 31  
**Queries**: 1015   
**Average Execution**: 12.29 seconds  
**Median Execution**: 2.25 seconds  
**Processed Bytes**: 1.23 Tb  

Estimates are based on observed performance and anticipated volumes: 

- Cloud Run (*): 100 thousand executions, 8 CPU, 16 Gb, 60 seconds = £1k
- Storage: 1 Petabyte (compressed approx 4/5ths) 200Tb = £4k

Approximately £60k per annum at the upper end of the estimate. 

> (*), this is double the current memory and CPU specification of the comparitor execution environment and 4x the execution time of the existing service, but as prices were rounded up to the nearest £1k it made no difference to the cost estimate.

Anticipated most likely scenario is 10x longer queries to to data volumes and fewer queries per month; scaling just the number of queries to 50k/mo, the cost estimate remains the same - primarily due to the small numbers involved rounding up to £1k per month.

## Options

### Google Data Stack

Focusng on BigQuery as the largest part of the experience and cost differentiation.

- BigQuery £45k per month (150 Tb interactive querying, 1 Pb processing)
- BigQuery £46k per month (1600 Slots)

Assuming all data in BigQuery as per current MVD.

Approximately £500k per annum at the lower end of the estimate.

This assumes a flat growth of data per query, what is more likely is some datasets will be many terabytes themselves, so rather than queries processing 10s of gigabytes as per Explorer today, some queries will be processing 10s of terabytes. Approximating this to 10 Pb of data accessed in queries is £1.740 million per annum.

## Security Considerations

## Risks

- Infrastructure over Application advocates challenging approach.
- Short-termism encouraging a buy-over-build, rather than a buy-for-value approach.

## References

[Dremel Paper](https://www.vldb.org/pvldb/vol13/p3461-melnik.pdf)  
[Iceberg Internals](https://www.dremio.com/resources/guides/apache-iceberg-an-architectural-look-under-the-covers/)

[Snowflake Query Optimizer](https://www.youtube.com/watch?v=CPWn1SZUZqE)

