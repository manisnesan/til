## Setting up VSCode on Google Colab or on Kaggle

- On your google colab or on kaggle, install the colab code package

- Connect to the cpu or gpu runtime appropriately. 

`$ !pip install colabcode` 

- Import the ColabCode class from the package colabcode

```python
from colabcode import ColabCode
ColabCode(port=1000, password='xxxxx'
```

This will return ngrok url and allow you to login to a page. Once you provide the password, code-server instance will be available for you with full fledged editor features.

## References
- VSCode on Google Colab (https://amitness.com/vscode-on-colab/)
- Abishek Thakur [Youtube Tutorial](https://youtu.be/7kTbM3D02jU)

