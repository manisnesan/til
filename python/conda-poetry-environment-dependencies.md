# Activate a conda environment and install pyproject.toml file inside the conda environment using poetry

To activate a Conda environment and install dependencies from a `pyproject.toml` file using Poetry, follow these steps:

---

## **1. Activate the Conda environment**

```bash
conda activate <ENV_NAME>;  # Replace <ENV_NAME>; with your environment name
```

You can list available environments with `conda info --envs`[^3][^5].

---

## **2. Configure Poetry to use the Conda environment**

Prevent Poetry from creating a new virtual environment and link it to the active Conda environment:

```bash
poetry config virtualenvs.create false
poetry config virtualenvs.path $CONDA_PREFIX  # Points to the active Conda environment
```

This ensures Poetry installs packages directly into the Conda environment[^9].

---

## **3. Install dependencies from pyproject.toml**

Navigate to the project directory containing `pyproject.toml` and run:

```bash
poetry install
```

This installs all dependencies specified in `pyproject.toml` and generates a `poetry.lock` file for reproducibility[^6][^11].

---

## **Optional: Add new packages**

To add additional packages (e.g., `pandas`), use:

```bash
poetry add pandas
```

This updates both `pyproject.toml` and the environment[^11].

---

**Note**: Always use Poetry (not Conda) for package management in this workflow to avoid conflicts between `pyproject.toml` and the environment[^9][^7].

