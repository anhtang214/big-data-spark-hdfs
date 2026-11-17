# Distributed Big Data Analysis with Apache Spark & HDFS

A big data analytics project exploring a large, deeply nested JSON dataset using a distributed processing cluster — built to demonstrate why cloud-native tools (Spark, HDFS) succeed where traditional single-machine computing breaks down.

## Overview

The dataset consists of four JSON files (books, authors, works, and genres) totaling over 10,000 records each, drawn from a larger ~2 million record public dataset. The nested relationships between these entities — a book can have multiple authors and genres — make it a good test case for distributed, parallel data processing.

## Why Not Traditional Computing

- **Memory limits** — exploding nested structures (multiple authors/genres per record) during joins quickly exceeds local memory.
- **No parallelism** — filtering, exploding, and joining are naturally parallelizable, but a local script processes them sequentially.
- **No fault tolerance** — a crash mid-join loses all progress, with no retry mechanism.
- **Poor scalability** — scaling up requires manual hardware/software changes.

## Architecture

- **Apache Spark** — distributed processing engine for reading, cleaning, and flattening the nested JSON (via `explode()`), running Spark SQL joins and aggregations across entities, and computing ratings/popularity statistics.
- **HDFS** — distributed file system (1 namenode + 3 datanodes) storing the datasets across multiple nodes for replication and fault tolerance.
- **YARN** — resource management and job scheduling across the cluster.
- **Docker Compose** — deploys the full cluster (Spark master + 2 workers, HDFS namenode/datanodes, YARN resource/history managers, and a Jupyter Notebook environment) with configurable worker/node counts and a Spark master endpoint for running PySpark interactively.

Deployed locally via Oracle VirtualBox for development; a cost estimate was also produced for an equivalent GCP Dataproc deployment (Sydney region, 3-node cluster) as a cloud-scaling comparison.

## Core Features

- **Data Storage** — load the nested JSON datasets into HDFS.
- **Data Cleaning** — drop redundant columns, flatten nested genre structs into one row per (book, genre), cast ratings/year fields to numeric types, and filter out incomplete records.
- **Data Update** — modify ratings for specific authors and write the updated dataset back to HDFS.
- **Analysis & Visualisation** — interactive exploration of reading trends via Spark SQL, including:
  1. Top genres by average rating and total ratings count
  2. Published-works volume and popularity across decades (1960–2025)
  3. Genre frequency trends over time (1990–2025)
  4. Most popular fiction authors by total ratings sum

## Repository Contents

- `docker-compose.yml` — full cluster definition (Spark master/workers, HDFS namenode/datanodes, YARN, Jupyter)
- `hadoop.env` — HDFS/YARN configuration used by the cluster
- `nbs/analysis.ipynb` — the PySpark analysis notebook

**Data is not included** due to file size. The project uses the public Goodreads books graph dataset (JSON format); place the four source files in HDFS at the paths referenced in the notebook (`hdfs://namenode:9000/...`) before running.

## Running It

```bash
docker-compose up -d
# then open Jupyter at localhost:8888 and run notebooks/analysis.ipynb
```

## Tech

Apache Spark, Spark SQL, HDFS, YARN, Docker & Docker Compose, Jupyter Notebook, PySpark, Pandas, Matplotlib, Plotly.

## Context

Built as a university cloud computing project exploring distributed storage and parallel processing for large-scale nested data.
