# Korolev

A big data technology stack.

- Metadata Catalog
- Data Reader/Writer
- Console
- Indexing
- Querying
- Integration

## Requirements

- Able to store petabyte-sized datasets
- Reliable and durable storage
- Able to integrate with other storage platforms

## Principals

- The system should be modularized to allow for components to be replaced as needed
- The system should expose functionality as RESTful APIs
- Components should be able to stand-alone
- Storage and Compute are separate

## Table format

How data is stored is the lifeblood of a big data platform. 

Parquet is the current convention for storing data, although is aging as a solution and does not define a table (tables have versions, evolving schemas, and large files are multiple parquet files)

- Iceberg is ill-suited to handling continuous data
- BigQuery as the only solution for storing and accessing data is not cost-conscious

~~~
table/
 |- metadata
 |   |- metadata/
 |   |   +- metadata-0000-0000.json
 |   |- manifests/
 |   |   +- manifest-0000-0000.avro
 |   +- indexes/
 |       +- index-0000-0000.index
 +- data/
     +- year=2000
         +- month=01
             +- day=01
                 +- hour=00
                     +- data-0000-0000.parquet
~~~

## Metadata Catalog

~~~mermaid
flowchart TD
    CATALOG[(Catalog)] --> SNAPSHOT(Metadata)
    SNAPSHOT --> MANIFEST(Manifest)
    SNAPSHOT --> INDEX(Indexes)
    MANIFEST --> DATA(Data Files)

~~~




