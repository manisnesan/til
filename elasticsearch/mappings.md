# Iterative approach in building mapping for elastcisearch application

## Prerequistite

- Dev Tools
- Sample Doc in json

## Steps

- Start with `dynamic mapping` using a single doc with `numeric_detection` & `date detection` turned on.
- Add the sample doc with `POST /tmp_index/_doc`.
- Get the mapping identified by elasticsearch using `GET /tmp_index/_mapping`.
- Update mapping based on values & the desired features. 
- Create a new index with the updated mapping. `POST /tmp_index_mapping/_doc`
- Determine the features required on the search application and iterate on the mapping.

## References

- [GitHub - Understanding Mapping with ElasticSearch and Kibana](https://github.com/LisaHJung/Part-5-Understanding-Mapping-with-Elasticsearch-and-Kibana) - LisaHJung
