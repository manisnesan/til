# Setup docker compose to run Rubrix

I wanted to explore [rubrix](https://rubrix.readthedocs.io/en/stable/index.html), a framework to explore, annotate and manage data for NLP. And it required docker-compose as prerequisite to simplify the setup process. Though I have been using docker but never attempted docker-compose, so I attempted to get my hands dirty.

## Steps (tested on fedora 34)

- Remove existing docker installation `sudo dnf remove docker-ce docker-ce-cli containerd.io`
- Optionally remove images, containers, volumes

```bash
$sudo rm -rf /var/lib/docker
$sudo rm -rf /var/lib/containerd
```

- Install podman, docker-compose, podman-docker using dnf. Here we will be using Podman[2], alternative to docker following the steps listed in [1]. podman started supporting docker-compose after > 3.2.

```bash
$sudo dnf install -y podman podman-docker docker-compose
```

- docker-compose requires a unix socket to podman service in order to communicate.

```bash
systemctl --user enable podman.socket
systemctl --user start podman.socket
systemctl --user status podman.socket
export DOCKER_HOST=unix:///run/user/$UID/podman//podman.sock
```

- Verify the service

```bash
$sudo curl -H "Content-Type: application/json" --unix-socket /var/run/docker.sock http://localhost/_ping
```

- Now let's setup rubrix
- Create a conda env using mamba `mamba create -n rubrix python=3.8`. Activate the environment.
- install rubrix `mamba install -c conda-forge rubrix`
- create a folder rubrix `mkdir rubrix && cd rubrix`
- Download the yaml file for docker-compose

```bash
wget -O docker-compose.yml https://raw.githubusercontent.com/recognai/rubrix/master/docker-compose.yaml && docker-compose up -d
```

This contains services such as rubrix, elasticsearch and kibana.

- Create a script `test.py` to log a single record

```python
import rubrix as rb

rb.log(rb.TextClassificationRecord(inputs="My first Rubrix example"), name='example-dataset')
```

- Run the script `python test.py`
- Access the service at `http://localhost:6900` where you can see the example-dataset and the corresponding records. Read more about rubrix concepts [here](https://rubrix.readthedocs.io/en/stable/getting_started/concepts.html).

## References

- [1][Use Docker Compose with podman to orchestrate containers on fedora](https://fedoramagazine.org/use-docker-compose-with-podman-to-orchestrate-containers-on-fedora/)
- [2] [Podman](https://podman.io/)
- [3] [Rubrix Setup and installation](https://rubrix.readthedocs.io/en/stable/getting_started/setup%26installation.html#setup-and-installation)
