# Generate pip requirements file based on imports

## Problem

- Cleaning the requirements.txt based on the imports actually used.

## pipreqs

- Install pipreqs package `$ pip install pipreqs`
- cd into the directory containing *.py files
- Run `$ pipreqs .`
- The above run will generate requirements.txt file
- If there are issues due to package files stored understand the same directory, then create an dummy directory and move all the *.py files into the directory. Then run the pipreqs inside the dummy directory. 
- Copy the generated requirements.txt file into the parent.
- Move the py files back from dummy directory into the original location. Remove the the dummy directory.
- Finally remove the pipreqs once you are done as this is a utility package. `$ pip uninstall -y pipreqs`
