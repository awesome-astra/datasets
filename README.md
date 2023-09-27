# Datasets

A curated collection of datasets, mostly for repeated usage in demos and integrations with Astra.

![Gliophorus psittacinus (formerly Hygrocybe psittacina)](images/hygrocybe.jpg)

### Usage

We would like to have few shared dataset fit for various purposes (demos & so on).

Quality over quantity: remember one of our goals is for the same few datasets to
be used over and over, whenever suited for the purpose of the demo/tutorial.

If you want to contribute:

- please respect the directory structure;
- take the time to fill the "dataset card" (`README.md` in the dataset directory);
- categorize and describe your contribution in the listing below;
- do not commit large files, host them elsewhere and place a (public) URL instead.

## The sets

### Vector data

_Data including vector embeddings for various usage._

[**Cifar10 images with SqueezeNet 1.1 embeddings**](dataset-archive/cifar10-images-squeezenet/README.md): 5000 images from CIFAR10 with their vector embeddings and a few metadata. _Multiple JSON and CSV, 106 MB compressed_.

### Big tabular data

_Big datasets in tabular format mainly for use with Cassandra (through `dsbulk` and similar)._

[**Uber Eats synthetic data for ML**](dataset-archive/uber-ml-synthetic-data/): Synthetic data modeled after Uber Eats' feature-based ML use case. _Multiple CSVs, 17 GB_.

### Structured mid-size data

_Mid-sized data with a structure going beyond flat tables (e.g. JSON with nested fields, relations, etc)_

**None yet**

### Referential data

_Small, simple general-purpose datasets. Useful for reference tables, listings, collections and the like._

[**Language codes**](dataset-archive/language-codes/): ISO 639.2 two-letter language codes table. _CSV, 2.7 KB_.
