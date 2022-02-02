# Building a python image with openjdk installed

This can be achieved using multistage builds.

COPY --from is used for the multistage build.
```
FROM registry.redhat.io/rhscl/python-38-rhel7:latest
COPY --from=docker.io/openjdk:8 /usr/local/openjdk-8 /usr/local/openjdk-8

USER root
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV JAVA_HOME=/usr/local/openjdk-8/bin/java
RUN update-alternatives --install /usr/bin/java java /usr/local/openjdk-8/bin/java 1

RUN \
    echo $JAVA_HOME && \
    echo "========" && \
    java -version && \
    echo "========" && \
    python -V && \
    echo "========"
```

## Issues faced

- Failing to pull images from docker.io registry with "You have reached your pull rate limit"
```
Trying to pull docker.io/library/openjdk:8...                                                                          
WARN[0030] failed, retrying in 2s ... (1/3). Error: initializing source docker://openjdk:8: reading manifest 8 in docker.io/library/openjdk: toomanyrequests: You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limit
```
The pull rate limits can be increased if the image pull requests are made by Authenticated user on DockerHub   

Authenticate to the registry using the below command
```
# docker login -u <username> -p <password> docker.io
Login Succeeded!
```

## References

- [Pulling image from docker.io fails with "You have reached your pull rate limit" ](https://access.redhat.com/solutions/5603421)
