# Korolev

Korolev is a big data technology stack.

- Metadata Catalog
- Data Reader/Writer
- Console
- Indexing
- Querying

## Requirements

- Able to store petabyte-sized dataset
- Reliable and durable storage
- Able to integrate with other storage platforms

## Table format

How data is stored is the lifeblood of a big data platform. Parquet is the current convention, although is aging as a solution.

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
    CAT[(Catalog)] --> MET(Metadata)
    MET --> MAN(Manifest)
    MET --> IDX(Indexes)
    MAN --> PAR(Data Files)

~~~




