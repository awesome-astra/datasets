# Cifar10 images with SqueezeNet 1.1 embeddings

A sample of [CIFAR10](https://www.cs.toronto.edu/~kriz/cifar.html) images equipped with their SqueezeNet 1.1 embedding vectors.

#### Description

A sample of up to 5000 images from the CIFAR10 dataset, processed
with the SqueezeNet embedding model (dimensionality = 1000 components).

The data is ready for ingestion with dsbulk (starting from v1.11 as there are
vector columns). There is a without-vector counterpart of all files, which are
cut at different row counts.

Check a [notebook]() detailing how the data is generated: in particular,

- usage of SqueezeNet for the embeddings;
- additional usage of a Huggingface `pipeline` for image classification, to add finer metadata;
- tweaking (monkeypatching) the `json.dumps` to meet the "non-double-rather-floats" requirements by dsbulk.

#### Specs

- _Header_: yes (for csv)
- _Format_: 20 files: (CSV/JSON, with/out embeddings, {100, 500, 1000, 2000, 5000} rows). All files compressed as a single `.tar.gz` file.
- _Size_: 106 MB (compressed)
- _Records_: 5K
- _License_: the website implies freedom to further use
- _Locally hosted_: no, [download Link](https://drive.google.com/uc?id=1rEbAwyr0_Ki7qTdhpUI6WE6z8F7tpX9M&export=download).
- _Source_: from the Cifar10 site, plus our own manipulation.

https://drive.google.com/file/d/1rEbAwyr0_Ki7qTdhpUI6WE6z8F7tpX9M&export=download

#### Load to Cassandra / Astra DB

**With embeddings**

Table for the "with-embeddings":

```
CREATE TABLE my_keyspace.cifar (
    id text PRIMARY KEY,
    embedding vector<float, 1000>,
    label text,
    metadata map<text, text>
)
```

Loading JSON with dsbulk (v 1.11+):

```
java -jar dsbulk-1.11.0.jar load \
  -k my_keyspace -t cifar \
  -u "token" \
  -p "AstraCS:..." \
  -b "/home/.../secure-connect-db.zip" \
  --dsbulk.connector.json.mode SINGLE_DOCUMENT \
  --connector.json.url cifar10-data-5000.json \
  -c json
```

Loading CSV with dsbulk (v 1.11+):

```
java -jar dsbulk-1.11.0.jar load \
  -k my_keyspace -t cifar \
  -u "token" \
  -p "AstraCS:..." \
  -b "/home/.../secure-connect-db.zip" \
  --connector.csv.url cifar10-data-5000.csv \
  --connector.csv.maxCharsPerColumn 1000000
```

Note the `--connector.csv.maxCharsPerColumn 1000000`, for the very long lines with vectors.

**Without embeddings**

Table for the "without-embeddings":

```
CREATE TABLE my_keyspace.cifar_no_v (
    id text PRIMARY KEY,
    label text,
    metadata map<text, text>
)
```

Loading JSON with dsbulk:

```
java -jar dsbulk-1.11.0.jar load \
  -k my_keyspace -t cifar_no_v \
  -u "token" \
  -p "AstraCS:..." \
  -b "/home/.../secure-connect-db.zip" \
  --dsbulk.connector.json.mode SINGLE_DOCUMENT \
  --connector.json.url cifar10-data-5000-no-embedding.json \
  -c json
```

Loading CSV with dsbulk:

```
java -jar dsbulk-1.11.0.jar load \
  -k my_keyspace -t cifar_no_v \
  -u "token" \
  -p "AstraCS:..." \
  -b "/home/.../secure-connect-db.zip" \
  --connector.csv.url cifar10-data-5000-no-embedding.csv
```

#### References

[Cifar10](https://www.cs.toronto.edu/~kriz/cifar.html)

[SqueezeNet](https://pytorch.org/vision/main/models/generated/torchvision.models.squeezenet1_1.html)
