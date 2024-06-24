# Korolev - Moonshot Data Platform
**Commiting ourselves, before this year is out, of building a high-performance, low-cost petabyte-scale data platform**

## Context


## Goals

- Simplify data sharing and usage.
- Enable easy system modifications.
- Rapidly build the system to deliver quick value.
- Keep the core system low-cost, adding expenses only where it adds value.

## Scope and Purpose

### Purpose of this Document

### Scope

## Problem Space

This is not an exhaustive list of constraints and factors influencing technical direction.

- Able to store petabyte-sized datasets
- Reliable and durable storage
- Able to integrate with other storage platforms
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

- Indexing datasets is not required (neither BigQuery nor Snowflake index)
- The services will be written in Python, version 3.10 (or later) should be targeted.
- The services will be hosted in Containers run in a Kubernetes-like environment.
- The data will be stored on Google Cloud Storage and available to both read and write.
- There will be no access to the running or run workers beyond data outputs and logs.
- Code will be managed on GitHub and functionality on that platform such as locked branches and Actions will be available.


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


## Solution Overview



### Affected Components

## Data Pipelines

## Data Storage


- Metadata Catalog
- Data Reader/Writer
- Console
- Querying
- Storage

## Quality Control

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

## Security Considerations

## Risks




## References

[Dremel Paper](https://www.vldb.org/pvldb/vol13/p3461-melnik.pdf)
[Iceberg Internals](https://www.dremio.com/resources/guides/apache-iceberg-an-architectural-look-under-the-covers/)