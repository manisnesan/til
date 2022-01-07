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

- Faster Data Annotation with zero shot text classification (see [this](https://rubrix.readthedocs.io/en/stable/tutorials/zeroshot_data_annotation.html) for more info)
  - Install the libraries `pip install rubrix==0.7.0 transformers datasets torch`
 
  - This loads the first 100 examples from test split of news dataset and uses zero shot classifier to predict and log them to rubix 
 ```python
 # Example for Bootstraping data annotation with a zero-shot classifier

# Requires rubrix, transformers, datasets and torch

# Ingredients: zeroshot classifier, news dataset and target categories such as Business, Sports

# 0. Imports
from transformers import pipeline
from datasets import load_dataset
import rubrix as rb

# Load the zero shot pretrained checkpoint
model = pipeline('zero-shot-classification', model='typeform/squeezebert-mnli')

# Load the dataset
load_dataset
dataset = load_dataset('ag_news', split='test[0:100]')

# target categories
labels = 'World Sports Business Sci/Tech'.split()

for record in dataset:
    
    # make predictions
    prediction = model(record['text'], labels)
    
    # log them into a Rubrix dataset
    item = rb.TextClassificationRecord(
        inputs=record['text'],
        prediction=list(zip(prediction['labels'], prediction['scores'])),
    )
    rb.log(item, name='news_zeroshot')
 ```

- Then you can either handlabel or use a bulk-labeling approach (above a certain score, predictions from the model are correct).

## References

- [1][Use Docker Compose with podman to orchestrate containers on fedora](https://fedoramagazine.org/use-docker-compose-with-podman-to-orchestrate-containers-on-fedora/)
- [2] [Podman](https://podman.io/)
- [3] [Rubrix Setup and installation](https://rubrix.readthedocs.io/en/stable/getting_started/setup%26installation.html#setup-and-installation)
