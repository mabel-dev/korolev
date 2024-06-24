# Korolev - Moonshot Data Platform

**Commiting ourselves, before this year is out, of building a high-performance, low-cost petabyte-scale data platform**

## Context



## Design Goals

- **Encourage Sharing**: Data is valuable when it can be analyzed and shared.
- **Engineer Satisfaction**: Motivated engineers deliver better customer outcomes. Optimize for their experience.
- **Simplicity and Speed**: Build simple systems to deliver value quickly.
- **Cost Efficiency**: Keep core systems low-cost, adding expenses only where they add value, so design for modularity.

## Scope and Purpose

### Purpose of this Document

### Scope

## Problem Space

This is not an exhaustive list of constraints and factors influencing technical direction.

- Able to store and handle petabyte-sized datasets.
- Reliable and durable storage
- Data should be immutable

- Job failure will happen, it needs to be detected and gracefully recovered
- Storage and Compute are going to be the biggest drivers of consumption costs
- Data volumes will be in the order of hundreds of gigabytes
- All actions affecting data must be transparent, traceable and auditable
- Processing of data will be a mix of scheduled, event-based and ad hoc
- The majority of data processing will be exactly the same every day
- It is impractical to pull all data to the platform, at least some data will need to be pushed to the platform
- End-user APIs and dashboards to view data from the platform will be required
= Any business logic should be able to ported to other cloud platforms
- Third-party components, whilst do some heavy lifting, generally come with maintenance obligations and tie-ins


## Assumptions

For the purposes of this document, it is reasonably assumed that:

- Indexing datasets is not required (neither BigQuery nor [Snowflake](https://www.youtube.com/watch?v=CPWn1SZUZqE) index)
- The services will be primarily written in Python, version 3.10 (or later) should be targeted.
- The services will be hosted in Containers run in a Kubernetes-like environment.
- There will be no access to the running or run workers beyond data outputs and logs.
- Code will be managed on GitHub and functionality on that platform such as locked branches and Actions will be available.
- The data will be stored on Google Cloud Storage and available to both read and write.
- Someone else's R&D and opiniated service offering costs more to acquire and work with, than building a simple version of the same thing.

## Principles

### Warehouse

- **Scalability**: Design for growth, ensuring the system can handle increasing volumes of data without significant performance degradation.
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

## Prior Art

Mabel, Flows, Opteryx, Tarchia, Explorer

## Solution Overview



### Affected Components

## Data Pipelines

Data pipelines split data into categories:
- raw [Postel's Law](https://en.wikipedia.org/wiki/Robustness_principle) - keep everything and only reject malicious data
- managed
- published

## Data Storage


- Metadata Catalog
- Data Reader/Writer
- Console
- Querying
- Storage

## Quality Control

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

### Functional



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

£5000 per month
(*), this is double the specification of the existing execution environment and 4x the execution time of the existing service.

## Options

### Google Data Stack

- BigQuery £35k per month (150 Tb querying)
- BigQuery £32k per month (800 Slots)

## Security Considerations

## Risks

- Infrastructure over Application advocates challenging approach


## References

[Dremel Paper](https://www.vldb.org/pvldb/vol13/p3461-melnik.pdf)
[Iceberg Internals](https://www.dremio.com/resources/guides/apache-iceberg-an-architectural-look-under-the-covers/)

[Snowflake Query Optimizer](https://www.youtube.com/watch?v=CPWn1SZUZqE)