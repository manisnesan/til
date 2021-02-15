# Running solr on docker

## Prerequisites

- [podman](https://podman.io/) ($ sudo dnf install -y podman)

- Check the list of registries podman searches against by running `podman info`. We can confirm docker.io is one of them

```
registries:
  search:
  - registry.fedoraproject.org
  - registry.access.redhat.com
  - registry.centos.org
  - docker.io
```

- Pull the solr image `podman pull solr:8.8`
- Once the image is downloaded, verify by running `podman images`

```
$ podman images     
REPOSITORY                                          TAG     IMAGE ID      CREATED       SIZE
docker.io/library/solr                              8.8     dd3158c52a92  5 days ago    539 MB
```

- Run solr 
`$ podman run -d -p 8983:8983 -e VERBOSE=yes --name my_solr solr:8.8 solr-precreate gettingstarted`

- Check the status `$ podman ps -a`

- Load some examples
`$ podman exec -it my_solr post -c gettingstarted example/exampledocs/manufacturers.xml`

- Validate by running a query using http (should return 11 docs)
`$ http localhost:8983/solr/gettingstarted/select?q=*:* | jq '.response.numFound'`

