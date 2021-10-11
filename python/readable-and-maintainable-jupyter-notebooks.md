# Readable and Maintainable Jupyter Noteboks

## MetaData

- Source: [On writing clean Jupyter notebooks
](https://ploomber.io/posts/clean-nbs)

## Best Practices

- Lock dependencies: requirements.txt and requirements.lock.txt. Alternative is pip-compile with requirements.in and requirements.txt (locked by default)

- Package your project: With setup.py and a bit of change in layout, we can do `pip install --editable .` then 

``` python
# all of these work regardless of the current working directory!
import my_package
from my_package import plot
from my_package.plot import some_plotting_function
```

setup.py

```python
from glob import glob
from os.path import basename, splitext
from setuptools import find_packages, setup

setup(
    name='my_package',
    version='0.1',
    packages=find_packages(where='src'),
    package_dir={'': 'src'},
    py_modules=[splitext(basename(path))[0] for path in glob('src/*.py')],
)
```

project layout

```package
src/
    my_package/
        __init__.py
        plot.py
        process.py
notebooks/
    exploration.ipynb
tests/
    test_process.py
setup.py
```

- Modularize Code:
  - 1) snippets that we call more than once is best to abstracted into functions and call them in the notebook.
  - 2a) if its a single control structure, then leave it in the nb.
  - 2b) if it’s more than one control structure with many lines in its body, it’s best to create a function, even if you’re using it only once, then call it in your notebook.
  - A clean notebook is effectively a series of lines of code with few to no structures of control.

``` python
# converts jupyter notebook into python script
jupyter nbconvert exploration.ipynb --to python
# measures cyclomatic complexity
python -m mccabe --min 3 exploration.py
```

- Be careful with mutable data structures
- Auto-reload code from external modules

```jupyter
%load_ext autoreload
%autoreload 2
```

- Unit Testing: use pytest
  - `pytest tests/ --pdb` # upon failure an interactive debugging session starts
  - start debugging session at an arbitrary line of code `breakpoint()`. Trigger `pytest tests/ -s`
  - start a regular python session at a given line `from IPython import embed; embed()`

```python
import pytest
from my_package import process


@pytest.mark.parametrize(
    'name, expected',
    [
        ['Hemingway, Ernest', 'Ernest Hemingway'],
        ['virginia woolf', 'Virginia Woolf'],
        ['charles dickens   ', 'Charles Dickens'],
    ],
)
def test_clean_name(name, expected):
    # 1. Check process.clean_name('Hemingway, Ernest') == 'Ernest Hemingway'
    # 2. Check process.clean_name('virginia woolf') == 'Virginia Woolf'
    # 3. Check process.clean_name('charles dickens   ') == 'Charles Dickens'
    assert process.clean_name(name) == expected
```

- Organize in sections
  - Import statements
  - Configuration (e.g., open database connections)
  - Data loading
  - Content
    - Markdown header (e.g., # My notebook section)
    - Description. One or two lines summarizing what this section is about
    - Takeaways. A few bullets with the most important learnings of this section
    - Code. Actual program that cleans, analyzes, or plots data.

- Use a code linter such as flake8
- Use a code auto-formatter such as black, yapf
- Write shorter notebooks
  - Different datasets must be in a different notebook
  - When joining datasets, create a new one
  - One notebook for data cleaning, another one for plotting (or feature generation if doing ML)
