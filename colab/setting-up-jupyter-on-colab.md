# Setting up Jupyter Notebook instance on Colab

**Credits to [tezike](https://github.com/tezike) for sharing this tip**

This below snippet needs to added to Google Colab with GPU instance connected.

``` python
from google.colab import drive
drive.mount('/content/drive')

import os
import subprocess
def run_inf_loop():
  while True: ...

def setup_colab(tok=None):
  os.environ['ADDR']='127.0.0.1'
  os.environ['PORT']='6006'
  '''
  #Downloads the file containing the below shell script and run it

  export ADDR="127.0.0.1"
  export PORT="6006"
  curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash
  sudo apt-get install -y nodejs -q
  pip3 install jupyter jupyterlab --upgrade -q
  pip3 install jupyter_contrib_nbextensions && jupyter contrib nbextension install -q
  wget -q -c -nc https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
  unzip -qq -n ngrok-stable-linux-amd64.zip
  '''
  subprocess.call(['wget', 'http://tiny.cc/1234qwer', '-O', 'bash.sh'])
  subprocess.call(['sh', 'bash.sh'])

  !pip3 install -U fastai --upgrade -qqq
  !pip3 install nbdev -qqq

  get_ipython().system_raw(f'./ngrok authtoken {tok} && ./ngrok http 6006 && http --log=stdout 8888 > ngrok.log &')
  ##########################
  #get_ipython().system_raw('./ngrok http 6006 && http --log=stdout 8888 > ngrok.log &') #use this when you don't want token
  #########################
  '''
  ##Downloads the file containing the below shell script and run it

  #perl -pi -e 's|execution_count": null|execution_count": 1|g' course-v4/nbs/*ipynb
  # jupyter labextension install @jupyter-widgets/jupyterlab-manager
  nohup jupyter notebook --no-browser --allow-root --ip="127.0.0.1" --port="6006" &
  python3 -c "import time; time.sleep(5)" &
  '''
  subprocess.call(['wget', 'http://tiny.cc/1234rewq', '-O', 'bash2.sh'])
  subprocess.call(['sh', 'bash2.sh'])
  !curl -s http://localhost:4040/api/tunnels | python3 -c "import sys, json; print(json.load(sys.stdin)['tunnels'][0]['public_url'])"
  run_inf_loop()

```

And then get a token from [ngrok](https://ngrok.com/).

```python
tok=*****************
```

Execute the below cell

```python
setup_colab(tok)
```

This will return a public url for the jupyter instance with GPU instance from colab.

We can use the jupyter notebook interface, terminal to use either google drive as your storage mechanism or use github to clone our public repository and work on them. 