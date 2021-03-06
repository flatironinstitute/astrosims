# Simple tabular data query interface

This is a web-based interface for querying large numeric tabular datasets, designed primarily for halo object catalogs from astrophysical simulations.
It is built on Elasticsearch to allow efficient filtering and aggregation of datasets with billions of objects.

## [Python client](py/README.md)

## Server Installation

These are kubernetes definitions for [elastic search](k8s-es.yml) and the [web server](k8s.yml) as used in production.

There is also a [docker-compose](docker-compose.yml) definition for installing everything you need on a single machine.  To use it:

- Edit the [catalog definitions](catalogs) to define the object catalog(s) and fields.  There are a number of examples there already.
- Change `ES_JAVA_OPTS` and `scale` in docker-compose.yml as appropriate for your environment and needs
   - The current settings start 5\*20GB nodes on a single machine (requiring 100+GB memory)
   - You can also setup your own elasticsearch cluster on multiple machines, in which case you only need the flathub service
   - Update [config](config) to point to your elasticsearch cluster if it's not the default
- Run `docker-compose build` to build the application
- Run `docker-compose up -d` to bring up everything (the flathub service will exit with an error -- this is expected)
- Run `docker-compose run --rm flathub -s CATALOG` for each defined catalog to create the databases
- Run `docker-compose run --rm -v /data/dir:/data flathub -i CATALOG /data/FILE ...` to ingest data into each catalog
- Run `docker-compose up -d` to bring up the flathub webserver
